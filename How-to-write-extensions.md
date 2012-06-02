* [Download Brackets](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#wiki-howtoget)
* Create a new folder in extensions/user with yourExtensionName, and a main.js file for your extension code.
* For a quick start, you may want to read the [[Simple "Hello World" extension]] or refer to an existing extension (like InlineImageViewer) that is similar to what you want to do.
* If you're working on anything big we recommend you post to the [brackets-dev Google group](http://groups.google.com/group/brackets-dev) or the [#brackets IRC channel on freenode](http://freenode.net) early on so you can get feedback (there may be others working on similar ideas!).

## Development Workflow:

* Open up Brackets, and if it's not already opened, open the brackets/ folder.
* Open a second window (Debug->New Window), which opens a new Brackets instance. You'll use this window for testing your extension. You generally don't want to use the same window, as you may end up breaking Brackets itself. (But you can if you want)
* In window #2, you can open a different folder on your file system to access whatever content you need for testing your extension (e.g. test.html).
* The development workflow is like this:
 * In window #1, edit your extension code.
 * In window #2, reload window (Ctrl/Command-R) to get extension changes and test.
 * To debug problems, open the developer tools for window #2 (Debug->Show Developer Tools). You can use console.log() from the extension code, set breakpoints, etc. (If you set a breakpoint, clear it before you close dev tools, otherwise Bracket will die. Bug)
  * Repeat. 

## Extension Architecture:

Your extension consists of a main.js (your main module) and any other JS files (other modules). When you use Require in your main module to import your own modules, you get a private copy of modules, so they dont conflict with other modules. To access core modules, use ```bracket.getModule()```.

You often want an extension to integrate with the UI somehow. If your extension is doing something new from scratch, you can add new menu items or keyboard shortcuts for your new behavior -- see next section. Some Brackets features that already provide a UI are also extensible via a more abstract "headless" API -- see the section after next.

### <a name="uihooks"></a>Extending the UI generically

See [[Simple "Hello World" extension]] for a code sample.

For any new behavior, first register a Command that implements your behavior, via ```CommandManager.register()```. This just maps a Command id (string) to your handler function.

**Add a menu item:** Get a top-level menu by calling ```Menus.getMenu()``` with one of the ```AppMenuBar``` constants.  Then add a menu item via ```theMenu.addMenuItem()```, linking it to your Command id. The menu item's label will be the string name you gave the Command when it was created.

As a convenience, ```addMenuItem()``` also lets you create a keyboard shortcut for your Command at the same time.

**Add a keyboard shortcut:** To add a keyboard shortcut without any related menu item, call ```KeyBindingManager.addBinding()``` directly, linking a shortcut to your Command id.


### <a name="featurehooks"></a>Extending specific Brackets functionality

**Quick Edit:** To create an extension that responds on CTRL+E (like the inline image viewer), use ```EditorManager.registerInlineEditProvider()```. That manager works by iterating through all the possible inline edit providers on CTRL+E and finding which ones actually respond - it has no notion of priority now, so if your extension wants to respond in the same scenario as some other provider, you'll be out of luck. 

**Quick Open:** To interface with the quick open (file open/jump to) interface, use ```QuickOpen.addQuickOpenPlugin()```.