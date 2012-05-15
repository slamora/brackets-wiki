# Development Workflow:
* Download Brackets per [Main Wiki](https://github.com/adobe/brackets/wiki)
* Create a new folder in extensions/user with yourExtensionName, and a main.js file for your extension code. (You may want to copy an existing extension like InlineImageViewer - whatever is most similar to what you want to do.)
* Open up Brackets, and if it's not already opened, open the brackets/ folder.
* Open a new window (Debug->Experimental->New Window), which opens a new Brackets instance. You'll use this window for testing your extension. You generally don't want to use the same window, as you may end up breaking Brackets itself. (But you can if you want)
* In window #2, open a folder on your file system and create a new file for testing your extension (test.html). Write whatever code will help you test the extension.
* The development workflow is like this:
 * In window #1, edit your extension code.
 * In window #2, reload window (Ctrl/Command-R) to get extension changes and test.
 * To debug problems, open the developer tools for window #2. You can use console.log from the extension code, set breakpoints, etc. (If you set a breakpoint, clear it before you close dev tools, otherwise Bracket will die. Bug)
  * Repeat. 

## Extension Architecture:

Your extension consists of a main.js (your main module) and any other JS files (other modules). When you use Require in your main module to import your own modules, you get a private copy of modules, so they dont conflict with other modules. To access core modules, use bracket.getModules().

You often want an extension to integrate with the UI somehow, to respond to user interaction. 

To create an extension that responds on CTRL+E (like the inline image viewer), use EditorManager.registerInlineEditProvider(). That manager works by iterating through all the possible inline edit providers on CTRL+E and finding which ones actually respond - it has no notion of priority now, so if your extension competes with another or with built-in providers, you may be SOL. 

To interface with the quick open (file open/jump to) interface, use QuickOpen.addQuickOpenPlugin(). 