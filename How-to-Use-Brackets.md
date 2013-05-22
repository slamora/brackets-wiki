<a id="nettuts_video"></a> Video Tutorial
-----------

Nettuts+ [did a tutorial video on Brackets](http://net.tutsplus.com/tutorials/tools-and-tips/a-peek-at-brackets/) which makes for a helpful introduction on how to use some of the main features.

<a id="howtoget"></a> Getting Brackets
-----------

Brackets is built with HTML, CSS and JS, and currently runs as a desktop application in a thin native shell so that it can access your local files. **Brackets isn't ready for general use yet**, but if you like living on the bleeding edge, you can find instructions on how to get Brackets running below.

The latest stable builds can be downloaded from the [downloads page](http://download.brackets.io). 

If you want to download and run the absolutely _latest_ version of the code, you will have to pull it directly from GitHub. See [How to Hack on Brackets](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets) for details on pulling the source.

Basic Usage
-----------

You can open a file from **File > Open (Ctrl/Cmd-O)**, or open a folder in the file tree 
on the left using **File > Open Folder**.
It's a good idea to open the root of whatever project you're working on in the 
file tree, since other Brackets features search in the current tree. You can
easily get back to previous projects you've worked on by clicking on the project's
root folder name in the file tree.

Unlike other editors that show open files in tabs, Brackets has a notion of 
the "working set", which is displayed above the file tree. Just clicking on 
files in the file tree doesn't automatically add them to the working set, 
so you can quickly browse through different files without opening them. To 
add a file to the working set, just make an edit in it, or double-click it 
in the file tree.

To change the default indentation for the editor, use the controls on the
right end of the status bar at the bottom of the window. Click the word
"Spaces" or "Tab Size" to switch whether you're using spaces or tabs, and 
change the indentation size by clicking on the number to the right.

<a id="inlines"></a>Quick Edit
----------

One of the goals of Brackets is to make it easy to make quick edits to
different bits of code without having to jump around between files.

Currently, Brackets has an early implementation of this. If you're in an HTML
file, and you put the cursor inside a class or id attribute or a tag name,
you can hit **Ctrl/Cmd-E** (for "edit"). Brackets will search the CSS files in the 
file tree for relevant rules, then open up an inline editor embedded in the HTML
file that lets you make quick tweaks to one of the rules.

On the right side of the inline editor, Brackets shows the list of
all rules that might be relevant to the selected tag/class/id. You can switch 
between the rules by clicking on them in the list, or using Alt-Up/Down Arrow.

You can make changes in the inline editor and save them, then close the editor 
by hitting Cmd-E again. Edits you make in the inline editor will be properly
applied in Live Development mode (see below).

Brackets also has an early implementation of JavaScript support. If you are in
some JavaScript code, you put the cursor in a function call, and hit **Ctrl/Cmd-E**.
Brackets will search the JS files in the file tree for matching functions,
then open up an inline editor embedded in the HTML file that lets you make
quick edits to one of the functions.

Some things to note about this feature:

* Brackets doesn't take the cascade, tag context, object type, etc. into account.
* Brackets doesn't check to see which CSS or JS files are linked into the current HTML 
  file--it searches all files in the file tree.
  
You can also use Quick Edit anywhere a hex color, rgb/rgba(), or hsl/hsla() appears 
to invoke the Color Picker to edit it inline. This includes shortcut swatches for
other colors used in the file.

Eventually Brackets could leverage inline editing for lots of other kinds of
things, like gradient and shadow editors for the selected CSS rule.

<a id="livedev"></a>Live Development
----------------

Another goal of Brackets is to bridge the gap between code editing and in-browser
inspection. Currently, developers use tools like Firebug or the WebKit Web
Inspector to inspect their page, debug problems, and try out quick tweaks to
CSS properties in order to fix layout issues. Once they're done, they then have
to remember what they did and then go back to their editor to fix the problems
in the source code.

With Live Development, the idea is to tie Brackets more closely to the browser, 
so that you can make changes and debug from Brackets itself, and see the results
instantly in the browser.

The initial implementation of this in Brackets is for CSS editing. If you open an 
HTML file, and then click the "lightning bolt" icon on the right side of the toolbar, 
Brackets will open the HTML file in Chrome. If you then make edits to CSS files 
used by that HTML file (either in an inline editor or just by opening up the CSS 
file), your edits will be instantly reflected in Chrome as you type.

If you save an HTML or JS file, it will auto-reload the browser
preview. We're hoping to work on making these work more like true live development
in the future.

While Live Preview is open, putting your cursor in an HTML tag in Brackets will
highlight the matching element in the browser. Putting your cursor in a CSS rule will
highlight all matching HTML elements. Use "File > Live Highlight" to toggle this off.

By default, Brackets previews files using a local http server that is included with Brackets. 
If you want to preview off a server, you can use **File > Project Settings...**. 
In the Base URL field, set the URL on the server that corresponds to the root of your project.

Some limitations of the current implementation:

* It only works with Chrome as the target browser.
* It relies on the remote debugging features in Chrome, which are enabled by
  a command-line flag. If you're already running Chrome when you start
  Live Preview, Brackets will ask if you want to relaunch Chrome with remote
  debugging enabled, and if you say yes, it will go ahead and do it for you.
* Only one HTML file can have a live connection to the browser at a time--if you
  switch to a different HTML file, Brackets will close the original preview and open 
  one for the new file. Note that if you have another tab open, Chrome will not
  shut down and restart between each page.
* Opening the developer tools in Chrome will close the live development connection.
  
As with quick edit, there are lots of ideas for how to extend this, including
highlighting DOM nodes in the browser from Brackets, clicking on an item in
the browser to jump back to its source code in Brackets, and setting JS breakpoints 
from Brackets.

<a id="quickview"></a>Quick View
----------------
Quick View makes it easy to visualize assets and colors in your code. Just hover your
mouse over a color, gradient, or image reference, and a popover will appear showing
a preview. You can toggle this feature on or off in the View menu.

<a id="quickdocs"></a>Quick Docs
----------------
Quick Docs brings documentation from [webplatform.org](http://docs.webplatform.org) 
right into your editor window, in a similar fashion to
Quick Edit. In the current implementation, Quick Docs shows documentation on CSS property names and 
values. To use it, just put your cursor in a property name or value in a CSS file, and hit
**Ctrl/Cmd-K**. To close the docs, hit Escape.

<a id="codehints"></a>Code Hints
----------------
Brackets has initial support for code hints in HTML, CSS, and JavaScript files. Code hints generally pop up automatically, but you can manually bring them up with **Ctrl-Space** (note that this shortcut uses Ctrl even on Mac).
* In HTML, code hints are provided for tag names, attribute names, and attribute values.
* In CSS, code hints are provided for property names and enumerated property values. Code hints 
don't yet work for shorthand properties (e.g. `background`), only for individual properties (e.g.
`background-repeat`).
* In JS, code hints are provided for variables and functions using the [Tern](http://ternjs.net/) engine.
    * RequireJS modules are supported, so referencing a required module will show you its exports.
    * For non-RequireJS files, Brackets looks in source files that are in the same folder as the JS file you're currently editing to try to find hints.
    * Code hints use smart matching, so you can do things like type camel-case initials to see functions that match that (e.g. `gsp` for `getScrollPos`).
    * In cases where Brackets can't determine exactly what hints should be available, it will fall back to a list of heuristic guesses. These guesses are shown in italics.

Unofficial Features
-------------------
A number of features, including Find/Replace, Find in Files, and everything
in the Debug menu, aren't "official" features yet. They don't have final UI, 
aren't completely implemented, and might have significant bugs. 

A couple of notes on unofficial features:

* By default, JSLint runs on all JS files and shows its results in a panel
  at the bottom. If your file is clean, you'll see a gold star in the status
  bar. JSLint is very picky about formatting. JSLint is very picky 
  about a lot of things. If you want to turn it off, uncheck **View > Enable
  JSLint**. This setting is global and remembered across runs.
* **Navigate > Quick Open** (Ctrl/Cmd-Shift-O) brings up a Quick Open field to 
  let you quickly switch to another file from the keyboard. You can start typing 
  a filename in the field, then down-arrow or use the mouse to select one of the 
  files that matches. 
* You can also use Ctrl/Cmd-T to jump to a particular method definition in 
  the current JS file, or Ctrl-G/Cmd-L to jump to a line number.

Extensions
----------
There is a large and growing body of extensions available for Brackets, many
of which were written by community members, that add all kinds of interesting
and useful functionality. You can install an extension using **File > Extension Manager...**
or clicking the "brick" icon on the toolbar, then clicking the **Install from URL...** button
at the bottom. From there, you can click on
**Browse Extensions** to get to the [list of Brackets extensions](https://github.com/adobe/brackets/wiki/Brackets-Extensions), which has further instructions on installation.
You can also remove extensions from the Extension Manager dialog.

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