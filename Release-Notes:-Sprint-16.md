_This is a draft!_
-------------------
_This document will not be finalized until the end of Sprint 16 -- approximately November 9._

What's New in Sprint 16
-----------------------
* **Live Development**
    * [URL mapping for Live Preview](https://trello.com/card/3-url-mapping-for-live-development/4f90a6d98f77505d7940ce88/664): Users can specify a base URL for the root folder of a project.
* **General Code Editing**
    * [Color Picker](https://trello.com/card/2-color-selector/4f90a6d98f77505d7940ce88/662): An inline editor for colors in CSS and HTML files. Supports rgba, hex, and hsla.
    * [Find and Replace](https://github.com/adobe/brackets/pull/1914): Search field turns red when no results are found
    * [CodeMirror 3 migration: Critical editing functionality](https://trello.com/card/2-codemirror-3-critical-editing-functionality/4f90a6d98f77505d7940ce88/660)
    * [Status Bar Edit Mode Formatting](https://github.com/adobe/brackets/pull/1923): The edit mode is now capitalized correctly.
* **Files and Folders**
    * [Sort Working Set](https://github.com/adobe/brackets/pull/1999): Sort the working set automatically by type, name or time added.
    * [Reorder Working Set by Drag and Drop ](https://github.com/adobe/brackets/pull/1940): Sort the working set manually by drag and drop.
    * [Working Set Rename and Show In Tree](https://github.com/adobe/brackets/pull/1919): Context menu items are now available in the working set for Rename and Show In Tree
* **Extensions**
    * [Move extensions folder outside application root](https://trello.com/card/3-extensions-outside-application-root/4f90a6d98f77505d7940ce88/659): Extensions are now stored per user instead of in the application bundle. Windows: C:\Users\<user>\AppData\Roaming\Brackets\extensions. Mac: /Users/<user>/Library/Application Support/Brackets/extensions.
* **Developer Workflow**
    * [Automated unit tests](https://trello.com/card/2-automate-unit-tests/4f90a6d98f77505d7940ce88/661) - Unit tests now run automatically on a Mac build server behind the Adobe firewall.
* **Localization**
    * [Japanese Translation](https://github.com/adobe/brackets/pull/1929)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-16...sprint-17#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-16...sprint-17#commits_bucket)

UI Changes
----------

* **Project Settings** - Project settings are available in the File menu as well as the drop down menu (recent projects and Open Folder...) in the project tree. Currently, the only setting available is the Live Preview Base URL.

API Changes
-----------
* [Require 2.1.1](https://github.com/adobe/brackets/pull/1968): For better error handling while loading extensions, we've upgraded from Require 1.0.3 to [2.1.1](https://github.com/jrburke/requirejs/wiki/Upgrading-to-RequireJS-2.1)
* focusedEditorChange has been renamed to activeEditorChange
* CodeHintManager now requires a code hint provider to return true by default in its handleSelect() function. It can return false if the code hint provider wants to keep the hint list open.

New/Improved Extensibility APIs
-------------------------------

Known Issues
------------
* [#1551](https://github.com/adobe/brackets/issues/1551): Changes within an extension (or a unit test) are not reflected by a simple "Debug > Reload Brackets." Workarounds:
    * Quit and re-launch Brackets to pick up the changes.
    * Open Developer Tools, click the gear icon in the lower-right, and select "Disable cache." This setting is remembered, but is only in effect so long as the Developer Tools browser tab remains open.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* _Debug > Show Developer Tools_ opens in a new tab in Chrome, rather than a new window in Brackets. This is a temporary(ish) change due to CEF3. But on the upside, CEF3's developer tools include many updated features!
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight sometimes jiggles when horizontally resizing window.
* [#1473](https://github.com/adobe/brackets/issues/1473): Uninstalling on Windows sometimes does not remove the Start menu shortcut for Brackets.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.


Community contributions to Brackets
-----------------------------------
* [Panel resizing: refactor size prefs, add min size support](https://github.com/adobe/brackets/pull/1899) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #1886: Saving CSS file from inline editor auto-refreshes page](https://github.com/adobe/brackets/pull/1897) by [Jake Stoeffler](https://github.com/JakeStoeffler)
* [Updated German translation](https://github.com/adobe/brackets/pull/1903) by ["J.M."](https://github.com/mynetx)
* [Support Linux in brackets.platform](https://github.com/adobe/brackets/pull/1983 by [pritambaral](https://github.com/pritambaral)
* [Add LIVE_DEVELOPMENT_TROUBLESHOOTING to de locale](https://github.com/adobe/brackets/pull/1989 by ["J.M."](https://github.com/mynetx)
* [Sort Working Set](https://github.com/adobe/brackets/pull/1999) by [TomMalbran](https://github.com/TomMalbran)
* [Reorder Working Set by Drag and Drop ](https://github.com/adobe/brackets/pull/1940) by [TomMalbran](https://github.com/TomMalbran)
* [Fix #1780: Recent project gets dropped from list when cancelling a switch](https://github.com/adobe/brackets/pull/2013) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #1971: Renaming twice a file in the working set shows wrong name](https://github.com/adobe/brackets/pull/1990) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #1548: Resizing sidebar produces strange behavior in projects panel](https://github.com/adobe/brackets/pull/2040) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #389: Inline editor finds false positive animation name](https://github.com/adobe/brackets/pull/1907) by [jeffslofish](https://github.com/jeffslofish)
* [F2 keyboard shortcut for Rename](https://github.com/adobe/brackets/pull/1922) by [zanqi](https://github.com/zanqi)
* [Unit test for reading a drive](https://github.com/adobe/brackets/pull/2038) by [zanqi](https://github.com/zanqi)
* [Fix #1508: Root folders show nothing in recent projects list after the "-" (no path or drive name)](https://github.com/adobe/brackets/pull/1926) by [jeffslofish](https://github.com/jeffslofish)
* [Fix #1916: Menu name inconsistency: Lines vs. Line(s)](https://github.com/adobe/brackets/pull/1928) by [KraigWalker](https://github.com/KraigWalker)
* [Japanese Translation](https://github.com/adobe/brackets/pull/1929) by [shumpei](https://github.com/shumpei)
* [Unit Test for File Rename](https://github.com/adobe/brackets/pull/1939) by [zanqi](https://github.com/zanqi)
* [Fix #1917:Shift or Ctrl sort-of multi selects in project tree](https://github.com/adobe/brackets/pull/1945) by [TomMalbran](https://github.com/TomMalbran)
* [Fix #1172: Comment / Uncomment shortcut doesn't work on numpad](https://github.com/adobe/brackets/pull/1946) by [TomMalbran](https://github.com/TomMalbran)
* [Prepopulate replace input with current editor selection](https://github.com/adobe/brackets/pull/1964) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #104: Close file icon is not displayed until mouse is moved](https://github.com/adobe/brackets/pull/1969) by [jeffslofish](https://github.com/jeffslofish)

Contributions _from_ Brackets
-----------------------------

Bugs fixed in Sprint 16
-----------------------
For details on the bugs addressed, please refer to [closed sprint 16 bugs](https://github.com/adobe/brackets/issues?labels=sprint+16&state=closed). A few of the fixed bugs might not be caught by this search query, however.