_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 30 -- approximately August 28._

What's New in Sprint 30
-----------------------
* **Mac OS 10.6 support restored**: Brackets once again runs on 10.6
* **Linux Preview Build**
    * Linux build now supports: Extension Manager (install extensions), Live Preview Highlighting, saving Untitled documents, Save As, Delete file/folder, various extensions that require Node. Several install issues fixed.
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
**Live Development** - See [Live Preview API wiki page](https://github.com/adobe/brackets/wiki/Live-Preview-API)

New/Improved Extensibility APIs
-------------------------------
**Async utils** - The new `Async.chain()` API works much like `Async.doSequentially()`, but with an array of different functions whose results are piped together (rather than a single homogenous function repeated over a static array of data).

**Editor.setCursorPos()** - New, optional `expandTabs` argument similar to the one in `getCursorPos()`.

Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. See workaround https://support.google.com/chrome/answer/106010?hl=en.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Replace All](https://github.com/adobe/brackets/pull/4686) ([and](https://github.com/adobe/brackets/pull/4814)) by [Martin Zagora](https://github.com/zaggino)
* [Linux: Implement Delete file/folder](https://github.com/adobe/brackets-shell/pull/304) by [eyelash](https://github.com/eyelash)
* [Active line highlight: Hide when text is selected](https://github.com/adobe/brackets/pull/4878) ([and](https://github.com/adobe/brackets/pull/4927)) by [Lance Campbell](https://github.com/lkcampbell)
* [Active line highlight: Cover line numbers too; hide when not focused](https://github.com/adobe/brackets/pull/4887) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix tabbing through dialog box controls, improve layering of nested dialogs](https://github.com/adobe/brackets/pull/4714) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Linux: Fix 'hacking on Brackets' scripts](https://github.com/adobe/brackets/pull/4810) by [Andreas Renberg](https://github.com/IQAndreas)
* [Fix #4781: After stopping Replace, next Find isn't prepopulated with selection](https://github.com/adobe/brackets/pull/4782) by [Martin Zagora](https://github.com/zaggino)
* [Fix #4784: Correct singular strings in Find bar](https://github.com/adobe/brackets/pull/4853) by [Corné Dorrestijn](https://github.com/cornedor)
* [Fix #4762: `word-break` missing from CSS hints](https://github.com/adobe/brackets/pull/4771) by [Tom Erik Støwer](https://github.com/testower)
* [Fix truncation when Find/Find in Files string very wide](https://github.com/adobe/brackets/pull/4661) by [Martin Zagora](https://github.com/zaggino) (fix not visible yet - see [#4940](https://github.com/adobe/brackets/pull/4940))
* [Allow theme extensions to style active line highlight](https://github.com/adobe/brackets/pull/4806) by [John Amiah Ford](https://github.com/johnamiahford)
* [Support tab expansion in Editor.setCursorPos()](https://github.com/adobe/brackets/pull/4719) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix unit test failure when indentation is set to tabs](https://github.com/adobe/brackets/pull/4805) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #4750: Debug > Switch Language ignores Cancel button](https://github.com/adobe/brackets/pull/4753) by [Tomás Malbrán](https://github.com/TomMalbran)
* [UI tweak to Debug > Show Performance Data results](https://github.com/adobe/brackets/pull/4902) by [Robert Gawdzik](https://github.com/rgawdzik)
* [German translation update](https://github.com/adobe/brackets/pull/4639) ([part 2](https://github.com/adobe/brackets/pull/4754), [part 3](https://github.com/adobe/brackets/pull/4835)) by [mynetx](https://github.com/mynetx)
* [Spanish translation update](https://github.com/adobe/brackets/pull/4935) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Swedish translation update](https://github.com/adobe/brackets/pull/4691) by [Mikael Jorhult](https://github.com/mikaeljorhult)
* [Turkish translation update](https://github.com/adobe/brackets/pull/4820) by [Veysi Ertekin](https://github.com/veysiertekin)
* [Turkish translation update](https://github.com/adobe/brackets/pull/4355) by [Mahmut Bulut](https://github.com/vertexclique)
* [Czech translation update](https://github.com/adobe/brackets/pull/4967) by [kvarel](https://github.com/kvarel)

#### Pulling source code from Git
* A new brackets-shell build is _required_ for this sprint. Be sure to rerun `grunt setup` before building.
* brackets-shell's Node dependencies have changed. Run `npm install` before rebuilding brackets-shell.
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and then `git submodule update --init --recursive` to ensure your local source tree reflects the update.
* The Windows "Open With" feature noted above _only_ works with installed builds of Brackets. Copies of brackets-shell you build yourself will not appear in the "Open With" list.

Bugs fixed in Sprint 30
-----------------------
For details on the bugs addressed, please refer to [closed sprint 30 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=17&state=closed). A few of the fixed bugs might not be caught by this search query, however.