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
**TODO** - utility API deprecations/removals due to lodash integration

**Image files** - TODO

**Inline editors** - TODO: automatically-created close button; `InlineTextEditor.editors` removal

**Files** - TODO: trailing-"/" cleanups; deprecated `FileUtils.canonicalizeFolderPath()`


New/Improved Extensibility APIs
-------------------------------
**Lodash** - TODO

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
TBD

#### Pulling source code from Git
more TBD
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Sprint 33
-----------------------
For details on the bugs addressed, please refer to [closed sprint 33 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=20&state=closed). A few of the fixed bugs might not be caught by this search query, however.