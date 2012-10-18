_This is a draft!_
-------------------
_This document will not be finalized until the end of Sprint 15 -- approximately October 18._


What's New in Sprint 15
-----------------------
* **Live Development**
    * [Auto reload w/ Live Preview](https://trello.com/card/2-auto-reload-w-live-preview/4f90a6d98f77505d7940ce88/636): ...
* **Overall UI**
    * [Status bar with line/col info, JSLint status & spinner](https://github.com/adobe/brackets/pull/1717)
    * [Resizable search results & JSLint panel](https://github.com/adobe/brackets/pull/1661)
* **Code Editing**
    * [Tab size setting with cleaner UI](https://trello.com/card/3-tabs-vs-spaces-default-configurable-tab-size/4f90a6d98f77505d7940ce88/472): ...
    * [Major performance improvements in editor scrolling, typing, etc.](https://github.com/adobe/brackets/pull/1847)
    * [Incremental search while typing in the Find bar](https://github.com/adobe/brackets/pull/1781)
    * [Delete Line(s) command](https://github.com/adobe/brackets/pull/1763)
    * [Research CodeMirror 3 integration](https://trello.com/card/3-research-codemirror-3-prototype/4f90a6d98f77505d7940ce88/635): ...
* **Code Hinting**
    * [Code hinting for HTML href/src attributes](https://github.com/adobe/brackets/pull/1747): Hints for relative URLs based on files on disk.
* **Files & Folders**
    * [New Folder command](https://github.com/adobe/brackets/pull/1719)
    * [Rename command](https://github.com/adobe/brackets/pull/1719)
    * ["Show in File Tree" command](https://github.com/adobe/brackets/pull/1823)
    * [Button to remove entries from recent projects dropdown](https://github.com/adobe/brackets/pull/1757)
* **Localization**
    * [Italian translation](https://github.com/adobe/brackets/pull/1711)
    * [Spanish translation](https://github.com/adobe/brackets/pull/1839)
    * [Brazilian Portuguese translation](https://github.com/adobe/brackets/pull/1660)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-14...sprint-15#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-14...sprint-15#commits_bucket)

UI Changes
----------

API Changes
-----------

New/Improved Extensibility APIs
-------------------------------
_TODO:_ https://github.com/adobe/brackets/pull/1753

Known Issues
------------
* [#1551](https://github.com/adobe/brackets/issues/1551): Changes within an extension (or a unit test) are not reflected by a simple "Debug > Reload Brackets." Workarounds:
    * Quit and re-launch Brackets to pick up the changes.
    * Open Developer Tools, click the gear icon in the lower-right, and select "Disable cache." This setting is remembered, but is only in effect so long as the Developer Tools browser tab remains open.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* _Debug > Show Developer Tools_ opens in a new tab in Chrome, rather than a new window in Brackets. This is a temporary(ish) change due to CEF3. But on the upside, CEF3's developer tools include many updated features!
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight sometimes jiggles when horizontally resizing window.
* [#1473](https://github.com/adobe/brackets/issues/1473): Uninstalling on Windows sometimes does not remove the Start menu shortcut for Brackets.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not being digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.


Community contributions to Brackets
-----------------------------------
* [Status bar](https://github.com/adobe/brackets/pull/1717) by [Chema Balsas](https://github.com/jbalsas)
* [Resizable bottom panels (JSLint & search results)](https://github.com/adobe/brackets/pull/1661) (plus follow-up fixes) by [Chema Balsas](https://github.com/jbalsas)
* [Italian translation](https://github.com/adobe/brackets/pull/1711) by [Antonello Pasella](https://github.com/antonellopasella)
* [Spanish translation](https://github.com/adobe/brackets/pull/1839) by [Chema Balsas](https://github.com/jbalsas)
* [Brazilian Portuguese translation](https://github.com/adobe/brackets/pull/1660) by [Massimiliano Giroldi](https://github.com/massimiliano-giroldi), with [followup fixes & updates](https://github.com/adobe/brackets/pull/1824) by [Bruno Cardoso](https://github.com/brucardoso2)
* [Delete Line(s) command](https://github.com/adobe/brackets/pull/1763) by [Mihai Corlan](https://github.com/mcorlan)
* [Add connection checkmark to Live Preview menu item](https://github.com/adobe/brackets/pull/1707) by ["ckarande"](https://github.com/ckarande)
* [Fix #1064: Make URLs line-wrap in various parts of UI](https://github.com/adobe/brackets/pull/1790) by [Jake Stoeffler](https://github.com/JakeStoeffler)
* [Fix #1256: Don't close entire inline editor when a file _some_ of its results is deleted](https://github.com/adobe/brackets/pull/1769) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Fix #1606: Inline editor layout glitch when switching back to file in certain cases](https://github.com/adobe/brackets/pull/1750) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Fix #510: Middle-click in inline editor's chrome scrolls view & breaks editor](https://github.com/adobe/brackets/pull/1751) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Misc bug fixes](https://github.com/adobe/brackets/pull/1813) by [Jonathan Diehl](https://github.com/jdiehl)
* [Improved error message in log for key binding conflicts](https://github.com/adobe/brackets/pull/1851) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Fire AppInit.APP_READY after loading extensions, as originally intended](https://github.com/adobe/brackets/pull/1854) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Fix GotoAgent (disabled by default) to work with paths containing spaces](https://github.com/adobe/brackets/pull/1748) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Fix JSLint warning](https://github.com/adobe/brackets/pull/1777) by [Dennis Kehrig](https://github.com/DennisKehrig)

Contributions _from_ Brackets
------------------------------


Bugs fixed in Sprint 15
-----------------------
For details on the bugs addressed, please refer to [closed sprint 15 bugs](https://github.com/adobe/brackets/issues?labels=sprint+15&state=closed). A few of the fixed bugs might not be caught by this search query, however.