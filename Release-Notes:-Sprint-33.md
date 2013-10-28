_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 33 -- approximately October 29._

What's New in Sprint 33
-----------------------
* **Quick Edit**
    * [Create new CSS rules via Quick Edit](https://trello.com/c/5I5AddGo/599-5-css-quick-edit-create-new-selector): In HTML files, click the new "New Rule" button in the Quick Edit inline editor to create a new CSS rule based on the tag/class/id your cursor was on. Fully keyboard accessible using Cmd/Ctrl-Alt-N. The inline editor now appears even when no existing rules match the search, so you can easily create new CSS rules at any time.
    * [Visually edit CSS transition timing function Bezier curves](https://trello.com/c/5EPJdO1q/838-2-quick-edit-css-cubic-bezier): Just invoke Quick Edit when your cursor is on any `cubic-bezier()`, `linear`, `ease`, `ease-in`, `ease-out`, or `ease-in-out` functions in a CSS rule!
* **Images**
    * [Preview image files](https://trello.com/c/l9AcILkC/24-8-preview-images): Select in image in the file tree (or via Quick Open) to see a preview in the editor area.
* **Overall UI**
    * [Dark themed window chrome on Mac](https://trello.com/c/oyGfEvrK/900-3-into-darkness-shell-osx): Similar to the update on Windows in Sprint 31.
* **Files and Folders**
    * [Close Others [Above/Below] in Working Files context menu](https://github.com/adobe/brackets/pull/4590): Quickly close batches of files with these three new commands.
* **Localization**
    * [Greek translation added](https://github.com/adobe/brackets/pull/5378)
    * [Dutch translation added](https://github.com/adobe/brackets/pull/5372)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-32...sprint-33#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-32...sprint-33#commits_bucket)


UI Changes
----------
**Dark-themed window chrome on Mac** - the Mac shell now has a dark window chrome that visually complements the Brackets UI. (The Windows shell received a similar update in Sprint 31).

**Dialogs** - Modal Dialogs are now [auto-centered](https://github.com/adobe/brackets/pull/5399) both horizontally and vertically over the Brackets Window.


API Changes
-----------
**Lo-Dash** - added as a third-party dependency. The following conversions and deprecations were made:

* Convert `Array.slice(0)` to `_.clone()`
* Convert `Async.whenIdle` to `_.debounce`.
* Convert `NumberUtils.getRandomNumber()` to `_.random` and removed `NumberUtils.js`
* Convert `StringUtils.htmlEscape` to `._escape` and deprecated it.
* Convert `CollectionUtils.indexOf` to `_.findIndex` and deprecated it.
* Convert `CollectionUtils.forEach` to `_.forEach` and deprecated it.
* Convert `CollectionUtils.some` to `_.some` and deprecated it.
* Convert `CollectionUtils.hasProperty` to `_.has` and deprecated it.

Functions are deprecated by adding a `@deprecated` annotation, and by replacing their implementation with the corresponding Lo-Dash function and a `console.warn` message.


**Image files** - TODO

**Inline editors** - TODO: automatically-created close button; `InlineTextEditor.editors` removal

**Files** - TODO: trailing-"/" cleanups; deprecated `FileUtils.canonicalizeFolderPath()`


New/Improved Extensibility APIs
-------------------------------
**Lo-Dash** - [utility library](http://lodash.com/) is now available in Brackets.

**Closing files** - TODO: new DocumentCommandHandlers API for closing multiple files in a batch


Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. [See workaround](https://support.google.com/chrome/answer/106010?hl=en).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Updated german translation](https://github.com/adobe/brackets/pull/5667) by [SAPlayer](https://github.com/SAPlayer)
* [German](https://github.com/adobe/brackets/pull/5657) [translation](https://github.com/adobe/brackets/pull/5567) of [samples](https://github.com/adobe/brackets/pull/5583) by [SAPlayer](https://github.com/SAPlayer)
* [Spanish strings for Sprint 33](https://github.com/adobe/brackets/pull/5676) by [jbalsas](https://github.com/jbalsas)
* Finnish translation [fixes](https://github.com/adobe/brackets/pull/5557), [more fixes](https://github.com/adobe/brackets/pull/5556) by [jukkah](https://github.com/jukkah)
* [Start live development if there is no open file](https://github.com/adobe/brackets/pull/5547) by [SAPlayer](https://github.com/SAPlayer)
* [Added Serbian translation](https://github.com/adobe/brackets/pull/5515) by [Gocilla](https://github.com/Gocilla)
[Fix sv Node translation](https://github.com/adobe/brackets/pull/5647) by [micole](https://github.com/micole)
* [css quick edit blank line number](https://github.com/adobe/brackets/pull/5582) by [thehogfather](https://github.com/thehogfather)
* [Clicking project-dropdown-toggle again closes the dropdown](https://github.com/adobe/brackets/pull/5435) by [SAPlayer](https://github.com/SAPlayer)
* [Recent Projects - Pressing delete key removes project from list](https://github.com/adobe/brackets/pull/5354) by [sandeepjain](https://github.com/sandeepjain)
* [Fix wrong path of Getting Started directory in Polish version](https://github.com/adobe/brackets/pull/5471) by [mateuszgachowski](https://github.com/mateuszgachowski)
* [remove all menu items and dividers prior to removing menu](https://github.com/adobe/brackets/pull/5384) by [lkcampbell](https://github.com/lkcampbell)
* [Provide i18n for InlineBezierCurveEditor](https://github.com/adobe/brackets/pull/5553) by [SAPlayer](https://github.com/SAPlayer)
* [Change not.toBeNull() to toBeTruthy() in unit tests](https://github.com/adobe/brackets/pull/5492) by [gmeluski](https://github.com/gmeluski)
* [Fix URL hints not showing](https://github.com/adobe/brackets/pull/5422) by [WebsiteDeveloper](https://github.com/WebsiteDeveloper)
* [Update 'de' locale](https://github.com/adobe/brackets/pull/5470) by [mynetx](https://github.com/mynetx)
* [czech language](https://github.com/adobe/brackets/pull/5510) by [kvarel](https://github.com/kvarel)
* [Fix: Quick View popup closes & reopens...](https://github.com/adobe/brackets/pull/5428) by [SAPlayer](https://github.com/SAPlayer)
* [Standardize the format of nls files](https://github.com/adobe/brackets/pull/5505) by [SAPlayer](https://github.com/SAPlayer)
* [Fix layout issue](https://github.com/adobe/brackets/pull/5484) by [WebsiteDeveloper](https://github.com/WebsiteDeveloper)
* [Removed all references to CodeHintManager from Editor](https://github.com/adobe/brackets/pull/5421) by [WebsiteDeveloper](https://github.com/WebsiteDeveloper)
* [Find in Files - No result](https://github.com/adobe/brackets/pull/5477) by [cosmosgenius](https://github.com/cosmosgenius)
* [Inline Editor Close Button](https://github.com/adobe/brackets/pull/5443) by [VizuaaLOG](https://github.com/VizuaaLOG)
* [Remove preferences migration code](https://github.com/adobe/brackets/pull/5429) by [WebsiteDeveloper](https://github.com/WebsiteDeveloper)
* [Fix Tests for non English locales](https://github.com/adobe/brackets/pull/5433) by [WebsiteDeveloper](https://github.com/WebsiteDeveloper)

#### Pulling source code from Git
more TBD
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Sprint 33
-----------------------
For details on the bugs addressed, please refer to [closed sprint 33 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=20&state=closed). A few of the fixed bugs might not be caught by this search query, however.