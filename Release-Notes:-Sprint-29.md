What's New in Sprint 29
-----------------------
This sprint didn't turn out quite the way we expected! We worked on several features that became more complex than expected, and weren't ready in time to include in this sprint (see "Ongoing Research" below). As a result, there are fewer new features than usual &ndash; but hopefully more next time!

* **Search**
    * [Pagination for unlimited Find in Files results](https://github.com/adobe/brackets/pull/4303): Click the next/previous arrows to navigate through long results.
    * Double-click Find in Files results to add to Working Files, just like double-clicking in file tree
* **Files and Folders**
    * [Show folder name when several Working Files items have same name](https://github.com/adobe/brackets/pull/4419)
    * [Support "Open With..." in OS X Finder](https://github.com/adobe/brackets-shell/pull/293): Previously available for a very limited set of file extensions, "Open With" now works with all file types Brackets supports out of the box.
* **Editing**
    * [Status bar shows length of text selection](https://github.com/adobe/brackets/pull/4579)
* **Localization**
    * [Finnish translation added](https://github.com/adobe/brackets/pull/4506)
* **Ongoing Research** (not available yet)
    * [Prototype live HTML editing](https://trello.com/c/cc8kk9zG/927-5-live-development-html-initial-implementation)
    * [Plan extensibility API revamp](https://trello.com/c/rnN0XwK0/876-3-research-extension-api-design)
    * [Investigate crashes blocking CEF upgrade](https://trello.com/c/gIwbocii/938-3-cef-crash-issues)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-28...sprint-29#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-28...sprint-29#commits_bucket)


UI Changes
----------
**Confirm folder delete** - A confirmation dialog will appear when attempting to delete a whole folder. As before, deleted files/folders are sent to the trash, so you can still recover anything you delete this way.

**Working Files** - When two open files (in the "Working Files" list) have the same file name, the folder name is shown after a hyphen to make the labels less ambiguous.

API Changes
-----------
No changes to existing APIs.

New/Improved Extensibility APIs
-------------------------------
**Unit tests** - We've extended the Jasmine API to add `beforeFirst()` and `afterLast()`, useful for setting up & tearing down anything shared across an entire test suite. Especially useful with `SpecRunnerUtils.createTestWindowAndRun()`.

Known Issues
------------
* This build _does not support Mac OS 10.6 (Snow Leopard)_. We expect to restore 10.6 support in the next sprint (see [#4394](https://github.com/adobe/brackets/issues/4394)).
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. See workaround https://support.google.com/chrome/answer/106010?hl=en.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Show folder name when several Working Files items have same name](https://github.com/adobe/brackets/pull/4419) by [Martin Zagora](https://github.com/zaggino)
* [Pagination (next/previous buttons) for unlimited Find in Files results](https://github.com/adobe/brackets/pull/4303) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Add Finnish translation](https://github.com/adobe/brackets/pull/4506) by [valtlait](https://github.com/valtlait)
* [Confirmation prompt for folder deletion](https://github.com/adobe/brackets/pull/4515) by [Greg Palmer](https://github.com/g-palmer)
* [Big unit test improvement: share one Brackets window for multiple tests](https://github.com/adobe/brackets/pull/4581), greatly improving test speed ([part 2](https://github.com/adobe/brackets/pull/4598), [part 3](https://github.com/adobe/brackets/pull/4635/files)) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #4536: Add tooltip for author name in Extension Manager](https://github.com/adobe/brackets/pull/4622) by [Luan Pham](https://github.com/thanhluan001)
* [Fix regression in JSLint version](https://github.com/adobe/brackets/pull/4642) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #4562: Console warning at startup](https://github.com/adobe/brackets/pull/4569) by [Lance Campbell](https://github.com/lkcampbell)
* [Swedish translation for 'Getting Started' project](https://github.com/adobe/brackets/pull/4626) by [Mikael Jorhult](https://github.com/mikaeljorhult)
* [Swedish translation update](https://github.com/adobe/brackets/pull/4605) by [Mikael Jorhult](https://github.com/mikaeljorhult)
* [German translation update](https://github.com/adobe/brackets/pull/4520) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Czech translation update](https://github.com/adobe/brackets/pull/4607) mainly by [kvarel](https://github.com/kvarel)

#### Pulling source code from Git
* A new brackets-shell build is required _only_ to enable the OS X "Open With" functionality. You may need to restart Finder for the rebuilt .app package to appear in Open With menus.

Bugs fixed in Sprint 29
-----------------------
For details on the bugs addressed, please refer to [closed sprint 29 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=16&state=closed). A few of the fixed bugs might not be caught by this search query, however.