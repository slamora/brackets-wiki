_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 17 -- approximately November 29._

What's New in Sprint 17
-----------------------
* **Live Development**
    * [Highlight HTML elements when cursor in CSS code](https://trello.com/card/3-live-development-highlight-html-elements-in-browser-from-css/4f90a6d98f77505d7940ce88/285): ....
* **Visual Editing**
    * [Inline color picker](https://trello.com/card/3-color-selector/4f90a6d98f77505d7940ce88/662): Use Quick Edit anywhere a hex color, rgb() or hsl() appears to edit it inline. Shows shortcut swatches for all other colors used in the file.
* **Code Editing**
    * [Block comment/uncomment](https://github.com/adobe/brackets/pull/2080): Use Ctrl+Shift+/ (Cmd on Mac) to toggle block comments around the current selection.
    * [More progress on CodeMirror 3 migration](https://trello.com/card/2-codemirror-3-inline-editor-open-edit/4f90a6d98f77505d7940ce88/650): Inline editors (aka Quick Edit) are partially working on the [cmv3 branch](https://github.com/adobe/brackets/compare/master...cmv3). However, they do not have the correct height [yet](https://trello.com/card/codemirror-3-inline-editor-size-vertically/4f90a6d98f77505d7940ce88/651).
* **Search**
    * [Find in Files scoping](https://github.com/adobe/brackets/pull/2084): Search a specific subtree or even a single file: right-click in the folder tree and choose "Find in..."


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-16...sprint-17#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-16...sprint-17#commits_bucket)


UI Changes
----------


API Changes
-----------

New/Improved Extensibility APIs
-------------------------------

Known Issues
------------
* [#1551](https://github.com/adobe/brackets/issues/1551): Changes within an extension (or a unit test) are not reflected by a simple "Debug > Reload Brackets." Workarounds:
    * Quit and re-launch Brackets to pick up the changes.
    * Open Developer Tools, click the gear icon in the lower-right, and select "Disable cache." This setting is remembered, but is only in effect so long as the Developer Tools browser tab remains open.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* _Debug > Show Developer Tools_ opens in a new tab in Chrome, rather than a new window in Brackets.
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight sometimes jiggles when horizontally resizing window.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.


Community contributions to Brackets
-----------------------------------
* [Block comment/uncomment](https://github.com/adobe/brackets/pull/2080) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Visual cue if no quick open results](https://github.com/adobe/brackets/pull/2101) by ["J.M."](https://github.com/mynetx)
* [Fix #2061: No gray JSLint star when Brackets launched with JSLint disabled](https://github.com/adobe/brackets/pull/2099) by [Tomás Malbrán](https://github.com/TomMalbran)

Contributions _from_ Brackets
-----------------------------

Bugs fixed in Sprint 17
-----------------------
For details on the bugs addressed, please refer to [closed sprint 17 bugs](https://github.com/adobe/brackets/issues?labels=sprint+17&state=closed). A few of the fixed bugs might not be caught by this search query, however.