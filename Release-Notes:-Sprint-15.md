_This is a draft!_
-------------------
_This document will not be finalized until the end of Sprint 15 -- approximately October 18._


What's New in Sprint 15
-----------------------
* **Placeholder**
    * Placeholder
* **Placeholder**
    * Placeholder

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-14...sprint-15#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-14...sprint-15#commits_bucket)

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
* _Debug > Show Developer Tools_ opens in a new tab in Chrome, rather than a new window in Brackets. This is a temporary(ish) change due to CEF3. But on the upside, CEF3's developer tools include many updated features!
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight sometimes jiggles when horizontally resizing window.
* [#1473](https://github.com/adobe/brackets/issues/1473): Uninstalling on Windows sometimes does not remove the Start menu shortcut for Brackets.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not being digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.


Community contributions to Brackets
-----------------------------------


Contributions _from_ Brackets
------------------------------


Bugs fixed in Sprint 15
-----------------------
For details on the bugs addressed, please refer to [closed sprint 15 bugs](https://github.com/adobe/brackets/issues?labels=sprint+15&state=closed). A few of the fixed bugs might not be caught by this search query, however.