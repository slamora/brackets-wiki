##Goal

The goal of this exercise was to write a feature as an extension, without modifying core Brackets code, and document the issues and pain points. The extension needs to be functional, but not necessarily "release ready". The main objective was to hit on all of the extensibility points required to write a robust, full-featured extension.

The original goal was to write a single extension: an inline editor for JavaScript functions. In the end, three extensions were written. 

##Inline Editor for JavaScript functions

When in a JavaScript file, put the cursor (or selection) on a function name and press Cmd/Ctrl-E. If the function can be found it is displayed in an inline editor. If multiple functions with the same name are found, they can be navigated in the same way that multiple CSS rules can be navigated.

This extension searches all JavaScript files in your project, regardless of whether they are used or not.

This extension was possible without any changes to core Brackets code. It is checked in to brackets/src/extensions/disabled.

###Issues:

* CSSInlineEditor class is really just a generic inline editor that supports multiple text ranges. This class should be moved from the CSSInlineEditor module and given a generic name. Done: https://github.com/adobe/brackets/issues/768

* We need file watchers! This plugin scans all JavaScript files in your project and extracts all functions that are defined in the file. This function list is cached on a per-file basis in the FileIndexManager structures. In order to see if the cached data is still valid, the timestamp of the cached data is compared against the timestamp of the file on disk <sup><small>1</small></sup>. Simply checking the timestamp is virtually the same speed as reloading and rescanning the entire file. File watchers would significantly improve performance. This item is already in our backlog: https://trello.com/c/zldzEXmk
<br><small>[1] If the file is in memory and dirty, it is always rescanned</small>

* Needed to access the "private" `_codeMirror` property of Editor to determine the mode of the editor (`getOption("mode")`) and to call the `getTokenAt()` function. We need to decide if we want to simply expose the CodeMirror instance or if we want to add more methods to the Editor class. Filed: https://github.com/adobe/brackets/issues/804

* There is no priority for inline editor providers. Let's say I wanted to add an inline editor that supports editing a particular css selector, but fall back to the default inline CSS editor for all other selectors. There is no way to say my editor takes precedence over the default editor. This probably isn't a big deal for now, but will become an issue if lots of inline editors are written. This should be added to the backlog, but I don't consider it a 1.0 requirement.

###Other notes:

* Per-file information was cached on the FileIndexManager fileInfo structure, which is really handy. We should document best practices around this (each plugin should store all of its data on a single object and make sure it has a unique name).


##Inline Image Viewer

If the cursor (or selection) is in a string that is an image filename (basically any string that ends with .jpg, .jpeg, .png, .gif or .svg), the image is displayed in an inline widget.

This extension was possible without any changes to core Brackets code. It is checked in to brackets/src/extensions/disabled.

###Issues

* Uses the `getTokenAt()` method on `_codeMirror` (same as previous extension)

* This inline editor has a file header section that should match the appearance of the file header in MultiRangeInlineEditor. There wasn't a common base class, so I had to duplicate some code. I'm not sure how much abstraction we can really get here since I only wanted the file name, but MultiRangeInlineEditor also has line number. I recommend not doing anything here for now, but let's keep an eye on it to see if more inline editors are running into the same issue.

##Ambiance Theme for Code Editor

This extension adds a menu item to the "Experimental:" section of the Debug menu. When selected, a dark theme is applied to the main code editor and the menu item is checked. 

This extensions was *not* fully possible without changing core Brackets code. The menu item could be added (in a very hacky way), but we have no way to add keyboard shortcuts.

This extension is much more experimental in nature, so I checked it in to my personal repo here: https://github.com/gruehle/AmbianceTheme 

###Issues:

* Need a way to add a keyboard shortcut - both the handler and display in menu (if the command is in a menu)
* Need to have a documented, supported place to put extension menu items.

We probably want theme support built into our core Editor classes. If not, we need the following:
* Need a way to get notified whenever an Editor is created. 
* Need access to all editor instances (there is a private `_instances` var, but no public access)

Built-in theme support added to backlog: https://trello.com/c/y5ed9WKY
 
##General Extensibility Issues

We currently load the "main" module for all extensions. We should support package.json files for defining extension loading points. See http://requirejs.org/docs/api.html#packages for details. Added to backlog: https://trello.com/c/cyKQeDzd