# Extensions 2 #

**Status** This is a first draft.

Extensions are an important feature for Brackets. They help keep the core small, while allowing developers to easily extend Brackets with the features that they need for their own unique working style.

Brackets has more than 50 extensions now, but that number will grow as:

1. Extensions are easier to discover, install and use
2. Extensions become easier to develop

We've reached a point at which we're going to make extensions easier to discover and install. This is an important inflection point, and we may discover that it is time to move to a new kind of extension. For convenience, I'll call these new extensions "E2".

What do we want and need from an E2?

## Metadata ##

This one is straightforward: for discovery purposes, we need extensions to provide metadata. Extension name and author are good examples. The metadata *could* be kept entirely on the extension registry site. Extension developers would enter the information into a form, either on the site or in an upload interface in Brackets. More common, however, is to use a system like NPM's:

* the developer creates a package.json file
* there is a command line tool and/or Brackets command to upload the extension to the registry that needs nothing more than to know what directory the extension is in

Bespin also had support for "single file plugins". These Bespin plugins were defined by a single .js file that contained the JSON metadata in a format that could be easily pulled out of the file without executing the JS code.

The metadata that is stored in the JSON file will be made available to the E2's JavaScript code, that way the extension author will not need to duplicate any of the information that might appear in there in their JS.

## Dependencies ##

Extension authors will have a lot more freedom if they are able to build extensions that add new features for other extension developers to build on.

For example, imagine an extension that adds an "Outline" capability that shows an outline view of objects defined by the current file. Rather than building such a view to only be used for JavaScript files, for example, the Outline view creator could make the Outline view support extensions so that JavaScript, CSS, LESS and HTML files could all support outlines as long as the user has extensions that support those types.

## Loose Coupling ##

If an E2's metadata expresses the extension's *capabilities*, then other extensions can declare that they depend on certain capabilities rather than specific extensions. Here's an example of how this could work:

1. User installs an E2 that requires the capability "language.coffeescript"
2. There are two extensions that provide "language.coffeescript": one is a generic CoffeeScript extension and another is one that provides support for an async-enhanced CoffeeScript.
3. The user is presented with a choice between the two and selects the preferred one.

I would expect that in *many* cases, there would be a single E2 for a given capability and this would automatically be installed. However, the CoffeeScript example above is realistic (as would be something like an HTML mode with special support for AngularJS).

Loose coupling would go a step farther by providing a global object registry. Rather than loading specific modules from specific extensions to get at a service or piece of data, an E2 would just look up the object it needs.

This kind of setup will make it easier for people to extend Brackets and other E2s in ways in which we could not expect. The drawback? It creates a sort of loose system that could be not unlike the open web: a very active area of E2 development could require feature detection to be resilient for users having different configurations of Brackets.

If this loose coupling proves too onerous, it would be possible to allow E2's to have a stricter level of requirements later on.

## Documentation/Hinting ##

Aleksandr Motsjonov has started an API doc project that would provide nicely formatted, searchable docs for Brackets. However, these docs don't factor in to better code intelligence/hinting and cover the whole Brackets API surface area, not just the main extension points.

Through metadata and a registration system, we can build a layer of useful machine-understandable documentation that can be presented to an E2 developer. This documentation would cover not only core Brackets features, but also features provided by E2s. This will make extending Brackets much easier.

## Restartless ##

No matter how good the "session restoration" feature is, requiring a restart after installing an extension irritates users, especially if there is an extension update available.

Firefox is a great example here. For years, Firefox would check for add-on updates on launch. If it found one, the user would have to restart the browser just as they were trying to start it up. Or, if an update is found while the browser is open, the user would need to restart the browser for that update to take effect (and the same applies to newly installed extensions).

Firefox now offers restartless extensions, as does every editor I can think of.

Making extensions restartless requires significantly different API design. Ideally, almost (if not all) E2s should not need to have special code in place to unload themselves. Declarative APIs for registering E2 features will allow the straightforward reversal of those registrations if the E2 needs to unload. There can also be an imperative hook for features that cannot be unloaded in a straightforward manner.

## Sandbox? ##

Just as the restartless feature has a significant impact on the APIs we provide to E2s, so would the decision to put E2s into a sandbox.

Why would we want to put E2s into a sandbox?

1. security – E2s would *only* have the capabilities we provide them
2. predictable core performance – Firefox performance has often been impacted by non-performant add-ons. Sublime Text 3 has moved its plugins into a separate process. By putting E2s into a sandbox, it may be possible to run that sandbox in a separate thread or process, ensuring that the performance of Brackets core is not impacted by an extension.

The downsides of using a sandbox:

1. possibly more complicated APIs – there would be an asynchronous boundary between the E2 and the rest of Brackets. For most extensions, this would likely not be a problem.
2. possibly slower E2 performance – if data needs to be copied between Brackets core and the E2, that can slow things down. Odds are that this impact can be minimized.
3. considerably more difficult to extend the user interface – most E2s will probably not need to display custom UI, but those that do will need a mechanism for getting that UI displayed and having UI events make it into the E2 code.

