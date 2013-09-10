_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 23 -- approximately September 18._

What's New in Sprint 31
-----------------------
* **Live Preview**
    * [Live Preview HTML changes](https://trello.com/c/ya9wexlA/998-2-improve-html-live-development-performance): Live Preview updates in real time as you type in HTML files. Updates pause whenever the HTML is not syntactically valid.
* **Overall UI**
    * [Compact, dark titlebar (Windows)](https://trello.com/card/5-into-darkness-shell-windows/4f90a6d98f77505d7940ce88/874): A similar change will be [coming soon for Mac](https://trello.com/card/into-darkness-shell-osx/4f90a6d98f77505d7940ce88/900) too.
    * [Keyboard shortcut to switch between recent projects](https://github.com/adobe/brackets/pull/4546): Press Ctrl+Alt+R (&#x2325;⌘R) to open the "recent projects" dropdown, then use the arrows and Enter to switch projects.
* **SCSS Support**
    * [Code hints for CSS property names & values](https://github.com/adobe/brackets/pull/4931): The same code hints you see in CSS files will now also appear in SCSS files.
    * [Quick Docs for CSS properties](https://github.com/adobe/brackets/pull/5069): Ditto &ndash; just like in CSS.
* **HTML Editing**
    * [Highlight matching open/close tag](https://github.com/adobe/brackets/pull/4504)
* **Search**
    * [Update Find in Files results while typing](https://github.com/adobe/brackets/pull/4729)
* **Linting**
    * [Extensibility for linting tools](https://github.com/adobe/brackets/pull/4588)
* **Linux Support**
    * [Linux universal installer](https://github.com/adobe/brackets-shell/pull/316)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-30...sprint-31#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-30...sprint-31#commits_bucket)


UI Changes
----------
TODO: Windows shell appearance

TODO: [UI indicating](https://trello.com/c/cmkAdt20/985-2-live-development-html-ui-for-invalid-state) HTML is invalid & won't be syncing


API Changes
-----------
TODO: Node upgrade

New/Improved Extensibility APIs
-------------------------------
**Linting** - Use `CodeInspection.register()` to provide linting/inspection for a given Language. Just like the built-in JSLint functionality, the provider is invoked whenever a file is opened or saved, and its results are displayed in a panel below the editor (providers may be run more frequently in the future, however). Currently, only one provider is accepted per language, although extensions _can_ replace the default JSLint provider for JavaScript.


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
* A new brackets-shell build is required _only_ to enable the new Windows border appearance.
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and _then_ `git submodule update` to ensure your local source tree reflects the update.

Bugs fixed in Sprint 31
-----------------------
For details on the bugs addressed, please refer to [closed sprint 31 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=18&state=closed). A few of the fixed bugs might not be caught by this search query, however.