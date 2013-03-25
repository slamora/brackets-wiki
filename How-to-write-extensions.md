* [Download Brackets](How to Use Brackets#wiki-howtoget)
* Create a new folder in `src/extensions/dev` with yourExtensionName, and a main.js file for your extension code.
* For a quick start, you may want to read the [[Simple "Hello World" extension]] or refer to an [existing extension](https://github.com/adobe/brackets/wiki/Brackets-Extensions) (like InlineImageViewer) that is similar to what you want to do.
* If you're working on anything big we recommend you post to the [brackets-dev Google group](http://groups.google.com/group/brackets-dev) or the [#brackets IRC channel on freenode](http://freenode.net) early on so you can get feedback (there may be others working on similar ideas!).

## Development Workflow:

* Open up Brackets, and if it's not already opened, open the brackets/ folder.
* Open a second window (Debug->New Window), which opens a new Brackets instance. You'll use this window for testing your extension. (This is optional: you can use just one window, but it's nice to have separate stable & testing windows in case you end up breaking Brackets in the process of developing your extension).
* In window #2, you can open a different folder on your file system to access other content for testing your extension (e.g. test.html).
* The development workflow is like this:
 * In window #1, edit your extension code.
 * In window #2, reload window (F5/Command-R) to get extension changes and test. _Note: if your extension is not reloading correctly, [disable caching in the Chrome Dev Tools](https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/E5iqcD8VqD4)_
 * To debug problems, open the developer tools for window #2 (Debug->Show Developer Tools). You can use console.log() from the extension code, set breakpoints, etc.
  * Repeat. 

## Extension Architecture:

An extension consists of a main.js (your main module) and any other JS files (other modules). When you use Require in your main module to import your own modules, you get a private copy of modules, so they dont conflict with other modules. To access core Brackets modules, use ```brackets.getModule()```.

You often want an extension to integrate with the UI somehow. If your extension is doing something new from scratch, you can add new menu items or keyboard shortcuts for your new behavior -- see next section. Some Brackets features that already provide a UI are also extensible via feature-specific APIs -- see the section after next.

### <a name="uihooks"></a>Adding menu items and keyboard shortcuts

_See [[Simple "Hello World" extension]] for a code sample._

For any new behavior, first register a Command that implements your behavior, via ```CommandManager.register()```. This just maps a Command id (string) to your handler function. Use package-style naming for your Command id (e.g. ```"myorg.myextension.mycommand"```) to avoid collisions with other extensions. (See also: [higher-level overview of command architecture](Brackets Development How Tos#wiki-commands)).

**Add a menu item:** Get a top-level menu by calling ```Menus.getMenu()``` with one of the ```AppMenuBar``` constants (currently ```FILE_MENU```, ```EDIT_MENU```, ```VIEW_MENU```, ```NAVIGATE_MENU``` or ```HELP_MENU```).  Then add a menu item via ```theMenu.addMenuItem()```, linking it to your Command id. The menu item's label will be the string name you gave the Command when it was created.

As a convenience, ```addMenuItem()``` also lets you create a keyboard shortcut for your Command at the same time.

**Add a context menu item:** Get a context menu by calling ```Menus.getContextMenu()``` with one of the ```ContextMenuIds``` constants (currently ```EDITOR_MENU```, ```INLINE_EDITOR_MENU```, ```PROJECT_MENU``` or ```WORKING_SET_MENU```).  Then add a menu item via ```theContextMenu.addMenuItem()```, linking it to your Command id exactly like a top level menu item. 

**Add a menu divider** Get a top level or context menu as explained above.  Then add a menu dividers via ```theMenu.addMenuDivider()```. It will default to the last position currently in the menu.  You have the option of placing it with the position parameter ```first``` and ```last```, which will place the divider accordingly. Additionally, you can set position parameter to ```before``` and ```after```, pass in a Command ID, and place the divider accordingly.  

**Add a keyboard shortcut:** To add a keyboard shortcut without any related menu item, call ```KeyBindingManager.addBinding()``` directly, linking a shortcut to your Command id. Be sure to use the [Brackets Shortcuts](https://github.com/adobe/brackets/wiki/Brackets-Shortcuts) page to see which shortcuts are available and to add the shortcuts that you use to the list.

To decline a keyboard event and allow other parts of Brackets to handle it, make your Command handler return a `$.Promise` that is _already_ rejected at the time you return it. (This is useful if you want to override editing keys like Enter only when the cursor lies in certain places, and allow the default behavior in other cases; or always override a key in the code editor but allow default behavior in simpler textfields). _(Note: requires Sprint 18 or later)_


### <a name="newui"></a>Adding new UI elements

Be sure to follow the [[Extension UI Guidelines]].

**Add UI elements to the DOM:** Official APIs like the Quick Edit provider API above are the only officially supported way to add new UI elements to Brackets. You _could_ just insert new DOM nodes somewhere, but it may break with future updates to Brackets. 

**<a name="addpanel"></a>Add a panel below the editor:** There's no official way to do this yet, so you'll have to insert your own DOM elements.  Tips:
* Be sure you add your panel _above_ the status bar. To ensure this, use `$myPanel.insertBefore("#status-bar")` or better yet `$myPanel.insertAfter(".bottom-panel:last")`.
* To make your panel resizable, use `Resizer.makeResizable()`: it even takes care of remembering the panels size across launches. See Resizer.js for documentation.

**Load a CSS file:** use `ExtensionUtils.loadStyleSheet()`. It returns a Promise you can use to track when the CSS is done loading.
<br>_To avoid accidentally breaking core Brackets UI_, place a CSS class on the root of your UI and make sure _all_ your CSS rules include a descendant selector. E.g. instead of `li { ... }` use `.myExtension li { ... }`.

### <a name="featurehooks"></a>Extending specific Brackets features

**Quick Edit (inline editors):** To create an extension that responds on CTRL+E (like the inline image viewer), use ```EditorManager.registerInlineEditProvider()```. If multiple "providers" all want to respond in a given context, however, the first one wins - there's no notion of priority or cycling through providers yet. 

**Quick Open:** To interface with the quick open (file open/jump to) feature, use ```QuickOpen.addQuickOpenPlugin()```.

**Code Hints:** To create an extension that shows a code hint popup, use `CodeHintManager.registerHintProvider()`. Unlike Quick Edit, these "providers" can have varying priority to resolve conflicts; more specific providers take precedence.

**Syntax Coloring:** Extensions can add new code-coloring "modes" via `LanguageManager.defineLanguage()`. See [[Language Support]] for details.

### <a name="tourl"></a>Accessing resources (e.g. images) in your extension

Extensions can get installed at (semi-)arbitrary paths. For example, you might develop your extension in the ```brackets/src/extensions/dev/foo``` directory, but a user might install it in ```/Users/<user>/Library/Application Settings/Brackets/extensions/user/bar```.

Thankfully, the ```require``` context that's passed in to your extension's ```main.js``` file can help you resolve paths. Just call ```require.toUrl``` with the relative (to your module) path you'd like to make relative to the site root. **IMPORTANT:** Make sure you're using the ```require``` object that was passed to your module, _not_ the global ```require``` object.

For example, if you have ```awesome.jpg``` in your extension's top-level ```foo``` folder, you can do ```require.toUrl('./awesome.jpg')```, and it will return something like ```/extensions/dev/foo/awesome.jpg``` when you call it and ```/Users/<user>/Library/Application Settings/Brackets/extensions/user/bar/awesome.jpg``` when your user calls it. The path you give ```toUrl``` should be relative to your extension's top-level folder (yes, subdirectories work), and the URL you get back will be relative to the site root (i.e. it will begin with "/").

### Further reading

For more detail on Brackets internals, see [[Brackets Development How Tos]].

If you're interested in contributing to the core Brackets codebase, see [[How to Hack on Brackets]].