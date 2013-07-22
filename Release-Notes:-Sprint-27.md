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


API Changes
-----------
**DocumentManager**
* `Document` class moved from `document/DocumentManager` to new `document/Document` module. Should _not affect_ most code because, while document objects are widely used, the `Document` identifier itself is rarely referenced directly.
* `"workingSetReorder"` event renamed to `"workingSetDisableAutoSorting"` (still has same semantics: fired when working set is reordered via manual drag & drop). [See pull request](https://github.com/adobe/brackets/pull/3080) for details.

**brackets.app**  
* `openURLInDefaultBrowser()` -- Parameters reversed to match other APIs - URL now comes 1st, optional error callback 2nd. Most code uses <code><i>NativeApp</i>.openURLInDefaultBrowser()</code> instead, and is _not affected_ by this change.

New/Improved Extensibility APIs
-------------------------------
**Editor**
* `"optionChange"` event -- Triggered when an option for the editor is changed. [See pull request](https://github.com/adobe/brackets/pull/4162) for details. (Note: this may be deprecated when a full preferences API becoems available).

**NativeFileSystem**
* `showSaveDialog()` -- Shows a modal dialog for selecting a new file name.

**brackets.fs**
* Callbacks for most `brackets.fs` methods are now optional. Note: this is a low-level API; most code should use NativeFileSystem instead.



Known Issues
------------
* This build _does not support Mac OS 10.6 (Snow Leopard)_. We are hoping to restore 10.6 support very soon (see [#4394](https://github.com/adobe/brackets/issues/4394)).
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. See workaround https://support.google.com/chrome/answer/106010?hl=en.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Update experimental Linux build to CEF 3.1453.1255](https://github.com/adobe/brackets-shell/pull/264) by [Chhatoi Pritam Baral](https://github.com/pritambaral), [radorodopski](https://github.com/radorodopski)
* [Add Editor optionChange event](https://github.com/adobe/brackets/pull/4162) by [Lance Campbell](https://github.com/lkcampbell)
* [Add Groovy syntax highlighting](https://github.com/adobe/brackets/pull/4322) by [Arturo Elias](https://github.com/arturoeanton)
* [Improve .ejs syntax highlighting](https://github.com/adobe/brackets/pull/4166) by [Lance Campbell](https://github.com/lkcampbell)
* [Fix #2076: Change the events fired when working set is reordered](https://github.com/adobe/brackets/pull/3080) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Move Project Preferences Dialog variables to the template](https://github.com/adobe/brackets/pull/3286) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Template optimization: do not copy entire Strings Object; pass by reference instead](https://github.com/adobe/brackets/pull/4260) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #3891: Extension name and description should be selectable](https://github.com/adobe/brackets/pull/4284) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Minor code clean ups](https://github.com/adobe/brackets/pull/4059) by [Anatoly Shikolay](https://github.com/shikolay)
* [Add Hungarian translation](https://github.com/adobe/brackets/pull/4282) by [rigor789](https://github.com/rigor789)
* [Spanish translation update](https://github.com/adobe/brackets/pull/4286) by [Chema Balsas](https://github.com/jbalsas)
* [German translation update](https://github.com/adobe/brackets/pull/4279) by [J.M.](https://github.com/mynetx)
* [Use actual French in localization example](https://github.com/adobe/brackets/pull/4005) by [Florian M Valence](https://github.com/FloValence)


Bugs fixed in Sprint 27
-----------------------
For details on the bugs addressed, please refer to [closed sprint 27 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=14&state=closed). A few of the fixed bugs might not be caught by this search query, however.