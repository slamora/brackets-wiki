_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 0.43 -- approximately September 3rd._

What's New in Release 0.43
--------------------------
* **SCSS/LESS Editing**
    * **[Quick Edit, Quick Find Definition, & Live Preview Highlight support](https://trello.com/c/KoK6kPtp/1190-m-simple-support-for-less-scss-in-key-brackets-features)**: All these features now work the same for SCSS & LESS as they do for plain CSS.
    * Live Preview supports SCSS & LESS editing if you use a third-party "file watcher" to automatically recompile your CSS on save. Brackets notices the changed CSS file and updates the page (without reloading). You can also use a Brackets extension such as [Brackets SASS](https://github.com/jasonsanjose/brackets-sass) or [LESS AutoCompile](https://github.com/jdiehl/brackets-less-autocompile) for this.
* **Themes**
    * [Improved 'dark mode'](https://github.com/adobe/brackets/pull/8731): Dark themes now cause the overall Brackets UI to become darker, not just the editor area.
    * [Separate Extension Manager tab](https://github.com/adobe/brackets/pull/8759): Themes are listed separately in Extension Manager so they're easier to find, and the list of extensions stays cleaner.
* **Search**
    * [Find/Replace: indicate current match index](https://trello.com/c/0GXqt5GF/1089-s-find-next-indicate-current-match-index): The Find/Replace bar now shows "3 of 28" instead of just "28."
* **File Types**
    * [Change file extension -> language/syntax mode mapping](https://github.com/adobe/brackets/pull/8444): After changing a file's language using the status bar switcher, you can save the change for _all_ files with that extension by choosing "Set as Default" from the same dropdown. (This was previously accessible only by manually editing preferences).
* **Live Preview**
    * Support for .xhtml files.
    * Improved reliability when using [iframes](https://github.com/adobe/brackets/pull/8144) or [files not listed in "Working Files"](https://github.com/adobe/brackets/pull/8605).
* **Ongoing Research** (not available yet)
    * [Split view](https://trello.com/c/atD9BEDl/1281-m-splitview-implement-mainviewmanager-code-for-1x2-editors) (early implementation on branch)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-0.42...release-0.43#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-0.42...release-0.43#commits_bucket)


UI Changes
----------
**Keyboard shortcut changes**

* _Replace in Files_ changed to Ctrl-Shift-H (Win) or Cmd-Alt-Shift-F (Mac), to parallel the single-file Replace shortcut better.
* _Show/Hide Sidebar_ changed to Ctrl-Alt-H (Win) or Cmd-Shift-H (Mac).

**Linux: Edit menu** - The _Edit > Cut/Copy/Paste_ menu items are removed on Linux, because they did not work. These menu items will be restored once [a native menu bar](https://trello.com/c/WMB6vtwO/893-linux-native-menus) is implemented. Keyboard shortcuts for cut/copy/paste still work.


API Changes
-----------
**Theme authoring** - Simplified how Find/Replace highlight colors are set. [Read more...](https://github.com/adobe/brackets/wiki/Creating-Themes#theme-styles)

**window.open()** - Only permits 'file://' URLs and 'about:blank', since brackets-shell is not a secure general-purpose web browser. Use `NativeApp.openURLInDefaultBrowser()` to open webpages in the user's regular browser.


Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
TBA

#### Pulling source code from Git
* **TODO** - brackets-shell required?
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 0.43
--------------------------
For details on the bugs addressed, please refer to [closed Release 0.43 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+0.43%22). Not all fixed bugs will be caught by this search query, however.