What's New in Sprint 14
-----------------------
* **Editor UI**
    * [New 'Source Code Pro' Editor Font](https://trello.com/card/2-add-adobe-source-code-pro-font/4f90a6d98f77505d7940ce88/452): Brackets now uses a new, open-source monospace web font from Adobe called Source Code Pro.
* **Build/Development Process**
    * [Improved Development Workflow](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-setup_for_hacking): It's now easier and less error-prone to point an official build of brackets-shell at your own brackets Git repo. See link for details.
    * Version Info: Improved version numbering of official builds, including Git repo SHA. Version info can be accessed via the Help > About menu.
    * More Modular Metadata: Application properties like icon and version number are now consolidated in a `package.json` file. See below for details.
* **Localization**
    * [Norwegian translation](https://github.com/adobe/brackets/pull/1448) added
    * [Spanish translation](https://github.com/adobe/brackets/pull/1587) added
    * [French localization improved](https://trello.com/card/1-cc-french-localization/4f90a6d98f77505d7940ce88/618): both the application and installers are now fully localized in French.
    * German localization updated & improved
* **Other Enhancements**
    * "Use Tab Characters" setting is remembered across launches
    * [Improved Quick Open sorting](https://github.com/adobe/brackets/pull/1565): better matches are shown at the top of the list
    * New Help menu consolidating existing menu items, plus a link to the [discussion forum](https://groups.google.com/forum/?fromgroups#!forum/brackets-dev)
    * Next/Previous Document navigation commands (Ctrl+Tab / Ctrl+Shift+Tab) are now visible in the Navigate menu

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-13...sprint-14#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-13...sprint-14#commits_bucket)

UI Changes
----------
* Code editor now uses a new, open-source monospace web font from Adobe called Source Code Pro.
* "Debug > Show Extensions Folder" and "Debug > Check for Updates" have moved to the new Help menu.

API Changes
-----------
**Quick Open search provider API** - your search provider's itemFocus() and itemSelect() functions will now get passed the item returned by your search() function, _not_ a DOM node as before. In addition, search() can now optionally return an object instead of a string (which is then passed to resultsFormatter(), itemFocus(), and itemSelect()). Combined, these changes make it easier to store metadata for each search result.

The QuickOpen module has several new helper APIs: stringMatch() and basicMatch() simplify creating a basic search provider.

[See the pull request for an example](https://github.com/adobe/brackets/pull/1565/files#diff-0) of updating an existing search provider for these changes.

New/Improved Extensibility APIs
-------------------------------
**package.json metadata** - a new ``src/package.json`` file was introduced to capture metadata in a format compatible with [NPM](https://npmjs.org/doc/json.html).

* ``package.json`` data can be accessed at runtime via ``brackets.metadata``
* ``name`` field is copied to the ``strings`` module as ``APP_NAME``
    * ``APP_NAME`` string is substituted throughout the code base
* ``package.json`` config object allows forks of Brackets to customize some options including:
    * ``about_icon`` - Change the ``src`` relative path to the about icon
    * ``show_debug_menu`` - Show/Hide the "Debug" menu
    * ``enable_jslint`` - Enable/Disable JSLint by default
    * ``forum_url`` - Specifies a URL in the Help menu

**Help menu** - use `AppMenuBar.HELP_MENU` to add items to the new Help menu


Known Issues
------------
* [#1512](https://github.com/adobe/brackets/issues/1512): If you install Sprint 14 after having run Sprint 13 (and don't [delete your prefs](https://github.com/adobe/brackets/wiki/Cache-Folder)), the old Sprint 13 "Getting Started" project will show in your recent projects list (and will open by default if you left it open last time you quit Brackets). This has been fixed for future upgrades post-Sprint 14.
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
* [Norwegian translation](https://github.com/adobe/brackets/pull/1448) (plus followup fixes) by [Thomas Andersen](https://github.com/thomasandersen)
* [Spanish translation](https://github.com/adobe/brackets/pull/1587) by [Chema Balsas](https://github.com/jbalsas)
* [Persist "Use Tab Characters" setting across launches](https://github.com/adobe/brackets/pull/1500) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Support escaped characters in CSS selectors (bug #391)](https://github.com/adobe/brackets/pull/1509) by [Chema Balsas](https://github.com/jbalsas)
* [Show next/previous document commands in menu](https://github.com/adobe/brackets/pull/1642) by [Justin ("twoquestions")](https://github.com/twoquestions)
* [Fix #1508: Root folders show nothing in recent projects list after the "-"](https://github.com/adobe/brackets/pull/1601) by [Rifat Nabi](https://github.com/torifat)
* [Code cleanup: use constants for numeric keycodes](https://github.com/adobe/brackets/pull/1583) by [Jochen Jochen Hagenstroem](https://github.com/couzteau)
* [German translation updates & localization fixes](https://github.com/adobe/brackets/pull/1572) ([and](https://github.com/adobe/brackets/pull/1608)) by [J.M. ("mynetx")](https://github.com/mynetx)
* [README typo fix](https://github.com/adobe/brackets/pull/1607) by [Antoine Lyset](https://github.com/antoinelyset)


Bugs fixed in Sprint 14
-----------------------
For details on the bugs addressed, please refer to [closed sprint 14 bugs](https://github.com/adobe/brackets/issues?labels=sprint+14&page=1&state=closed). A few of the fixed bugs might not be caught by this search query, however.