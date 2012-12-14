What's New in Sprint 16
-----------------------
* **Live Development**
    * [Local server support for Live Preview](https://trello.com/card/3-url-mapping-for-live-development/4f90a6d98f77505d7940ce88/664): Specify a base URL for the project root via File > Project Settings. This overrides the default file:// URL.
* **Search**
    * [Quick Open search improvements](https://github.com/adobe/brackets/pull/1470): Quick Open now searches on the whole path (not just filename), and accepts CamelCase abbreviations and other non-contiguous matches.
    * [Find and Find in Files](https://github.com/adobe/brackets/pull/1914): Search field turns red when no results are found. Invalid regexes trigger an error tip.
* **Files and Folders**
    * [Reorder working set by drag & drop](https://github.com/adobe/brackets/pull/1940)
    * [Sort working set](https://github.com/adobe/brackets/pull/1999): Sort the list of open files manually or automatically by file type, name or when added (the default).
    * [Close All command](https://github.com/adobe/brackets/pull/2037)
    * [Working set context menu](https://github.com/adobe/brackets/pull/1919): Now includes Rename, Show In Tree.
    * [F2 shortcut for Rename](https://github.com/adobe/brackets/pull/1922)
* **General Code Editing**
    * [Select Line command](https://github.com/adobe/brackets/pull/2002): Ctrl+L selects the current line or adds one line to an existing selection.
    * [Progress on CodeMirror 3 migration](https://trello.com/card/2-codemirror-3-critical-editing-functionality/4f90a6d98f77505d7940ce88/660): Critical editing functionality is now working on the [cmv3 branch](https://github.com/adobe/brackets/compare/master...cmv3).
    * Syntax highlighting for: YAML, SVG (as XML)
* **Extensions**
    * [Move extensions folder outside application root](https://trello.com/card/3-extensions-outside-application-root/4f90a6d98f77505d7940ce88/659): Extensions are now stored per user instead of in the application bundle. As before, you can use Help > Show Extensions Folder to access these locations:
        * Windows: `C:\Users\<user>\AppData\Roaming\Brackets\extensions`
        * Mac: `/Users/<user>/Library/Application Support/Brackets/extensions`
    * Extension developers can still place extensions in the Brackets source location instead, in the new `src/extensions/dev` folder.
* **Developer Workflow**
    * [Automated unit tests](https://trello.com/card/2-automate-unit-tests/4f90a6d98f77505d7940ce88/661) - Unit tests now run automatically on a Mac build server. Results are not posted publicly yet.
* **Localization**
    * [Japanese translation](https://github.com/adobe/brackets/pull/1929)
* **Steps Toward Future Platforms**
    * Community work on a Linux build is [making great progress](https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/29vOJ6tvl8A)
    * Early prototype of Brackets running in the browser: see [in-browser](https://github.com/adobe/brackets/compare/master...in-browser) branch.



_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-15...sprint-16#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-15...sprint-16#commits_bucket)


UI Changes
----------
**Extensions folder** - The location where you install extensions has changed. See above for details.

**Project settings** - A Project Settings dialog is accessible from the File menu or the project dropdown menu in the sidebar. Currently the only setting available is the Live Preview Base URL.

**Code hinting** - Tab and Enter now work exactly the same when accepting a code hint suggestion.


API Changes
-----------
**Extensions folder** - The `src/extensions/user` folder has been removed. Use `src/extensions/dev` for developing extensions (or the new user-specific folder described above, but note that unit tests will only be read from the dev location).

**Require 2.1.1** - For better error handling while loading extensions, we've [upgraded](https://github.com/adobe/brackets/pull/1968)) from Require 1.0.3 to [2.1.1](https://github.com/jrburke/requirejs/wiki/Upgrading-to-RequireJS-2.1).

**EditorManager focus handling** - The "focusedEditorChange" event was renamed to "activeEditorChange". New `getActiveEditor()` API: similar to `getFocusedEditor()`, but doesn't return null when focus temporarily lies in auxiliary UI like the search bar. `EditorManager.focusEditor()` now focuses whichever Editor last had focus (full-size or inline; previously, it would always focus the full-size editor).

**Code hinting** - Code hint providers can now control whether accepting a suggestion should close the code hint popup or keep it open with updated hints (previously this was controlled by which key the user used, but Enter and Tab no longer behave differently). Return true from `handleSelect()` to close the popup, or false to leave it open if more hints are available after inserting the suggestion. The 4th argument to handleSelect() has been removed.

**Adding panels** - If you add a panel below the editor area, make sure you add it _above_ the new status bar. Use `insertBefore("#status-bar")` or `insertAfter(".bottom-panel:last")`. _(This applies to Sprint 15 as well, but was left out of its release notes)._

**Quick Open** - `QuickOpen.stringMatch()` now supports looser matching, including CamelCase matching. This is transparent to Quick Open providers that use `basicMatchSort()` and the default results formatter. Providers using a custom formatter should use the new `QuickOpen.highlightMatch()` utility to ensure the right chars are bolded in the list item.

**Rename** - `ProjectManager.renameSelectedItem()` has been replaced with `renameItemInline()`, where the item to rename is explicitly passed in. `Commands.FILE_RENAME` now functions if the selection highlight lies in the working set instead of the folder tree.

**Working set** - The order of entries in the working set can now change. DocumentManager dispatches "workingSetReorder" or "workingSetSort" when this occurs. _(Note: these events are likely to change soon: see [#2076](https://github.com/adobe/brackets/issues/2076))._

New/Improved Extensibility APIs
-------------------------------
**brackets.app.showOSFolder()** - Displays a folder in the OS shell

**EditorManager.getActiveEditor()** - See above.

**Resizeable panels** - Panels created via `Resizer.makeResizable()` can now be toggled open/shut programmatically via new methods on Resizer, or by the user (via drag or double-click) if true is passed for the new `collapsible` argument. Panel size is remembered across launches, keyed by panel's DOM id. Dispatches new events "panelResizeStart", "panelResizeUpdate", and "panelCollapsed"/"panelExpanded".

**Menus** - Use `Menu.removeMenuItem()` to remove a command from a given menu.


Known Issues
------------
* Previous versions of the [PhoneGap Build extension](https://github.com/adobe/brackets-phonegap) do not work with this release. Download an updated version via the link above.
* [#1551](https://github.com/adobe/brackets/issues/1551): Changes within an extension (or a unit test) are not reflected by a simple "Debug > Reload Brackets." Workarounds:
    * Quit and re-launch Brackets to pick up the changes.
    * Open Developer Tools, click the gear icon in the lower-right, and select "Disable cache." This setting is remembered, but is only in effect so long as the Developer Tools browser tab remains open.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* _Debug > Show Developer Tools_ opens in a new tab in Chrome, rather than a new window in Brackets.
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight sometimes jiggles when horizontally resizing window.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.


Community contributions to Brackets
-----------------------------------
* [Quick Open improvements: non-contiguous matches; include path in file search](https://github.com/adobe/brackets/pull/1470) by [Kevin Dangoor](https://github.com/dangoor)
* [Reorder working set by drag & drop](https://github.com/adobe/brackets/pull/1940) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Sort working set](https://github.com/adobe/brackets/pull/1999) by [Tomás Malbrán](https://github.com/TomMalbran)
* [F2 shortcut for Rename file/folder](https://github.com/adobe/brackets/pull/1922) by ["zanqi"](https://github.com/zanqi)
* [Select Line command](https://github.com/adobe/brackets/pull/2002) by [Dimitar S.](https://github.com/deemeetar) & [Pritam Baral](https://github.com/pritambaral)
* [Prepopulate Replace field with current selection](https://github.com/adobe/brackets/pull/1964) by [Chema Balsas](https://github.com/jbalsas)
* [Japanese translation](https://github.com/adobe/brackets/pull/1929) by [Shumpei Shiraishi](https://github.com/shumpei)
* [Enable manual path entry field in Open Folder dialog](https://github.com/adobe/brackets-shell/pull/140) by ["zanqi"](https://github.com/zanqi)
* [Unify panel resizing code, allow custom min size](https://github.com/adobe/brackets/pull/1908) by [Chema Balsas](https://github.com/jbalsas)
* [removeMenuItem() API](https://github.com/adobe/brackets/pull/2072) mostly by [Terry Ryan](https://github.com/tpryan)
* [Support Linux in brackets.platform](https://github.com/adobe/brackets/pull/1983) by [Pritam Baral](https://github.com/pritambaral)
* [Fix #1886: Saving CSS file from inline editor auto-refreshes page](https://github.com/adobe/brackets/pull/1897) by [Jake Stoeffler](https://github.com/JakeStoeffler)
* [Fix #1971: Renaming file twice shows wrong name](https://github.com/adobe/brackets/pull/1990) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #1172: Support numpad symbols for keyboard shortcuts (Comment/Uncomment, font size)](https://github.com/adobe/brackets/pull/1946) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #1780: Recent project dropped from list when cancelling project switch](https://github.com/adobe/brackets/pull/2013) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #104: Close file icon not displayed until mouse is moved](https://github.com/adobe/brackets/pull/1969) by [Jeffrey Fisher](https://github.com/jeffslofish)
* [Fix #1548: Layout issues when sidebar is narrow](https://github.com/adobe/brackets/pull/2040) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #1508: Root folders displayed wrong in recent projects list](https://github.com/adobe/brackets/pull/1926) by [Jeffrey Fisher](https://github.com/jeffslofish)
* [Fix #1917: Prevent multi-select in project tree](https://github.com/adobe/brackets/pull/1945) (since it was broken) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #1916: Menu name inconsistency: Lines vs. Line(s)](https://github.com/adobe/brackets/pull/1928) by [Kraig Walker](https://github.com/KraigWalker)
* [Unit test for file rename](https://github.com/adobe/brackets/pull/1939) by ["zanqi"](https://github.com/zanqi)
* [Unit test for reading root of drive](https://github.com/adobe/brackets/pull/2038) by ["zanqi"](https://github.com/zanqi)
* [Reinstate disabled unit test](https://github.com/adobe/brackets/pull/1907) by [Jeffrey Fisher](https://github.com/jeffslofish)
* [Code cleanup: simplify Live Development init](https://github.com/adobe/brackets/pull/1880) by [Jonathan Diehl](https://github.com/jdiehl)
* [Code cleanup: unused CodeMirror keybindings](https://github.com/adobe/brackets/pull/2039) by ["zanqi"](https://github.com/zanqi)
* [German translation updates](https://github.com/adobe/brackets/pull/1903) ([and](https://github.com/adobe/brackets/pull/1989)) by ["J.M."](https://github.com/mynetx)
* [Make LICENSE format more standard](https://github.com/adobe/brackets/pull/1696) by [Dave Johnson](https://github.com/davejohnson)

Contributions _from_ Brackets
-----------------------------

Bugs fixed in Sprint 16
-----------------------
For details on the bugs addressed, please refer to [closed sprint 16 bugs](https://github.com/adobe/brackets/issues?labels=sprint+16&state=closed). A few of the fixed bugs might not be caught by this search query, however.