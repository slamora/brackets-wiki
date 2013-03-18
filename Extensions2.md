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

If we build a new extension API, we can build it such that we make some attractive side effects possible, even if they're not implemented in the first round.

* pieces of Brackets core can be more independent/less coupled
* looser coupling will make the transition to the browser easier
* we can offer live documentation about the APIs that are available within Brackets
* extensions can themselves be extensible
* we can hide away more of the complexity of working with Node
* Brackets core can be more protected from poorly written extensions
* we *may* also be able to protect Brackets core code from poorly performing extensions
* it may actually be possible to secure extensions later on if we decide that's valuable (we don't have to do it, but we also don't have to shut the door on it)
* lazy loading? (my experiments to date have not incorporated this trait, but if we decide we want this feature it should be possible to incorporate)

## Traits of a New API ##

If we can agree on the features we want and the basic patterns we wish to apply, we can start working on the extension mechanism and build out the new API iteratively. By starting out talking about the features and basic concepts, we can avoid getting hung up on syntax.

**Declarative**

To allow extensions to be restartless and not require a bunch of extra work on the part of extension authors, extensions will need to register the features they provide in a declarative manner. The declarations should be tied to the extension so that they can be automatically unregistered when the extension is removed (whether it's because the user uninstalled it, disabled it or upgraded it).

Things like commands, menu items and key bindings are already fairly declarative and I have had three different experimental models that have wrapped those APIs and enabled restartlessness.

**Loosely Coupled**

Registering commands today is largely declarative. However, to register a command an extension has to get a direct reference to the CommandManager module. There is a tight coupling between the CommandManager module and the extension. Looking from the other direction the CommandManager has a reference to the command function, but it does not actually know which extension the command function came from (which is part of the reason that it can't be automatically unregistered).

We can make this situation better by putting a *mediator* in between. This is the sort of architecture proposed in Addy Osmani's [Patterns for Large-Scale JavaScript Application Architecture](http://addyosmani.com/largescalejavascript/). That article is long, but worth reading. For convenience, I'll quote from it along the way in this document.

Through the use of a mediator (more on this to come), we can break the direct link between CommandManager and extensions. Beyond helping extensions become unloadable, Osmani cites four architecture questions that are answered by this sort of pattern:

> 1. How much of this architecture is instantly re-usable?
>
> 2. How much do modules depend on other modules in the system?
>
> 3. If specific parts of your application fail, can it still function?
>
> 4. How easily can you test individual modules?

Flipping the questions around, this architecture makes:

* more code reusable
* has fewer direct dependencies between modules
* can make more of the application resilient to failure of other parts
* can make testing easier

He expressed what the architecture is trying to achieve thusly:

> We want a loosely coupled architecture with functionality broken down into independent modules with ideally no inter-module dependencies. Modules speak to the rest of the application when something interesting happens and an intermediate layer interprets and reacts to these messages. 

Note that when Osmani says "modules", we don't have to interpret this as modules in the RequireJS/CommonJS sense. We can think of these modules as independent functional units (Brackets core, extensions, Node code). In his article, Osmani does talk about routing all communication between actual code modules through the mediator (with a façade in between), but I personally think of modules more in terms of "library code" and the mediator as a means to communicate between services that are available in the total system.

## A New Extensions Conceptual Model ##

![Conceptual Model](https://www.evernote.com/shard/s24/sh/5c77ad19-35da-495d-89dc-1182623ffd9f/f4299d5e27042d1d2cebcac861946aa0/res/7a0b75f5-3c0b-46f8-ab92-98046c9aaa23/skitch.png?resizeSmall&width=832)

To enable all of the features listed in the previous section, extensions will be given a [façade][http://en.wikipedia.org/wiki/Facade_pattern] that allows them to communicate with the core Brackets code, the extensions' own Node code and other extensions. The communication between parts of the system happen via a mediator which offers a shared messaging bus that supports publish/subscribe messaging, request/response messaging and a notion of shared data.

Osmani talks about using the façade for security purposes, but for us the façade has value as a way to maintain information about the services an extension uses and provides to assist in cleaning up if the extension goes away.

All of the communication between the façade and the mediator is asynchronous. Ideally, the data would be jsonifiable so that it can pass between any boundaries it needs to. This way, a message sent to the bus could be handled by code in the client side in brackets-shell, Node, a Web Worker, or the server (for Brackets in the browser). However, passing only jsonifiable data across the bus is only a requirement when crossing process boundaries and should not be considered a requirement.

As a concrete example of this, some editor commands could be implemented requiring just the full text of the current file or selection. The command does some manipulation of the text and then sends a replacement back. These kinds of editor commands could be run *anywhere* because the data is easily serialized.

Other editor commands (block comment, for example) use a lot of contextual information about the document and iteratively expand the selection. Implementing those commands with asynchronous, jsonifiable data would be difficult. For these types of commands, I feel that we are better off at this stage simply passing the document object along.

The "shared data" in the model makes the programming easier in the common cases by using something akin to dependency injection where the extension says which piece of data it needs (the selected text, for example) and the framework provides that when a subscriber is called.

Within Brackets core or within an extension, direct communication between modules can work as it always has.

## Show Me Some Code ##

The conceptual model is the key and we can adjust the syntax used to make writing extensions convenient. Even so, concrete examples in code can be a lot easier to understand than a diagram and prose.

What follows is a complete extension that adds a "Reverse" menu item to the Edit menu. The "ext" object is the façade in the conceptual model and the channels send/receive messages on the shared bus. This extension is functional (and hot reloadable) on the [dangoor/extensions2 branch](https://github.com/adobe/brackets/tree/dangoor/extensions2).

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

## Node ##

Extensions (and core Brackets code) that want to run code in Node need to go through a bit of set up ceremony in order to communicate, but the communication is reasonably straightforward after that.

We could bridge the message bus between Node and the client side code. This would make it possible for the extension above to run entirely in Node. Of course, there's no reason for that extension to run in Node, but it's easy to imagine other commands that would be easier to write in Node (a deployment command, for example).

Even better, though, is that an extension could use one communication model between its own client side code, its Node code, Brackets core and even other extensions.

## Why not JSON? ##

If we're using a declarative format for specifying what an extension offers and needs, why not use JSON for the declarations?

Bespin used a JSON file for its plugins in order to support lazy loading (the JSON for every plugin would be loaded, but the code would only be loaded as needed). There are disadvantages, however:

1. No comments
2. No variables
3. No loops
4. No functions (for Bespin, we defined a "pointer" format: path/to/module#functionName. This was key for lazy loading because it told the system what to load)

JSON is an *option*, but it is not necessarily the most convenient for extension writers. I'll note that this is not a theoretical concern. Here is [a block of main.js from Emmet](https://github.com/emmetio/emmet/blob/master/plugins/brackets/main.js):

```javascript

    r("actions").getList().forEach(function(action) {
    if (_.include(skippedActions, action.name))
        return;

    var id = "io.emmet." + action.name;
    var shortcut = keymap[action.name];

    CommandManager.register(action.options.label, id, function() {
        return runAction(action);
    });

    if (!action.options.hidden) {
        menu.addMenuItem(id, shortcut);
    } else if (shortcut) {
        KeyBindingManager.addBinding(id, shortcut);
    }
```

## What about UI? ##

UI patterns that are easily configured and compartmentalized (such as menu items) are no problem. What about more complicated bits of UI?

My view is that we help extension developers as much as possible, but otherwise they access the DOM and operate as they always have. To make their extensions restartless, anything that isn't explicitly registered with Brackets will have to be manually cleaned up. But, there are many cases that we can handle for the developers.

For example, the Hover Preview extension currently attaches a mouse move listener and watches for what's under the cursor. If Brackets exposed a set of "hover" events, Hover Preview could just listen for those events and then it only needs to worry about placing its UI on the screen and removing that visible UI if it is disabled. Brackets will manage the mouse move listener, adding and removing it as necessary.

## Sandboxing? ##

The model presented here allows us to move forward with extensions that are not sandboxed but does not actively shut the door on sandboxing. The more API surface area that we build out that is asynchronous and jsonifiable, the easier it would be to put compliant extensions into a sandbox. But, we will not need to implement that today or burden extension developers with added complexity.

# Conclusion #

What I'm proposing at this point is that we move forward with an API for extensions that offers:

1. a predictable extension lifecycle (so that manual setup and teardown is possible as needed)
2. a declarative mechanism for registering what an extension offers and needs (but this mechanism is in JS)
3. a mediator that break the coupling and provides a consistent model between Brackets core, extensions, Node and future server-based Brackets
4. an extension mechanism core that supports these features with actual extension APIs built iteratively over time
