What's New in Sprint 33
-----------------------
* **Quick Edit**
    * [Create new CSS rules via Quick Edit](https://trello.com/c/5I5AddGo/599-5-css-quick-edit-create-new-selector): In HTML files, click the CSS inline editor's "New Rule" button (or press Cmd/Ctrl-Alt-N) to create a new CSS rule based on the tag/class/id your cursor was on. The inline editor now appears even when no existing rules match the search, so you can easily create new CSS rules at any time.
    * [Visually edit CSS transition Bezier timing functions](https://trello.com/c/5EPJdO1q/838-2-quick-edit-css-cubic-bezier): Just invoke Quick Edit when your cursor is on any `cubic-bezier()` function in a CSS rule! (or a named shorthand such as `linear`, `ease-in`, etc.) to get a graphical editor based on Lea Verou's [cubic-bezier.com](http://cubic-bezier.com/).
* **Images**
    * [Preview image files](https://trello.com/c/l9AcILkC/24-8-preview-images): Select in image in the file tree (or via Quick Open) to see a preview in the editor area.
* **Search/Replace**
    * [Regexp Replace using $1, $2, etc. substitutions](https://github.com/adobe/brackets/pull/5618) in the replacement text
    * [Better Find in Files feedback](https://github.com/adobe/brackets/pull/5477): Search bar remains open while search is in progress, and turns red when no results found (like regular Find).
* **Files and Folders**
    * [Mac: Drag folders onto dock icon](https://github.com/adobe/brackets-shell/pull/353): Previously, folders could only be dragged onto an already-open Brackets window.
    * [Close Others [Above/Below]](https://github.com/adobe/brackets/pull/4590): Quickly close batches of files with these three new commands in the Working Files context menu.
* **Extensions**
    * [Install older version of extension if latest isn't compatible](github.com/adobe/brackets/pull/5653): Previously, Extension Manager would refuse to install the extension at all.
* **Live Preview**
    * [Launch Live Preview when no files open](https://github.com/adobe/brackets/pull/5547) if an index.html (or similar) file exists at the project root.
* **Localization**
    * [Serbian translation added](https://github.com/adobe/brackets/pull/5515)
    * Updated translations: Czech, Finnish, French, German, Spanish, Swedish

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-32...sprint-33#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-32...sprint-33#commits_bucket)


UI Changes
----------
**Inline editors** - All inline widgets (Quick Edit, Quick Docs, etc.) have added an "**x**" Close button in the upper-left.


API Changes
-----------
**Lo-Dash utils** - Now included in Brackets, replacing the following Brackets APIs:

* **Removed** `Async.whenIdle` - use `_.debounce`
* **Removed** `NumberUtils.getRandomNumber()` - use `_.random` (entire NumberUtils module was removed)
* Deprecated `StringUtils.htmlEscape` - use `_.escape`
* Deprecated `CollectionUtils.indexOf` - use `_.findIndex`
* Deprecated `CollectionUtils.forEach` - use `_.forEach`
* Deprecated `CollectionUtils.some` - use `_.some`
* Deprecated `CollectionUtils.hasProperty` - use `_.has`

The deprecated functions now simply call over to the corresponding Lo-Dash function.

**Image files** - `getCurrentDocument()` and `getActiveEditor()` will return null any time an image is open. Use `getCurrentlyViewedPath()` if you need to get the current file even when it's an image. See the [Preview Images Spec](https://github.com/adobe/brackets/wiki/Preview-Images-Spec) for details.

**Inline editors** - `InlineTextEditor.editors` (array of Editors) was removed and replaced with `InlineTextEditor.editor` (single Editor). With current usage, there was never more than a single element in the array anyway, so this simplifies the API.

All InlineWidget subclasses now have a close button automatically inserted at a fixed position in the upper-left. Ensure your widget's UI stays clear of this area.

**Extension load order** - User/dev extensions are now loaded after _all_ built-in Brackets extensions have loaded. Previously, the order was unpredictable - making menu item order inconsistent.

**Dialogs** - Modal Dialogs are now [auto-centered](https://github.com/adobe/brackets/pull/5399) both horizontally and vertically over the Brackets Window. Extensions that set custom margins on a dialog's top level DOM node may conflict with this.

**Files** - `FileUtils.canonicalizeFolderPath()` is deprecated (it actually makes paths **not** canonical: the standard format used by DirectoryEntry.fullPath includes a trailing "/", while this function removes the trailing "/"). Use `stripTrailingSlash()` if you need to explicitly _un_-normalize a path.

`ProjectManager.getInitialProjectPath()` and `updateWelcomeProjectPath()` now include a trailing "/" as well (previously they did not). (No known extensions use these APIs).

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
* [Replace with regexp subexpression substitutions ($1, etc.)](https://github.com/adobe/brackets/pull/5618) by [Marcel Gerber](https://github.com/SAPlayer)
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