_This is a draft!_
------------------
_This document will not be finalized until the end of Sprint 14 -- September 14._

What's New in Sprint 14
-----------------------
* New Help menu ``AppMenuBar.HELP_MENU``
* Moved "Show Extensions" and "Check for Updates" from Debug to Help menu

UI Changes
----------
* Editor now uses the open source, monowidth Source Code Pro OpenType font from Adobe.

API Changes
-----------


New/Improved Extensibility APIs
-------------------------------
* A new ``src/package.json`` file was introduced to capture metadata in a format compatible with [NPM](https://npmjs.org/doc/json.html).
    * ``package.json`` data can be accessed at runtime via ``brackets.metadata``
    * ``name`` field is copied to the ``strings`` module as ``APP_NAME``
        * ``APP_NAME`` string is substituted throughout the code base
    * ``package.json`` config object allows forks of Brackets to customize some options including:
        * ``about_icon`` - Change the ``src`` relative path to the about icon
        * ``show_debug_menu`` - Show/Hide the "Debug" menu
        * ``enable_jslint`` - Enable/Disable JSLint by default
        * ``forum_url`` - Specifies a URL in the Help menu


Known Issues
------------
* Due to bug #1512, if you install Sprint 14 after having run Sprint 13 (and don't delete your prefs), the old Sprint 13 "Getting Started" project will show in your recent projects list (and will open by default if you left it open last time you quit Brackets). This has been fixed for upgrades post-Sprint 14.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* _Debug > Show Developer Tools_ now opens in a new tab in Chrome, rather than a new windows in Brackets. This is a temporary(ish) change due to CEF3. But on the upside, CEF3's developer tools include many updated features!
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight sometimes jiggles when horizontally resizing window.
* [#1473](https://github.com/adobe/brackets/issues/1473): Uninstalling on Windows sometimes does not remove the Start menu shortcut for Brackets.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not being digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.


Community contributions to Brackets
-----------------------------------
* [Persist "Use Tab Characters" setting across launches](https://github.com/adobe/brackets/pull/1500) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Support escaped characters in CSS selectors (bug #391)](https://github.com/adobe/brackets/pull/1509) by [Chema Balsas](https://github.com/jbalsas)
* [Norwegian translation](https://github.com/adobe/brackets/pull/1448) by [Thomas Andersen](https://github.com/thomasandersen)
* [Spanish translation](https://github.com/adobe/brackets/pull/1587) by [Chema Balsas](https://github.com/jbalsas)

Contributions _from_ Brackets
-----------------------------


Bugs fixed in Sprint 14
-----------------------
For details on the bugs addressed, please refer to [closed sprint 14 bugs](https://github.com/adobe/brackets/issues?labels=sprint+14&page=1&state=closed). A few of the fixed bugs might not be caught by this search query, however.