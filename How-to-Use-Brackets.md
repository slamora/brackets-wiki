<a id="howtoget"></a> Getting Brackets
-----------
**[Downloads Brackets here](http://download.brackets.io)** for Mac, Windows and Linux (Debian/Ubuntu). Brackets is built with HTML, CSS and JS, but currently runs as a desktop application in a thin native shell that can access your local files.

Brackets hasn't hit 1.0 yet, but it's getting close - so feel free to give it a spin and let us know what's missing!

Updates are released about twice a month.


The Basics
----------
Initially, Brackets opens a default "Getting Started" project. Follow the instructions in the HTML code for a quick walkthrough of Brackets feaures.

You can open a different folder in the file tree on the left using **File > Open Folder**. Brackets considers this folder your "project": it acts as the scope for various search operations, and some settings are tied to the folder you have open. You can easily switch back to previous projects by clicking on root folder name in the file tree. You can also drag a folder from the OS onto the Brackets window to open it in the file tree, and drag files onto the Brackets window to open them.

Unlike other editors that show open files in tabs, Brackets has a **"Working Files"** list, which is displayed above the file tree. Clicking a file in the file tree just views it, but doesn't add it to the Working Files list - so you can quickly browse through different files without cluttering the list. If you make an edit, the file is automatically added to Working Files. To add a file without editing it, double-click it in the file tree.

### Extensions
In addition to the core features built into Brackets, there is a large and growing community of developers building extensions that add all sorts of useful functionality. You can search for and install extensions using **File > Extension Manager...** (or click the "plugin block" icon on the toolbar).

You can also [browse the available extensions online](https://brackets-registry.aboutweb.com) without installing Brackets first.


Brackets Highlights
-------------------
### <a id="inlines"></a>Quick Edit
Instead of cluttering up your coding environment with lots of panels and icons, the Quick Edit UI in Brackets puts context-specific code and tools inline.

You open Quick Edit's inline editors by pressing **Ctrl/Cmd-E** when your cursor is on certain pieces of code. For example:

* In an HTML file with the cursor inside a `class` or `id` attribute or on the tag name - Quick Edit will show you all the CSS rules in your project that match. You can edit these rules directly inline, without ever leaving the context of the HTML file.
    * When there are multiple rules, you can navigate between them using the list on the right side of the inline editor (or use Alt+Up/Down).
    * You can also create a new rule in any CSS file directly from the inline editor. Click the **New Rule** button or press Ctrl-Alt-N/Cmd-Opt-N.
* In any file with a hex color, rgb/rgba color, or hsl/hsla color - Quick Edit opens an inline color picker for previewing and adjusting the color. The color picker also lists the top most-used colors from other parts of the file for quick access.
* In a JavaScript file with the cursor on a function name - Quick Edit will show you the function's body (even if it's in a different file).
* In a CSS file with the cursor on a `cubic-bezier()` or `steps()` transition timing function - Quick Edit opens a graphical transition curve editor.

A related feature, **Quick Docs**, shows you documentation relevant to your cursor position:

* In a CSS file with the cursor on a CSS property/value - Quick Docs opens documentation from the [Web Platform Docs](http://docs.webplatform.org/) project.

You can open _multiple_ inline editors and docs viewers simultaneously. To close a single inline editor or docs viewer, click the "X" in the upper-left or press Escape while it has focus. To close _all_ inline editors & docs at once, place your cursor back in the main enclosing code editor and press Escape.


### <a id="livedev"></a>Live Preview
Brackets works directly with your browser to push code edits instantly, so your browser preview is always up to date while you're coding - no page reloads needed.

Live Preview also includes support for live editing of HTML. If you open an HTML file, and then click the "lightning bolt" icon on the right side of the toolbar,  Brackets will open the HTML file in Chrome. If you then make edits to CSS files used by that HTML file (either in an inline editor or just by opening up the CSS file), your edits will be instantly reflected in Chrome as you type.  If you make changes to the content or structure of your HTML, those will be instantaneously updated in the browser without the need to refresh.  This includes changes to the style and class tag attributes.

By default, Brackets previews files using a local http server that is included with Brackets. 
If you want to preview off a server, you can use **File > Project Settings...**. 
In the Base URL field, set the URL on the server that corresponds to the root of your project.

Note that live HTML only works with the Brackets built-in server. If you are using your own web server (have a Live Preview Base URL set), only live CSS editing will work.

If you make changes and save a JS file, however, the content in your browser will be refreshed for preview. 

While Live Preview is open, putting your cursor in an HTML tag in Brackets will
highlight the matching element in the browser. Putting your cursor in a CSS rule will
highlight all matching HTML elements. Use "File > Live Highlight" to toggle this off.

Some limitations of the current implementation:

* It only works with Chrome as the target browser.
* It relies on the remote debugging features in Chrome, which are enabled by
  a command-line flag. On the Mac, if you're already running Chrome when you start
  Live Preview, Brackets will ask if you want to relaunch Chrome with remote
  debugging enabled, and if you say yes, it will go ahead and do it for you.
* Only one HTML file can have a live connection to the browser at a time--if you
  switch to a different HTML file, Brackets will close the original preview and open 
  one for the new file. Note that if you have another tab open, Chrome will not
  shut down and restart between each page.
* Opening the developer tools in Chrome will close the live development connection.
  
As with Quick Edit, there are lots of ideas for how to extend this, including
clicking on an item in the browser to jump back to its source code in Brackets, 
and setting JS breakpoints from Brackets.

### <a id="quickview"></a>Quick View
Quick View makes it easy to visualize assets and colors in your code. Just hover your
mouse over a color, gradient, or image reference, and a popover will appear showing
a preview. You can disable this feature in the View menu.


Other Features
--------------

### Settings
* **Indentation and tabs** - To change the default indentation for the editor, use the controls on the right end of the status bar at the bottom of the window. Click the word "Spaces" or "Tab Size" to switch whether you're using spaces or tabs, and change the indentation size by clicking on the number to the right.
* **Editor font and colors** - There are no official preferences for these yet. However, there are unofficial ways to [[Customize your code font]], and several extensions add the ability to choose different themes.

### Quick Open
To quickly jump to a file, press **Ctrl/Cmd-Shift-O** and type part of the filename. You can type abbreviations or other non-contiguous parts of the name, and Quick Open will intelligently find the best matching file.

### Quick Find Definition
To quickly jump around _within_ a file, press **Ctrl/Cmd-T** to see an outline view - functions in a JS file, selectors in a CSS file, etc. Similar to Quick Open, you can type parts of a name to filter the list.

### <a id="codehints"></a>Code Hints
Code hints generally pop up automatically while you're typing, but you can also manually display them with **Ctrl-Space** (note that this shortcut uses Ctrl even on Mac).

Code hints are provided by default in a number of places:
* HTML - tag names, attribute names, attribute values, and `&` entities.
* CSS - all property names, and enumerated property values (those where the value is a discrete list of keywords).
    * Code hints don't yet work for shorthand properties (e.g. `background`), only for individual properties (e.g. `background-repeat`).
* JS - variables and functions, using the [Tern](http://ternjs.net/) code intelligence engine.
    * Tern makes intelligent inferences about what properties and methods a given object contains, based on an analysis of your code. In addition to the current file, Brackets looks at other files in the same folder and any files referenced by a `require()` statement.
    * In cases where Brackets can't determine exactly what hints should be available, it will fall back to a list of heuristic guesses. These guesses are shown in italics.
    * JS code hints use smart matching - so you can type camel-case initials and other shorthand to filter the hint list more quickly (e.g. type "gsp" for `getScrollPos`).
    * You also get _argument hints_ - while you're typing arguments to a function, an indicator above the cursor lists the expected types of the arguments. Normally this appears automatically, but you can also display it manually by pressing **Ctrl-Shift-Space**. (Nothing is shown if Tern is unsure where the function is defined, however).

### JSLint

By default, Brackets runs [JSLint](http://www.jslint.com/lint.html) on JS files when you initially open them and whenever you save changes. If JSLint find problems, the results are shown in a panel at the bottom. If your file is clean, you'll see a green checkmark in the status bar instead.

JSLint is very picky about formatting. JSLint is very picky about a lot of things. You can hide the JSLint results panel by clicking the close box at the top (the status bar icon will still indicate if JSLint has found problems or not), or you can turn off JSLint completely by unchecking **View > Enable JSLint**.

Various extensions are available that replace JSLint with a different linting tool.


Keyboard Shortcut Cheat Sheet
-----------------------------
Here are some keyboard shortcuts that are worth knowing. Also see the
[Brackets Shortcut wiki page](https://github.com/adobe/brackets/wiki/Brackets-Shortcuts)
for a more complete list of shortcuts.

<table>
<tbody>
<tr>
<td>Ctrl/Cmd-E</td>
<td>Open/close the inline editor.</td>
</tr>
<tr>
<td>Alt-Up/Down Arrow</td>
<td>Switch between rules in the inline editor.</td>
</tr>
<tr>
<td>Ctrl/Cmd-K</td>
<td>Open Quick Docs.</td>
</tr>
<tr>
<td>Ctrl-Space</td>
<td>Bring up code hints, if applicable.</td>
</tr>
<tr>
<td>Ctrl/Cmd-Shift-O</td>
<td>Bring up the Quick Open prompt.</td>
</tr>
<tr>
<td>Ctrl/Cmd-G</td>
<td>Go to a line in the current file.</td>
</tr>
<tr>
<td>Ctrl/Cmd-T</td>
<td>Go to a method in the current file.</td>
</tr>
<tr>
<td>Ctrl/Cmd-Shift-H</td>
<td>Show/hide the sidebar.</td>
</tr>
<tr>
<td>Ctrl/Cmd-Alt-P</td>
<td>Live preview.</td>
</tr>
</tbody>
</table>


What's Next?
------------

For more info on Brackets, check out these resources:
* [How to contribute to the Brackets project](https://github.com/adobe/brackets/blob/master/CONTRIBUTING.md), including filing bugs and editing the source code
* [How to write an extension](https://github.com/adobe/brackets/wiki/How-to-write-extensions)
* Join the community!
    * [@brackets on Twitter](https://twitter.com/brackets)
    * [#brackets on Freenode IRC](http://webchat.freenode.net/?channels=brackets)
    * [Discussion forum](http://groups.google.com/group/brackets-dev)
* [Brackets blog](http://blog.brackets.io/)
* [Troubleshooting for common issues](Troubleshooting)
* Watch our [GitHub activity stream](https://github.com/adobe/brackets/pulse) or [latest release notes](https://github.com/adobe/brackets/wiki/Release-Notes) to see what's in the works for the next update!