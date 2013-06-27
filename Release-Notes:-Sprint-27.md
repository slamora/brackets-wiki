_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 27 -- approximately June 27._

What's New in Sprint 27
-----------------------
* **File Management**
    * [Save As](https://trello.com/c/wxmFpxW3): Create a new file from an existing file using the File > Save As… menu, project tree context menu, or with the keyboard shortcut Ctrl+Shift+S or ⇧⌘S.
* **Code Editing**
    * [Updated Quick View Gradient Support](https://github.com/adobe/brackets/issues/3458): Quick View for gradients now supports the "to" keyword for gradient direction and new repeating-linear-gradient and repeating-radial-gradient types.
* **Research**
    * [Data Structure for HTML DOM Edit Mapping](https://trello.com/c/lGIOrElQ): Research task to figure out how to do live HTML editing. 
    * [Drag and Drop](https://trello.com/c/PDyKD95J): Research task to figure out how to intercept native drag events on the mac so we can support dropping files onto the Brackets window. 
* **Architecture**
    * [Update Brackets Shell (CEF)](https://trello.com/c/YQlER69q): Update to the latest CEF, v 3.1453.1279.
* **Localization**
    * [Hungarian translation](https://github.com/adobe/brackets/pull/4282)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-26...sprint-27#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-26...sprint-27#commits_bucket)


UI Changes
----------
None

API Changes
-----------

**brackets.app**  
* `openURLInDefaultBrowser` -- The parameters for this API were reversed from what was the convention for the other APIs. This was confusing so to conform to convention and accommodate an optional callback, the parameters for this API were normalized.  The API usage is now `brackets.app.openURLInDefaultBrowser(url, err)` where `err` is an optional function callback that takes 1 argument.

**brackets.fs**
* `brackets.fs.showSaveDialog` -- Shows a modal dialog for selecting a new file name.
* Callbacks for most `brackets.fs` methods are now optional.

**NativeFileSystem**
* `showSaveDialog` -- Shows a modal dialog for selecting a new file name.

**Editor**

* `optionChange` event -- Triggered when an option for the editor is changed. See [Add Editor optionChange event](https://github.com/adobe/brackets/pull/4162).

**DocumentManager**

* [Two indistinguishable events for different cases of working set reordering](https://github.com/adobe/brackets/pull/3080) by [Tomás Malbrán](https://github.com/TomMalbran).
    * `workingSetReorder` renamed to `workingSetDisableAutoSorting`. 
* `Document` class moved from `document/DocumentManager` to `document/Document`.

New/Improved Extensibility APIs
-------------------------------
None

Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3570](https://github.com/adobe/brackets/issues/3570): Mac only - Quick View popover may not appear after resizing window or going fullscreen. Move the mouse to the top of the screen to fix.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. See workaround https://support.google.com/chrome/answer/106010?hl=en.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Fix for issue #4137](https://github.com/adobe/brackets/pull/4166) by [Lance Campbell](https://github.com/lkcampbell)
* [Minor clean ups](https://github.com/adobe/brackets/pull/4059) by [Anatoly Shikolay](https://github.com/shikolay)
* [Add Editor optionChange event](https://github.com/adobe/brackets/pull/4162) by [Lance Campbell](https://github.com/lkcampbell)
* [Move the Project Preferences Dialog variables to the template](https://github.com/adobe/brackets/pull/3286) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #2076: Two indistinguishable events for different cases of working set reordering](https://github.com/adobe/brackets/pull/3080) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Do not copy entire Strings Object; pass by reference instead](https://github.com/adobe/brackets/pull/4260) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Translate in French the localized text](https://github.com/adobe/brackets/pull/4005) by [Florian M Valence](https://github.com/FloValence)
* [Fix for Issue 3891: Extension name and description should be selectable](https://github.com/adobe/brackets/pull/4284) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Spanish strings for Sprint 26](https://github.com/adobe/brackets/pull/4286) by [Chema Balsas](https://github.com/jbalsas)
* [Update Linux to CEF 3.1453.1255](https://github.com/adobe/brackets-shell/pull/264) by [Chhatoi Pritam Baral](https://github.com/pritambaral), [radorodopski](https://github.com/radorodopski)
* [Adding groovy syntax highlighting.](https://github.com/adobe/brackets/pull/4322) by [Arturo Elias](https://github.com/arturoeanton)
* [Update 'de' locale](https://github.com/adobe/brackets/pull/4279) by [J.M.](https://github.com/mynetx)
* [Hungarian language pack for Brackets](https://github.com/adobe/brackets/pull/4282) by [rigor789](https://github.com/rigor789)
* [Fix for Issue 4265: Mac: KeyBindingManager displays errant error message](https://github.com/adobe/brackets/pull/4305) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)

Contributions _from_ the Brackets community
-------------------------------------------

* None

Bugs fixed in Sprint 27
-----------------------
For details on the bugs addressed, please refer to [closed sprint 27 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=14&state=closed). A few of the fixed bugs might not be caught by this search query, however.