_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 36 -- approximately ~~January 16~~. Now delayed until roughly February 5 -- [see forum post](https://groups.google.com/forum/#!topic/brackets-dev/I3j_DrDXVVU)._

What's New in Sprint 36
-----------------------
* **Preferences**
    * [Project- and file-specific editor settings](https://trello.com/c/kqFFDqhR/523-3-infrastructure-for-project-file-scoped-preferences): Use JSON configuration files to specify different indentation, word wrap, etc. settings for different folders or different file types. (TODO: link to detailed how-to)
* **File System & Performance**
    * [File/directory watchers](https://trello.com/c/zldzEXmk/292-2-file-directory-watching): Brackets will detect external modifications to files almost instantly and update the project tree, editor contents, etc. automatically (manually refreshing the tree is no longer needed).
    * [File caching](https://trello.com/c/zldzEXmk/292-2-file-directory-watching): Operations like Find in Files will be _much_ faster when used repeatedly, as the contents of files are now cached in memory.
* **Extensions**
    * [Extension download counts](https://trello.com/c/qOy9Slr1/799-2-extension-download-counts): We will begin tracking how many times each extension has been installed, and will post the download counts at periodic intervals. This will help extension authors decide where to prioritize their efforts, and will help Brackets developers understand what functionality is most important for our users.
    * ["Safe Mode"](https://github.com/adobe/brackets/issues/5078): Choose _Debug > Reload Without Extensions_ to temporarily run Brackets without any extensions loaded for troubleshooting. Extensions are re-enabled when you restart Brackets through any other means.
* **LESS Support**
    * [Code hints for CSS property names & values](https://github.com/adobe/brackets/pull/6268): The same code hints you see in CSS & SCSS files will now also appear in LESS files.
    * [Quick Docs support](https://github.com/adobe/brackets/pull/6419): Ditto – just like in CSS/SCSS files.
* **CSS Editing**
    * [Visually edit CSS transition step timing functions](https://github.com/adobe/brackets/pull/5799): Invoke Quick Edit when your cursor is on any `steps()` function in a CSS rule to edit it (building on top of the existing visual `cubic-bezier()` editor). Also works in LESS & SCSS files.
* **Overall UI**
    * [Windows: many window-chrome visual glitches fixed](https://github.com/adobe/brackets-shell/pull/408): Notably, the window border no longer bleeds onto other monitors.
    * [Windows: new, flatter scrollbar appearance](https://github.com/adobe/brackets/pull/6305)
* **Linting**
    * [Multiple linting providers per language](https://github.com/adobe/brackets/pull/5235): If multiple providers are registered, they will all be run and the results consolidated.

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-35...sprint-36#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-35...sprint-36#commits_bucket)


UI Changes
----------
**Scrollbars** - Scrollbars on all versions of Windows now have a flatter, Windows-8-style appearance that matches the overall Brackets UI design better.


API Changes
-----------
TODO: Preferences API changes?

TODO: FileSystem API changes

**Linting** - The process for registering a linter is unchanged, but it's now possible to register more than one linter per file type. (Except that, as before, the built-in JSLint provider is automatically disabled when any other JS linter is registered).

The result yielded by `CodeInspection.inspectFile()` has changed: instead of a single linter result, it now returns an array of objects, each containing the result of one specific linter.

**StatusBar API changes** - `addIndicator()` now receives an additional parameter: insertBefore. If specified, the new statusbar indicator is inserted before the indicator with id of insertBefore. If omitted, the indicator will be inserted at the beginning.

New/Improved Extensibility APIs
-------------------------------


Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. [See workaround](https://support.google.com/chrome/answer/106010?hl=en).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
TODO

#### Pulling source code from Git
* A new brackets-shell build is _required_ for this sprint. If you have built brackets-shell before, **delete** the `brackets-shell\deps\node` folder before re-running `grunt setup` (this one-time step is required due to a change in the node executable filename).
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.

Bugs fixed in Sprint 36
-----------------------
For details on the bugs addressed, please refer to [closed sprint 36 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=23&state=closed). Not all fixed bugs will be caught by this search query, however.