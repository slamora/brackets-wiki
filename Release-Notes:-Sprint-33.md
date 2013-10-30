What's New in Sprint 33
-----------------------
* **Quick Edit**
    * [Create new CSS rules via Quick Edit](https://trello.com/c/5I5AddGo/599-5-css-quick-edit-create-new-selector): In HTML files, click the new "New Rule" button in the Quick Edit inline editor to create a new CSS rule based on the tag/class/id your cursor was on. Fully keyboard accessible using Cmd/Ctrl-Alt-N. The inline editor now appears even when no existing rules match the search, so you can easily create new CSS rules at any time.
    * [Visually edit CSS transition timing function Bezier curves](https://trello.com/c/5EPJdO1q/838-2-quick-edit-css-cubic-bezier): Just invoke Quick Edit when your cursor is on any `cubic-bezier()`, `linear`, `ease`, `ease-in`, `ease-out`, or `ease-in-out` functions in a CSS rule!
* **Images**
    * [Preview image files](https://trello.com/c/l9AcILkC/24-8-preview-images): Select in image in the file tree (or via Quick Open) to see a preview in the editor area.
* **Replace**
    * When using Replace with a Regep search argument, Brackets now [handles replacements with $1, $2, etc.](https://github.com/adobe/brackets/pull/5618).
* **Extension Manager**
    * If latest version of an extension is not compatible with your version of Brackets, then [Extension Manager will install an older version of the extension](https://github.com/adobe/brackets/pull/5653), if available.
* **Live Preview**
    * If there is no file open, [Live Preview will now start with index.html file](https://github.com/adobe/brackets/pull/5547), if present.
* **Localization**
    * Added Serbian translation
    * Updated translations for: Czech, Finnish, German, Polish, Spanish, Swedish, and Turkish.

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-32...sprint-33#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-32...sprint-33#commits_bucket)


UI Changes
----------
**Dialogs** - Modal Dialogs are now [auto-centered](https://github.com/adobe/brackets/pull/5399) both horizontally and vertically over the Brackets Window.

**Inline editors** - An "**x**" Close button is now automatically created for all inline editors.


API Changes
-----------
**Lo-Dash** - added as a third-party dependency. The following conversions and deprecations were made:

* Converted `Array.slice(0)` to `_.clone()`
* Converted `Async.whenIdle` to `_.debounce`.
* Converted `NumberUtils.getRandomNumber()` to `_.random` and removed `NumberUtils.js`
* Converted `StringUtils.htmlEscape` to `._escape` and deprecated it.
* Converted `CollectionUtils.indexOf` to `_.findIndex` and deprecated it.
* Converted `CollectionUtils.forEach` to `_.forEach` and deprecated it.
* Converted `CollectionUtils.some` to `_.some` and deprecated it.
* Converted `CollectionUtils.hasProperty` to `_.has` and deprecated it.

Functions are deprecated by adding a `@deprecated` annotation, and by replacing their implementation with the corresponding Lo-Dash function and a `console.warn` message.

**Image files** - The [Preview Images Spec](https://github.com/adobe/brackets/wiki/Preview-Images-Spec) describes the API changes for the new Preview Images feature.

**Inline editors** - `InlineTextEditor.editors` (array of editors) was removed and replaced with `InlineTextEditor.editor` (single editor). With current usage, there was never more than a single element in the array anyway, so this simplifies the API.

**Files** - usage of trailing-"/" was cleaned up.

The "canonical" folder path format used in `DirectoryEntry.fullPath` includes a trailing "/".  However, we have some Brackets code that requires or generates paths in the _opposite_ format.  Several of those cases were cleaned up:

* Renamed `FileUtils.canonicalizeFolderPath()` to `stripTrailingSlash()` since it actually makes paths **not** canonical. Old API was **deprecated** since it's still used by several extensions.
* Added warning to docs for these APIs that return non-canonical paths: `FileUtils.getNativeBracketsDirectoryPath()` and `FileUtils.getNativeModuleDirectoryPath()`.
* Documented that `ProjectManager._loadProject()` and `openProject()` support receiving non-canonical paths
* Fixed several `ProjectManager` APIs that used to return and/or receive non-canonical paths: `getInitialProjectPath()`, `_getWelcomeProjectPath()`, and `updateWelcomeProjectPath()`.  No extensions were found that use these APIs.

New/Improved Extensibility APIs
-------------------------------
**Lo-Dash** - [utility library](http://lodash.com/) is now available in Brackets.


Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. [See workaround](https://support.google.com/chrome/answer/106010?hl=en).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Mac: Accept folder dropped on dock icon](https://github.com/adobe/brackets-shell/pull/353) by [Eugene Ostroukhov](https://github.com/eugeneo)
* [Start Live Development even if no open file](https://github.com/adobe/brackets/pull/5547) by [Marcel Gerber](https://github.com/SAPlayer)
* [Close Others [Above/Below] in Working Files context menu](https://github.com/adobe/brackets/pull/4590) by [Sathyamoorthi](https://github.com/sathyamoorthi)
* [Find in Files: Better feedback while searching & when no results](https://github.com/adobe/brackets/pull/5477) by [Sharat M R](https://github.com/cosmosgenius)
* [Add close button to all inline editors](https://github.com/adobe/brackets/pull/5443) by [Thomas Erbe](https://github.com/VizuaaLOG)
* [Add Serbian translation](https://github.com/adobe/brackets/pull/5515) by [Goran Vasić](https://github.com/Gocilla)
* [Make inline Bezier editor localizable](https://github.com/adobe/brackets/pull/5553) by [Marcel Gerber](https://github.com/SAPlayer)
* [Center dialogs dynamically](https://github.com/adobe/brackets/pull/5399) ([part 2](https://github.com/adobe/brackets/pull/5484), [part 3](https://github.com/adobe/brackets/pull/5579)) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Press Delete in Recent Projects popup to remove from list](https://github.com/adobe/brackets/pull/5354) by [Sandeep Jain](https://github.com/sandeepjain)
* [Close 'Recent Projects' dropdown on 2nd click](https://github.com/adobe/brackets/pull/5435) by [Marcel Gerber](https://github.com/SAPlayer)
* [Linux: Fix leak when creating folders](https://github.com/adobe/brackets-shell/pull/356) by [eyelash](https://github.com/eyelash)
* [Fix #4949: URL hints didn't show without explicit Ctrl+Space](https://github.com/adobe/brackets/pull/5422) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix #5517: Blank line number when switching between CSS rules with same number](https://github.com/adobe/brackets/pull/5582) by [Patrick Oladimeji](https://github.com/thehogfather)
* [Fix #5426: Quick View popup flickers when moving mouse rightward)](https://github.com/adobe/brackets/pull/5428) by [Marcel Gerber](https://github.com/SAPlayer)
* [Fix Polish translation bug that prevented launching a clean install of Brackets](https://github.com/adobe/brackets/pull/5471) by [Mateusz Gachowski](https://github.com/mateuszgachowski)
* [Fix Menus.removeMenu() to clean up dividers](https://github.com/adobe/brackets/pull/5384) by [Lance Campbell](https://github.com/lkcampbell)
* [Min height for search results panel](https://github.com/adobe/brackets/pull/5391) by [Oskar Tjoskar](https://github.com/tjoskar)
* [Improve unit test robustness: avoid not.toBeNull()](https://github.com/adobe/brackets/pull/5492) by [gmeluski](https://github.com/gmeluski)
* [Fix InstallExtensionDialog unit tests for non-English locales](https://github.com/adobe/brackets/pull/5433) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Cleanup: Remove references to CodeHintManager from Editor](https://github.com/adobe/brackets/pull/5421) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Cleanup: Change Bezier editor loc keys to match idenfier rename](https://github.com/adobe/brackets/pull/5644) by [Marcel Gerber](https://github.com/SAPlayer)
* [Cleanup: Remove Sprint 22 preferences migration code](https://github.com/adobe/brackets/pull/5429) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Cleanup: Keep translation files' formatting consistent](https://github.com/adobe/brackets/pull/5505) by [Marcel Gerber](https://github.com/SAPlayer)
* [German translation update](https://github.com/adobe/brackets/pull/5470) by [mynetx](https://github.com/mynetx)
* [German translation update](https://github.com/adobe/brackets/pull/5449) ([part 2](https://github.com/adobe/brackets/pull/5459), [part 3](https://github.com/adobe/brackets/pull/5567), [part 4](https://github.com/adobe/brackets/pull/5583), [part 5](https://github.com/adobe/brackets/pull/5657), [part 6](https://github.com/adobe/brackets/pull/5667)) by [Marcel Gerber](https://github.com/SAPlayer)
* [Spanish translation update](https://github.com/adobe/brackets/pull/5676) by [Chema Balsas](https://github.com/jbalsas)
* [Czech translation update](https://github.com/adobe/brackets/pull/5510) ([part 2](https://github.com/adobe/brackets/pull/5331)) by [kvarel](https://github.com/kvarel)
* [Finnish translation fixes](https://github.com/adobe/brackets/pull/5556) ([and](https://github.com/adobe/brackets/pull/5557)) by [Jukka Hyytiälä](https://github.com/jukkah)
* [Swedish translation fix](https://github.com/adobe/brackets/pull/5647) by [Michael Cole](https://github.com/micole)

#### Pulling source code from Git
* A new brackets-shell build is _required_ for this sprint (API change on Mac, Dark Shell fixes on Windows). Be sure to rerun `grunt setup` before building.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Sprint 33
-----------------------
For details on the bugs addressed, please refer to [closed sprint 33 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=20&state=closed). A few of the fixed bugs might not be caught by this search query, however.