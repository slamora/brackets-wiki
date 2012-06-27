* [Download Brackets](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#wiki-howtoget)
* Create a new folder in extensions/user with yourExtensionName, and a main.js file for your extension code.
* For a quick start, you may want to read the [[Simple "Hello World" extension]] or refer to an [existing extension](https://github.com/adobe/brackets/wiki/Brackets-Extensions) (like InlineImageViewer) that is similar to what you want to do.
* If you're working on anything big we recommend you post to the [brackets-dev Google group](http://groups.google.com/group/brackets-dev) or the [#brackets IRC channel on freenode](http://freenode.net) early on so you can get feedback (there may be others working on similar ideas!).

## Development Workflow:

* Open up Brackets, and if it's not already opened, open the brackets/ folder.
* Open a second window (Debug->New Window), which opens a new Brackets instance. You'll use this window for testing your extension. (This is optional: you can use just one window, but it's nice to have separate stable & testing windows in case you end up breaking Brackets in the process of developing your extension).
* In window #2, you can open a different folder on your file system to access other content for testing your extension (e.g. test.html).
* The development workflow is like this:
 * In window #1, edit your extension code.
 * In window #2, reload window (F5/Command-R) to get extension changes and test.
 * To debug problems, open the developer tools for window #2 (Debug->Show Developer Tools). You can use console.log() from the extension code, set breakpoints, etc.
  * Repeat. 

## Extension Architecture:

An extension consists of a main.js (your main module) and any other JS files (other modules). When you use Require in your main module to import your own modules, you get a private copy of modules, so they dont conflict with other modules. To access core Brackets modules, use ```brackets.getModule()```.

You often want an extension to integrate with the UI somehow. If your extension is doing something new from scratch, you can add new menu items or keyboard shortcuts for your new behavior -- see next section. Some Brackets features that already provide a UI are also extensible via feature-specific APIs -- see the section after next.

### <a name="uihooks"></a>Extending the UI generically

See [[Simple "Hello World" extension]] for a code sample.

For any new behavior, first register a Command that implements your behavior, via ```CommandManager.register()```. This just maps a Command id (string) to your handler function. Use package-style naming for your Command id (e.g. ```"myorg.myextension.mycommand"```) to avoid collisions with other extensions.

**Add a menu item:** Get a top-level menu by calling ```Menus.getMenu()``` with one of the ```AppMenuBar``` constants.  Then add a menu item via ```theMenu.addMenuItem()```, linking it to your Command id. The menu item's label will be the string name you gave the Command when it was created.

As a convenience, ```addMenuItem()``` also lets you create a keyboard shortcut for your Command at the same time.

**Add a context menu item:** Get a context menu by calling ```Menus.getContextMenu()``` with one of the ```ContextMenuIds``` constants (currently ```project-context-menu``` or ```editor-context-menu```).  Then add a menu item via ```theContextMenu.addMenuItem()```, linking it to your Command id exactly like a top level menu item. 

**Add a keyboard shortcut:** To add a keyboard shortcut without any related menu item, call ```KeyBindingManager.addBinding()``` directly, linking a shortcut to your Command id.


### <a name="featurehooks"></a>Extending specific Brackets functionality

**Quick Edit:** To create an extension that responds on CTRL+E (like the inline image viewer), use ```EditorManager.registerInlineEditProvider()```. That manager works by iterating through all the possible inline edit providers on CTRL+E and finding which ones actually respond - it has no notion of priority now, so if your extension wants to respond in the same scenario as some other provider, you'll be out of luck. 

**Quick Open:** To interface with the quick open (file open/jump to) interface, use ```QuickOpen.addQuickOpenPlugin()```.

### <a name="tourl"></a>Accessing resources (e.g. images) in your extension

Extensions can get installed at (semi-)arbitrary paths. For example, you might develop your extension in the ```brackets/src/extensions/user/foo``` directory, but a user might install it in ```brackets/src/extensions/default/bar```.

Thankfully, the ```require``` context that's passed in to your extension's ```main.js``` file can help you resolve paths. Just call ```require.toUrl``` with the relative (to your module) path you'd like to make relative to the site root. **IMPORTANT:** Make sure you're using the ```require``` object that was passed to your module, _not_ the global ```require``` object.

For example, if you have ```awesome.jpg``` in your extension's top-level ```foo``` folder, you can do ```require.toUrl('./awesome.jpg')```, and it will return something like ```/extensions/user/foo/awesome.jpg``` when you call it and ```/extensions/default/bar/awesome.jpg``` when your user calls it. The path you give ```toUrl``` should be relative to your extension's top-level folder (yes, subdirectories work), and the URL you get back will be relative to the site root (i.e. it will begin with "/").

### Further reading

For more detail on Brackets internals, see [[Brackets Development How Tos]].

If you're interested in contributing to the core Brackets codebase, see [[How to Hack on Brackets]].