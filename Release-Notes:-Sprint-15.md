What's New in Sprint 15
-----------------------
* **Live Development**
    * [Auto reload in Live Preview](https://trello.com/card/2-auto-reload-w-live-preview/4f90a6d98f77505d7940ce88/636): The page is automatically refreshed when you save changes to any file other than .css (CSS changes are reflected instantly - no need to save). A dot next to the lightning bolt icon indicates pending changes that will lead to an auto-refresh when you save.
* **Overall UI**
    * [Status bar with line/col info, JSLint status & spinner](https://github.com/adobe/brackets/pull/1717)
    * [Resizable search results & JSLint panel](https://github.com/adobe/brackets/pull/1661)
    * [Remember window size/position from last launch](https://github.com/adobe/brackets-shell/pull/123)
* **Performance**
    * [Major editor performance improvements](https://github.com/adobe/brackets/pull/1847): Almost doubles scrolling FPS, cuts 1/3 from keystroke response latency, and fixes freezes in files with very long lines (e.g. minified code).
* **General Code Editing**
    * [Tab size setting](https://trello.com/card/3-tabs-vs-spaces-default-configurable-tab-size/4f90a6d98f77505d7940ce88/472): Click the number in the lower-right of the status bar to choose tab size. This is currently a global setting, across all files and file types. We may refine this in [future](https://trello.com/card/5-tab-default-per-file/4f90a6d98f77505d7940ce88/289) [stories](https://trello.com/card/5-auto-tab-based-on-file/4f90a6d98f77505d7940ce88/290).
    * [Incremental search while typing in the Find bar](https://github.com/adobe/brackets/pull/1781)
    * [Delete Line(s) command](https://github.com/adobe/brackets/pull/1763): Ctrl+Shift+D / Cmd+Shift+D deletes the line the cursor is on (or all lines the selection spans).
    * [Research migration to CodeMirror 3](https://trello.com/card/3-research-codemirror-3-prototype/4f90a6d98f77505d7940ce88/635): See wiki for [notes on the next steps](https://github.com/adobe/brackets/wiki/CodeMirror-v3-integration).
* **HTML Code Editing**
    * [Code hinting for href/src attributes](https://github.com/adobe/brackets/pull/1747): Hints for relative URLs based on files on disk. Press Tab to accept the selected hint for a folder name and drill down to its contents.
    * [Auto-insert closing tags](https://github.com/adobe/CodeMirror2/pull/76)
* **Files & Folders**
    * [New Folder command](https://github.com/adobe/brackets/pull/1719)
    * [Rename command](https://github.com/adobe/brackets/pull/1719)
    * ["Show in File Tree" command](https://github.com/adobe/brackets/pull/1823): Selects and shows the current editor's file in the left-hand file tree.
    * [Remove recent projects dropdown entries](https://github.com/adobe/brackets/pull/1757): Open the dropdown and roll over an item to show "X" button.
* **Localization**
    * [Italian translation](https://github.com/adobe/brackets/pull/1711)
    * [Spanish translation](https://github.com/adobe/brackets/pull/1839)
    * [Brazilian Portuguese translation](https://github.com/adobe/brackets/pull/1660)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-14...sprint-15#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-14...sprint-15#commits_bucket)

UI Changes
----------
**Status bar** - The JSLint star icon has moved from the upper-right to the lower-right, in the new status bar. A gold star means no errors (as before), while a red star (new) means errors are listed, and a gray star (new) means JSLint is not running on the current file. Change tab settings by clicking the "Spaces" and "4" labels in the lower right (the former is the same as toggling Edit > Use Tab Characters).

API Changes
-----------
**EditorManager focusedEditorChange event** - Now reliably triggered whenever focus moves from one Editor to another. See comment at top of EditorManager for details.

**AppInit.appReady() event** - Now occurs after extensions are done loading, instead of before. See [#1854](https://github.com/adobe/brackets/pull/1854).

New/Improved Extensibility APIs
-------------------------------
**Code hinting** - Code hint provider's `handleSelect()` is now passed a 4th argument: a boolean indicating whether the user prefers to close code hints or continue showing suggestions for the next piece of code. True indicates hints should remain closed (user pressed Enter or clicked a suggestion); false indicates more hints should be shown after inserting the suggestion (user pressed Tab).

**Code tokens** - New TokenUtils module with APIs for traversing the tokens in a given editor. See [#1753](https://github.com/adobe/brackets/pull/1753).

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
* [Status bar](https://github.com/adobe/brackets/pull/1717) by [Chema Balsas](https://github.com/jbalsas)
* [Resizable bottom panels (JSLint & search results)](https://github.com/adobe/brackets/pull/1661) (plus follow-up fixes) by [Chema Balsas](https://github.com/jbalsas)
* [Delete Line(s) command](https://github.com/adobe/brackets/pull/1763) by [Mihai Corlan](https://github.com/mcorlan)
* [Add connection checkmark to Live Preview menu item](https://github.com/adobe/brackets/pull/1707) by ["ckarande"](https://github.com/ckarande)
* [Italian translation](https://github.com/adobe/brackets/pull/1711) by [Antonello Pasella](https://github.com/antonellopasella)
* [Spanish translation](https://github.com/adobe/brackets/pull/1839) by [Chema Balsas](https://github.com/jbalsas)
* [Brazilian Portuguese translation](https://github.com/adobe/brackets/pull/1660) by [Massimiliano Giroldi](https://github.com/massimiliano-giroldi), with [followup fixes & updates](https://github.com/adobe/brackets/pull/1824) by [Bruno Cardoso](https://github.com/brucardoso2)
* [Fix #1147: Top of UI is cut off on some smaller-screen Macs](https://github.com/adobe/brackets-shell/pull/125) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Fix #1064: Make URLs line-wrap in various parts of UI](https://github.com/adobe/brackets/pull/1790) by [Jake Stoeffler](https://github.com/JakeStoeffler)
* [Fix #1256: Don't close entire inline editor when a file _some_ of its results is deleted](https://github.com/adobe/brackets/pull/1769) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Fix #1606: Inline editor layout glitch when switching back to file in certain cases](https://github.com/adobe/brackets/pull/1750) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Misc bug fixes](https://github.com/adobe/brackets/pull/1813) by [Jonathan Diehl](https://github.com/jdiehl)
* [Improved log error message for key binding conflicts](https://github.com/adobe/brackets/pull/1851) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Trigger AppInit.appReady() after loading extensions, as originally intended](https://github.com/adobe/brackets/pull/1854) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Fix GotoAgent (disabled by default) to work with paths containing spaces](https://github.com/adobe/brackets/pull/1748) by [Dennis Kehrig](https://github.com/DennisKehrig)

Bugs fixed in Sprint 15
-----------------------
For details on the bugs addressed, please refer to [closed sprint 15 bugs](https://github.com/adobe/brackets/issues?labels=sprint+15&state=closed). A few of the fixed bugs might not be caught by this search query, however.