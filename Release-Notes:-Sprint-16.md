_This is a draft!_
-------------------
_This document will not be finalized until the end of Sprint 16 -- approximately November 9._

What's New in Sprint 16
-----------------------
* **TODO: headings**
    * [URL mapping for Live Development](https://trello.com/card/3-url-mapping-for-live-development/4f90a6d98f77505d7940ce88/664)
    * [Move extensions folder outside application root](https://trello.com/card/3-extensions-outside-application-root/4f90a6d98f77505d7940ce88/659)
    * [Integrate color picker](https://trello.com/card/2-color-selector/4f90a6d98f77505d7940ce88/662)
    * [CodeMirror 3 migration: Critical editing functionality](https://trello.com/card/2-codemirror-3-critical-editing-functionality/4f90a6d98f77505d7940ce88/660)
    * [Developer workflow: Automated unit tests](https://trello.com/card/2-automate-unit-tests/4f90a6d98f77505d7940ce88/661)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-15...sprint-16#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-15...sprint-16#commits_bucket)

UI Changes
----------

API Changes
-----------
* [Require 2.1.1](https://github.com/adobe/brackets/pull/1968): For better error handling while loading extensions, we've upgraded from Require 1.0.3 to [2.1.1](https://github.com/jrburke/requirejs/wiki/Upgrading-to-RequireJS-2.1)

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

Contributions _from_ Brackets
-----------------------------

Bugs fixed in Sprint 16
-----------------------
For details on the bugs addressed, please refer to [closed sprint 16 bugs](https://github.com/adobe/brackets/issues?labels=sprint+16&state=closed). A few of the fixed bugs might not be caught by this search query, however.