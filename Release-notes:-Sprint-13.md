Sprint 13 focused primarily on a more polished release experience: installers, updates, getting started, and an all new native app shell. We also rounded out HTML code hinting by adding hints for common attribute values, and began a push to localize the Brackets UI.

What's New in Sprint 13
-----------------------
* **Install/Upgrade Usability**
    * [Update Notification](https://trello.com/card/3-new-version-notification/4f90a6d98f77505d7940ce88/579): Brackets will automatically notify you when a newer build is released. This won't happen until the end of the _following_ sprint, 2-3 weeks from now.
    * [Mac Install Experience: DMG with shortcut](https://trello.com/card/2-brackets-installer-osx/4f90a6d98f77505d7940ce88/394): Instead of a ZIP file, the distribution for Mac is now a DMG file containing a shortcut to /Applications for easy drag & drop.
    * [Windows Install Experience: MSI installer](https://trello.com/card/3-brackets-installer-win/4f90a6d98f77505d7940ce88/597): Instead of a ZIP file, the distribution for Windows is now a native MSI installer.
    * [First Launch Readme/Demo Project](https://trello.com/card/2-first-launch-project/4f90a6d98f77505d7940ce88/598)
    * Quick access to the extensions folder via _Debug > Show Extensions Folder_
* **Native Shell**
    * Brackets has officially migrated to the [new CEF3-based native shell](https://github.com/adobe/brackets-shell/)! The installer/DMG distributions above package the new shell. This gives Brackets a stronger platform to build on, and offers benefits like slightly faster performance and a newer version of the developer tools for debugging Brackets. A few [issues](https://github.com/adobe/brackets-shell/issues?labels=&page=1&state=open) remain, but no blockers.
* **Code Hinting**
    * [Code Completion for HTML Attribute Values](https://trello.com/card/2-code-complete-html-attribute-values/4f90a6d98f77505d7940ce88/583): Attributes with enumerated values (such as `<link rel>` or `<input type>`) show hints automatically after completing the attribute name. Hints can also be invoked manually with Ctrl+Space.
* **Localization**
    * [French Localization](https://trello.com/card/3-localization-french/4f90a6d98f77505d7940ce88/374): Brackets detects the user's locale setting automatically. There are a few bugs where minor bits of the UI remain in English despite the locale.
    * Brackets unofficially also supports German in this sprint.
* **Other Enhancements**
    * [Recent projects dropdown](https://github.com/adobe/brackets/pull/1469): Click the project name in the left-hand to rapidly switch between recently opened project folders.

UI Changes
----------
no major changes to existing UI

API Changes
-----------
**Initialization & HTML structure** - The HTML DOM is no longer completly initialized when require.js loads Brackets modules. _Brackets extensions are unaffected_ since they are always loaded after the DOM initialization is complete; this only affects core modules that depend on or modify HTML during load.
* Before touching the DOM, modules should now register a ``htmlReady`` event listener on the ``utils/AppInit`` module before touching the DOM. E.g. ``AppInit.htmlReady(function () { /* query main Brackets DOM */ })``.
* The ``brackets.ready`` event handler has been moved to ``AppInit.appReady``
* A new ``utils/Global`` module was added to handle the initialization of the ``brackets`` namespace
* Most of the content of index.html has been moved into the Mustache template htmlContent/main-view.html.

**Live development** - Now uses standard jQuery events, so the code required to add/remove listeners is slightly different and all listeners receive a new first parameter (the event). Event names have also changed. [See pull request for details](https://github.com/adobe/brackets/pull/1396).


New/Improved Extensibility APIs
-------------------------------
**Command infrastructure** - New `CommandManager.getAll()` API: returns all registered commands. Could be used to build a shortcut key assignment UI, etc.

**Localization** - Brackets now includes the RequireJS i18n plugin and Mustache.

**Launch URLs in browser** - Use `NativeApp.openURLInDefaultBrowser()` to open a URL in the user's web browser.


Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* _Debug > Show Developer Tools_ now opens in a new tab in Chrome, rather than a new windows in Brackets. This is a temporary(ish) change due to CEF3. But on the upside, CEF3's developer tools include many updated features!
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight sometimes jiggles when horizontally resizing window.
* [#1473](https://github.com/adobe/brackets/issues/1473): Uninstalling on Windows sometimes does not remove the Start menu shortcut for Brackets.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not being digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.

Community contributions to Brackets
-----------------------------------
* [Localization infrastructure, basic language switcher UI, and first draft of German translation](https://github.com/adobe/brackets/pull/1358) by [Jonathan Diehl](https://github.com/jdiehl)
* [Live development code cleanup](https://github.com/adobe/brackets/pull/1396) by [Jonathan Diehl](https://github.com/jdiehl)
* [Fix #1409: Exception when opening inline editor on blank line](https://github.com/adobe/CodeMirror2/pull/75) by [Tom Lieber](https://github.com/alltom)
* [Code cleanup in SVG sprites](https://github.com/adobe/brackets/pull/1453) by [Dmitry Baranovskiy](https://github.com/DmitryBaranovskiy)

Bugs fixed in Sprint 13
-----------------------
For details on the bugs addressed, please refer to [closed sprint 13 bugs](https://github.com/adobe/brackets/issues?labels=sprint+13&page=1&state=closed) and [closed sprint 13 brackets-shell bugs](https://github.com/adobe/brackets-shell/issues?labels=sprint+13&page=1&state=closed). A few of the fixed bugs might not be caught by this search query, however.