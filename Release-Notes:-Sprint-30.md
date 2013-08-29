_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 30 -- approximately August 28._

What's New in Sprint 30
-----------------------
* **Mac OS 10.6 support restored**: Brackets once again runs on 10.6
* **Linux Preview Build**
    * Linux build now supports: Extension Manager (install extensions), Live Preview Highlighting, saving Untitled documents, Save As, extensions that require Node. Several install issues fixed.
    * _Warning: The Linux build is still a preview_ &ndash; it doesn't have all the features and stability of the Mac/Windows builds yet.
* **Find/Replace**
    * [Replace All](https://github.com/adobe/brackets/pull/4686): Click "All" in the Replace bar to display a panel showing all occurrences that will be replaced. You can review and uncheck any occurrences you don't want to change, then replace all the rest with a single click.
* **JavaScript Editing**
    * [Function parameter hints](https://github.com/adobe/brackets/pull/4637): Hints appear automatically when you first type inside `()`s, or when invoked manually with Ctrl+Shift+Space.
* **CSS Editing**
    * [Code hints for CSS Regions named flows](https://trello.com/c/gNvtHgu7/949-1-css-regions-named-flow): Hints names used in the same file
* **Files and Folders**
    * [Support "Open With..." in Windows](https://github.com/adobe/brackets-shell/pull/299): The file explorer context menu's Open With list now includes Brackets for all file types Brackets supports out of the box. (Already supported on Mac).
* **Ongoing Research** (not implemented yet)
    * [Live Preview HTML changes](https://trello.com/c/cc8kk9zG/927-5-live-development-html-initial-implementation): This is _almost_ ready to roll, but needs just a few more fixes and performance tweaks. If you'd like to try it out anyway, open the dev tools and run `brackets.livehtml = true` just after launching Brackets.
    * [Plan extensibility API revamp](https://trello.com/c/rnN0XwK0/876-3-research-extension-api-design): See [summary on brackets-dev](https://groups.google.com/forum/#!topic/brackets-dev/c5626MgNQG4).


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-29...sprint-30#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-29...sprint-30#commits_bucket)


UI Changes
----------
**Blue selected-file highlight** - The selection marker indicating the currently open file now shows the file name in "Brackets blue" text, matching the Brackets logo.

**Editor current-line highlight** - Line highlight is now hidden whenever there's a text selection, to improve contrast and reduce confusion with whole-line selections.


API Changes
-----------
**Live Development** - TODO https://github.com/adobe/brackets/pull/4801

New/Improved Extensibility APIs
-------------------------------
**Async utils** - The new `Async.chain()` API works much like `Async.doSequentially()`, but with an array of different functions whose results are piped together (rather than a single homogenous function repeated over a static array of data).

Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. See workaround https://support.google.com/chrome/answer/106010?hl=en.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------


#### Pulling source code from Git
* A new brackets-shell build is _required_ for this sprint. Be sure to rerun `grunt setup` before building.
* brackets-shell's Node dependencies have changed. Run `npm install` before rebuilding brackets-shell.
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and then `git submodule update --init --recursive` to ensure your local source tree reflects the update.
* The Windows "Open With" feature noted above _only_ works with installed builds of Brackets. Copies of brackets-shell you build yourself will not appear in the "Open With" list.

Contributions _from_ the Brackets community
-------------------------------------------

Bugs fixed in Sprint 30
-----------------------
For details on the bugs addressed, please refer to [closed sprint 30 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=17&state=closed). A few of the fixed bugs might not be caught by this search query, however.