The Eclipse Orion project uses iframes as sandboxes for its plugins. Simon Kaegi reports that they are quite happy with this architecture.

Sublime Text 3 uses a sandbox for its plugins. I have read that all plugins reside in a single plugin process, and it's possible that this works by providing a consistent view of the model to all of the plugins and then synchronizing that view with the main process.

One further note: we can design the API around a sandbox without actually implementing the sandbox right away. However, if we start off with APIs that are not sandbox-style, it will be (almost) impossible to switch later on without breaking all of the extensions.

## Lazy Loading? ##

Bespin had the ability to lazily load plugins, and some of the code became considerably more complex as a result. We should decide how much lazy loading (if any) is important to us.

## Orion? ##

If we decide that a sandbox is a desirable feature, that opens the door to four possible levels of collaboration with the Eclipse Orion project:

1. No collaboration: we build our own sandbox technology
2. Protocol collaboration: we use our own code but speak the same protocol between iframes. Additionally, we *could* try to ensure that extensions are as compatible as possible
3. Code collaboration: the Eclipse Orion project is available under a BSD-style license and we can reuse its code directly. This alone does not guarantee extension compatibility.
4. Full compatibility: we use the code and provide the same service registrations as Orion.

There are technical and project ramifications for each of these choices.

## Migration Path ##

From the prototyping I have done, I have found that it is possible to maintain compatibility with our existing extensions while we migrate to E2. Existing extensions would need to be converted to E2 if they want to be a part of the extension manager and repository.

# Implementation #

There is a very early prototype implementation in the dangoor/extensions branch. See below for sample extensions to get a feel for what I'm talking about.

## Non-sandboxed Declarative Style ##

```javascript
/*jslint vars: true, plusplus: true, devel: true, nomen: true, indent: 4, maxerr: 50 */
/*global define, $, CodeMirror, brackets, window */

define(function (require, exports, module) {
    "use strict";
    
    var ExtensionData = brackets.getModule("utils/ExtensionData");
    
    var extensionName;
    
    function alerter() {
        alert("Hi there!");
    }
    
    function removeNewfangled() {
        ExtensionData.unregister(extensionName);
    }
    
    exports.registering = function (register, metadata) {
        register("command", "alert", {
            name: "Alert",
            exec: alerter
        });
        register("menu.item", "alert", {
            name: "Alert",
            menu: "DEBUG_MENU",
            position: "last",
            keybinding: null
        });
        
        register("command", "reverse", {
            name: "Reverse",
            exec: function () {
                var editor = brackets.world.editor;
                var text = editor.getSelectedText();
                text = text.split("").reverse().join("");
                var selection = editor.getSelection();
                editor.document.replaceRange(text, selection.start, selection.end);
            }
        });
        
        register("menu.item", "reverse", {
            name: "Reverse",
            menu: "EDIT_MENU",
            position: "last",
            keybinding: null
        });
        
        extensionName = metadata.extensionName;
        register("command", "remove.me", {
            name: "Remove Newfangled Demo",
            exec: removeNewfangled
        });
        register("menu.item", "remove.me", {
            name: "Remove Alert",
            menu: "DEBUG_MENU",
            position: "last",
            keybinding: null
        });
    };
});
```

## Using Orion's Plugin Manager ##

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8" />
	<title>Reverse Plugin</title>

<script src="plugin.js"></script>
<script>
    /*jslint vars: true, plusplus: true, devel: true, nomen: true, indent: 4, maxerr: 50 */
    /*global define, $, CodeMirror, brackets, window, orion */
    
    "use strict";
    
    window.onload = function () {
        var provider = new orion.PluginProvider();
        var serviceImpl = {
                exec: function () {
                    alert("Oppa Orionstyle");
                }
            };
        var serviceProperties = { id: "orionalert", name: "Orion Alert" };
        provider.registerService("brackets.command", serviceImpl, serviceProperties);
        provider.registerService("brackets.menu.item", {}, {
            id: "orionalert",
            name: "Orion Alert",
            keybinding: null,
            menu: "DEBUG_MENU",
            position: "last"
        });
        
        provider.registerService("brackets.command.editor", {
            exec: function (text) {
                return text.split("").reverse().join("");
            }
        }, {
            id: "orionreverse",
            name: "Orion Reverse"
        });
        
        provider.registerService("brackets.menu.item", {}, {
            id: "orionreverse",
            name: "Orion Reverse",
            menu: "EDIT_MENU",
            position: "last",
            keybinding: null
        });
        
        provider.connect(
			function () {
			},
			function (e) {
				throw e;
			}
        );
    };
</script>
</head>
<body></body>
</html>
```
