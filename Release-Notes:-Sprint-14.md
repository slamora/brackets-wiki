_This is a draft!_
------------------
_This document will not be finalized until the end of Sprint 14 -- September 14._

What's New in Sprint 14
-----------------------

UI Changes
----------

API Changes
-----------

New/Improved Extensibility APIs
-------------------------------

Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* _Debug > Show Developer Tools_ now opens in a new tab in Chrome, rather than a new windows in Brackets. This is a temporary(ish) change due to CEF3. But on the upside, CEF3's developer tools include many updated features!
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight sometimes jiggles when horizontally resizing window.
* [#1473](https://github.com/adobe/brackets/issues/1473): Uninstalling on Windows sometimes does not remove the Start menu shortcut for Brackets.

Community contributions to Brackets
-----------------------------------

Contributions _from_ Brackets
-----------------------------

Bugs fixed in Sprint 14
-----------------------
For details on the bugs addressed, please refer to [closed sprint 14 bugs](https://github.com/adobe/brackets/issues?labels=sprint+14&page=1&state=closed). A few of the fixed bugs might not be caught by this search query, however.