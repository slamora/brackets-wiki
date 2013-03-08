# Extensions 2 #

Compiled by Kevin Dangoor with feedback from the Brackets team. Feel free to contact me (dangoor@adobe.com) with feedback.

**Status:** This is the second draft.

This document follows the research on [Extension Management](https://github.com/adobe/brackets/wiki/Research:-Extension-Management). We have charted out a general path forward describing how Brackets users will be able to find, install and manage the extensions they use. Making these things easier will likely increase the number of Brackets extensions significantly.

Extension Management covered extension packages and how they are shipped around. In that research, we did not delve into how extensions are written.

## Restartlessness ##

Our product owner has expressed that he would like extensions to be restartless. More specifically, users can install, upgrade, disable, re-enable and remove extensions at will without having to restart Brackets.

We *could* add this capability to Brackets extensions as they exist today (Sprint 21) by defining an extension lifecycle with a hook for removing everything an extension has plugged in. However, by not making automatic deregistration a core feature of our API, we are placing a burden on our extension developers. Adding the "restartless" feature will make writing extensions more difficult and will likely result in extensions that don't fully clean up after themselves.

To do restartless "right" means building out new APIs specifically for extensions.

## Side Effects of a New API ##

If we build a new extension API, we can build it such that we make some attractive side effects possible.

* pieces of Brackets core can be more independent, less coupled, better factored out
* looser coupling will make the transition to the browser easier
* live documentation about the APIs that's available within Brackets
* extensions can themselves be extensible
* poorly written extensions don't break Brackets
* or adversely affect Brackets performance
* we can hide away more of the complexity of working with Node
* it may actually be possible to secure extensions later on if we decide that's valuable (we don't have to do it, but we also don't have to shut the door on it)
* lazy loading? (my experiments to date have not incorporated this trait, but if we decide we want this feature it should be possible to incorporate)

## New Extensions Conceptual Model ##

![Conceptual Model](https://www.evernote.com/shard/s24/sh/e28f2fe4-8ed6-4264-bc35-13e985b6e1dd/d8030dec6889b9c424c551c05c077012/res/8ad1a591-277c-49af-99be-952ef08c030f/skitch.png?resizeSmall&width=832)

To enable all of the features listed in the previous section, extensions will be given a [façade][http://en.wikipedia.org/wiki/Facade_pattern] that allows them to communicate with the core Brackets code, the extensions' own Node code and other extensions. The communication between parts of the system happen via a mediator which offers a shared messaging bus that supports publish/subscribe messaging, request/response messaging and a notion of shared data.

A variation of this architecture is described in Addy Osmani's [Patterns for Large-Scale JavaScript Application Architecture](http://addyosmani.com/largescalejavascript/).

All of the communication between the façade and the mediator is asynchronous and the data is jsonifiable so that it can pass between any boundaries it needs to. A message sent to the bus could be handled by code in CEF, Node, a Web Worker, or the server (for Brackets in the browser).


The "shared data" makes the programming easier in the common cases by using a model like dependency injection where the extension says which piece of data it needs (the selected text, for example) and the framework provides that when a subscriber is called.

Within Brackets core or within an extension, direct communication between modules can work as it always has.

## Show Me Some Code ##

The conceptual model is the key and we can adjust the syntax used to make writing extensions convenient. Even so, concrete examples in code can be a lot easier to understand than a diagram and prose.

What follows is a complete extension that adds a "Reverse" menu item to the Edit menu. It is functional (and hot reloadable) on the [dangoor/extensions2 branch](https://github.com/adobe/brackets/tree/dangoor/extensions2).

```javascript
define(function (require, exports, module) {
    'use strict';
    
    exports.init = function (ext) {
        var command = ext.channel("command");
        var document = ext.channel("document");
        var menu = ext.channel("menu");
        
        // command registration is just a matter of subscribing 
        // to the corresponding event
        // the "data" parameter is the information passed along with
        // the event. The "envelope" contains metadata about the
        // event.
        command.subscribe("reverse", function (data, envelope) {

            // envelope.data contains the information requested
            // via the event subscription options (see below)
            var text = envelope.data["document.selectedText"];
            if (!text) {
                return;
            }
            text = text.split("").reverse().join("");

            // this message results in the document change
            document.publish("replaceSelectedText", text);

        }, {   // start the options for the subscription, this is command metadata
               // and withData is the data that this handler would like to
               // have passed in
            id: "reverse",
            name: "Reverse",
            withData: "document.selectedText"
        });
        
        // publish a message to add the item
        menu.publish("addItem", {
            menu: "edit-menu",
            position: "last",
            command: "reverse"
        });
    };

});
```

If this extension is turned off/removed, the menu item automatically goes away and the command subscription is removed as well.

## What about UI? ##

UI patterns that are easily configured and compartmentalized (such as menu items) are no problem. What about more complicated bits of UI?

My view is that we help extension developers as much as possible, but otherwise they access the DOM and operate as they always have. To make their extensions restartless, anything that isn't explicitly registered with Brackets will have to be manually cleaned up. But, there are many cases that we can handle for the developers.

For example, the Hover Preview extension currently attaches a mouse move listener and watches for what's under the cursor. If Brackets exposed a set of "hover" events, Hover Preview could just listen for those events and then it only needs to worry about placing its UI on the screen and removing that visible UI if it is disabled. Brackets will manage the mouse move listener, adding and removing it as necessary.

## One implementation path ##

This conceptual model is flexible enough to support anything we want to do, and we can adjust the syntax to make things convenient for all common cases we come across. The prototype demonstrates how things can work, but is definitely a prototype... the code should be thrown away.

I see two open questions:

* what do people think of this conceptual model for extensions?
* does lazy loading matter?

Assuming that people do like this model, implementation can proceed with a set of stories like this:

* Implement extensions core
    * publish/subscribe bus
    * request/response capability
    * shared data management
    * new extension lifecycle
* Implement commands, menus