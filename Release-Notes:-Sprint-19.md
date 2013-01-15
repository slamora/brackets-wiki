_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 19 -- approximately January 18, 2013._

What's New in Sprint 19
-----------------------
* **Overall UI**
    * [Native menu bar](https://trello.com/board/brackets/4f90a6d98f77505d7940ce88) on Mac and Windows. Menu-related APIs remain unchanged.
* **Code Editing**
    * [Code hints for CSS properties & values](https://github.com/adobe/brackets/pull/2492)
    * Final prep for CodeMirror3 migration: [inline editors](https://trello.com/card/2-codemirror-3-inline-editor-size-vertically/4f90a6d98f77505d7940ce88/651) and [scrolling](https://trello.com/card/3-codemirror-3-scrolling/4f90a6d98f77505d7940ce88/652) now work well on the [cmv3 branch](https://github.com/adobe/brackets/compare/master...cmv3).
* **Search**
    * Quick Open / Go to Definition ranks results much better
    * Go to Definition is more responsive, especially on longer files
* **Localization**
    * [Swedish translation](https://github.com/adobe/brackets/pull/2477)
* **JSLint**
    * ["Go to First JSLint Error" shortcut](https://github.com/adobe/brackets/pull/2525): F8 on Windows, Cmd+' on Mac


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-18...sprint-19#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-18...sprint-19#commits_bucket)


UI Changes
----------
* **Toggle Block Comment** on Mac now uses Cmd+Opt+/ (instead of Cmd+Shift+/)
* **Cmd+Left arrow** on mac now behaves the same as Fn+Left arrow &ndash; the cursor moves to the first _non-whitespace_ character on the line unless it's already on that char, in which case the cursor moves to the start of the line. (Previously, Cmd+Left always moved to the start of the line regardless of whitespace).


API Changes
-----------
**Startup order of events** - [TODO](https://github.com/adobe/brackets/pull/2501)

**Editor `offsetTopChanged` event deprecated** - `Editor` currently dispatches an `offsetTopChanged` event on inline (Quick Edit) widgets when something happens that changes their vertical position relative to the page. This used to be necessary for CSS and JS inline editors, which were doing hacky things to position the right-hand-side list. As part of the CodeMirror v3 merge, we will be eliminating those hacks, and plan to remove this event eventually, so as of now the event is deprecated. Please let us know if you rely on this event.

**ContextMenu `contextMenuClose` event removed** - This event was not very useful/reliable; in many cases, it was never actually dispatched after a menu closed.

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
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.


Community contributions to Brackets
-----------------------------------

Contributions _from_ Brackets
-----------------------------

Bugs fixed in Sprint 19
-----------------------
For details on the bugs addressed, please refer to [closed sprint 19 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=6&state=closed). A few of the fixed bugs might not be caught by this search query, however.