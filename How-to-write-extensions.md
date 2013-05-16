* [Download Brackets](How to Use Brackets#wiki-howtoget)
* Open your extensions folder by selecting Show Extensions Folder from the Help menu in Brackets
* Create a new "yourExtensionName" folder in the `user` folder in your extensions folder, and inside create a main.js file for your extension code.
* For a quick start, you can paste in the [[Simple "Hello World" extension]] or the code from an [existing extension](Brackets-Extensions) that is similar to what you want to do.
* If you're working on anything big we recommend you post to the [brackets-dev Google group](http://groups.google.com/group/brackets-dev) or the [#brackets IRC channel on freenode](http://freenode.net) early on so you can get feedback (there may be others working on similar ideas!).

(Note: you may also [clone Brackets source from git](How-to-Hack-on-Brackets#wiki-getcode). In that case, refer to the `src` folder instead of `www`).

## Simple Development Workflow

* Launch Brackets and use "File > Open Folder" to open the Brackets `www` folder.
* Open your main.js file and make changes.
* Save the file and restart Brackets via "Debug > Reload Brackets" to see your changes.
* To debug problems, use "Debug > Show Developer Tools" to open developer tools in a tab in Chrome. You can use console.log() from your extension code, set breakpoints, etc.
    * _The first time you open Developer Tools, you must [disable caching](https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/E5iqcD8VqD4)_ - otherwise using Reload while dev tools are open will not reflect changes to your extension.

See **[[Debugging Brackets]]** for a more robust two-window workflow.

## Extension Architecture

An extension consists of a main.js (your main module) and any other files in its subtree (which may include other JS modules). You can use Require to load modules from your extension's folder tree or modules from core Brackets (to load core Brackets modules, use ```brackets.getModule()``` instead of `require()`). You cannot load modules from _other_ extensions.

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

_**This is unofficial API**_ - adding UI elements directly through the DOM works, but puts you on shaky ground. Code that does this _will_ break as Brackets updates evolve the UI. _The only official way to extend the Brackets UI is through JavaScript APIs_, such as the Menu interface above or the Quick Edit interface below.

However, following these best practices will ensure your code behaves as nicely as it possibly can under the circumstances:

**<a name="addpanel"></a>Add a panel below the editor:** Use the CSS class `.bottom-panel`; see the JSLint bottom-panel.html for an example. Add your panel _above_ the status bar using `$myPanel.insertBefore("#status-bar")` or better yet `$myPanel.insertAfter(".bottom-panel:last")`. To make your panel resizable, use `Resizer.makeResizable()`: it even takes care of remembering the panel's size across launches. See Resizer.js for documentation.

**Add a toolbar icon:** Use `$myIcon.appendTo($("#main-toolbar .buttons"))`.

**Add a top panel/toolbar:** Use `$myPanel.insertBefore("#editor-holder")`.

**UI design:** Be sure to follow the [[Extension UI Guidelines]].

**Load a CSS file:** use `ExtensionUtils.loadStyleSheet()`. It returns a Promise you can use to track when the CSS is done loading.
<br>_To avoid accidentally breaking core Brackets UI_, place a CSS class on the root of your UI and make sure _all_ your CSS rules include a descendant selector. E.g. instead of `li { ... }` use `.myExtension li { ... }`.

### <a name="featurehooks"></a>Extending specific Brackets features

**Quick Edit (inline editors):** To create an extension that responds on Ctrl+E (like the inline color picker), use `EditorManager.registerInlineEditProvider()`. If multiple "providers" all want to respond in a given context, however, the first one wins - there's no notion of priority or cycling through providers yet. 

**Quick Docs:** Similar to Quick Edit, but register your provider with `EditorManager.registerInlineDocsProvider()` instead.

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

## Publishing Extensions

See the [Extension Package Format](https://github.com/adobe/brackets/wiki/Extension-package-format) for information on preparing your extension for sharing with others.

For the time being, publish your extension by linking to its zip file or GitHub repository here from the [extensions list](https://github.com/adobe/brackets/wiki/Brackets-Extensions).