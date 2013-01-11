_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 19 -- approximately January 18, 2013._

What's New in Sprint 19
-----------------------
* **TODO: category heading**
    * TODO: feature list


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-18...sprint-19#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-18...sprint-19#commits_bucket)


UI Changes
----------
No major changes to existing features.


API Changes
-----------
* **Deprecating `offsetTopChanged` event.** `Editor` currently dispatches an `offsetTopChanged` event on inline (Quick Edit) widgets when an edit happens above them. This used to be necessary for CSS and JS inline editors, which were doing hacky things to position the right-hand-side list. As part of the CodeMirror v3 merge, we will be eliminating those hacks, and plan to remove this event eventually, so as of now the event is deprecated. Please let us know if you rely on this event.

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
