<a id="howtoget"></a> Getting Brackets
-----------

Brackets is built with HTML, CSS and JS, and currently runs as a desktop application in a thin native shell so that it can access your local files. **Brackets isn't ready for general use yet**, but if you like living on the bleeding edge, you can find instructions on how to get Brackets running below.

The latest "stable" builds can be downloaded from the [downloads page](https://github.com/adobe/brackets/downloads). **Use the "brackets-sprint-N.zip" links** -- the generic download button at the top will not work!

If you want to download and run the absolutely _latest_ version of the code, you will have to pull it directly from GitHub. There are two steps to this. First you must clone the [adobe/brackets-app](https://github.com/adobe/brackets-app/) repository. Then clone the [brackets](https://github.com/adobe/brackets/) repository and pull all third-party repos that Brackets referenced as submodules.

Here are the commands:

    git clone https://github.com/adobe/brackets-app.git
    cd brackets-app

    git clone https://github.com/adobe/brackets.git
    cd brackets
    git submodule update --init --recursive

Note, you may be asked for your ssh authentication / passphrase a number of times while pulling the submodule. Make sure that you enter it each time (and it does not time out), or else the pull may fail.

**Note:** The brackets submodule SHA in brackets-app is not updated very often. So, to make sure you're getting the latest brackets core code, once your submodules are set up, make sure to do a `git pull` on master in the brackets subfolder. In general, you should keep up to date with the brackets core by pulling it directly instead of relying on `git submodule update` in brackets-app.

Once you have downloaded the code, launch Brackets from the _bin/win_ or _bin/mac_ folder.

Basic usage
-----------

Currently, most of the functionality in Brackets is available from the in-window
menus (not the standard native menus). These will be moved out to the native
menus in the desktop app eventually.

You can open a file from *File > Open* (Ctrl/Cmd-O) in the in-window Brackets 
menu, or open a folder in the file tree on the left using *File > Open Folder*.
It's a good idea to open the root of whatever project you're working on in the 
file tree, since other Brackets features search in the current tree.

Unlike other editors that show open files in tabs, Brackets has a notion of 
the "working set", which is displayed above the file tree. Just clicking on 
files in the file tree doesn't automatically add them to the working set, 
so you can quickly browse through different files without opening them. To 
add a file to the working set, just make an edit in it, or double-click it 
in the file tree.

Brackets currently has color-coding for HTML, JS, CSS, and LESS files. It
doesn't do any code hinting yet.

<a id="inlines"></a>Quick edit
----------

One of the goals of Brackets is to make it easy to make quick edits to
different bits of code without having to jump around between files.

Currently, Brackets has an early implementation of this. If you're in an HTML
file, and you put the cursor inside a class or id attribute or a tag name,
you can hit Ctrl/Cmd-E (for "edit"). Brackets will search the CSS files in the 
file tree for relevant rules, then open up an inline editor embedded in the HTML
file that lets you make quick tweaks to one of the rules.

On the right side of the inline editor, Brackets shows the list of
all rules that might be relevant to the selected tag/class/id. You can switch 
between the rules by clicking on them in the list, or using Alt-Up/Down Arrow.

You can make changes in the inline editor and save them, then close the editor 
by hitting Cmd-E again. Edits you make in the inline editor will be properly
applied in Live Development mode (see below).

Some things to note about this feature:

* Brackets doesn't take the cascade, tag context, etc. into account.
* Brackets doesn't check to see which CSS files are linked into the current HTML 
  file--it searches all files in the file tree.
  
Eventually Brackets could leverage inline editing for lots of other kinds of
things--in addition to showing relevant JS code, you could imagine adding
inline visual tools like gradient and shadow editors for the selected CSS rule.

<a id="livedev"></a>Live development
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

Some limitations of the current implementation:

* It only works with Chrome as the target browser.
* It relies on the remote debugging features in Chrome, which are enabled by
  a command-line flag. If you're already running Chrome when you hit the Go Live
  button, Brackets will ask if you want to restart Chrome with remote debugging
  enabled, and if you say yes, it will go ahead and do it for you.
* Only one HTML file can have a live connection to the browser at a time--if you
  switch to a different HTML file, Brackets will close the original preview and open 
  one for the new file.
* You cannot specify a URL to load the file from.
  
As with quick edit, there are lots of ideas for how to extend this, including
highlighting DOM nodes in the browser from Brackets, clicking on an item in
the browser to jump back to its source code in Brackets, and setting JS breakpoints 
from Brackets.

Unofficial features
-------------------
A number of features, including Find/Replace, Find in Files, and everything
in the Debug menu, aren't "official" features yet. They don't have final UI, 
aren't completely implemented, and might have significant bugs. 

A couple of notes on unofficial features:

* By default, JSLint runs on all JS files and shows its results in a panel
  at the bottom. If your file is clean, you'll see a gold star in the upper
  right corner. JSLint is very picky about formatting. JSLint is very picky 
  about a lot of things. If you want to turn it off, uncheck *Debug > Enable
  JSLint*.
* Ctrl/Cmd-Shift-O brings up a Quick Open field to let you quickly switch
  to another file from the keyboard. You can start typing a filename in the 
  field, then down-arrow or use the mouse to select one of the files that 
  matches. You can also type ":" followed by a number in the field to go to 
  that line in the current file.
* You can run the Brackets unit test suite from the Debug menu.

Keyboard Shortcut Cheat Sheet
-----------------------------
Here are some keyboard shortcuts that are worth knowing. There will be more as features are added.

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
<td>Ctrl/Cmd-Shift-O</td>
<td>Bring up the Quick Open prompt.</td>
</tr>
<tr>
<td>Ctrl/Cmd-Shift-H</td>
<td>Show/hide the sidebar.</td>
</tr>
<tr>
<td>Ctrl/Cmd-Alt-P</td>
<td>Live file preview.</td>
</tr>
</tbody>
</table>