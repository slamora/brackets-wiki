_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 23 -- approximately April 10._

What's New in Sprint 23
-----------------------
* **TODO: category heading**
    * [Live Development: highlight elements in browser from HTML code](https://trello.com/card/5-live-development-highlight-html-elements-in-browser-from-html/4f90a6d98f77505d7940ce88/565)
    * [Extension publishing workflow](https://trello.com/card/5-extension-publishing/4f90a6d98f77505d7940ce88/788)
    * [Toolbar redesign](https://trello.com/card/2-ux-implement-toolbar/4f90a6d98f77505d7940ce88/785)

* **Localization**
    * [Czech translation (cs)](https://github.com/adobe/brackets/pull/3150)
    * [Polish translation (pl)](https://github.com/adobe/brackets/pull/3235)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-22...sprint-23#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-22...sprint-23#commits_bucket)


UI Changes
----------
**TODO - toolbar**


API Changes
-----------

New/Improved Extensibility APIs
-------------------------------

* `QuickOpen.addQuickOpenPlugin(pluginDef)` now expects `pluginDef.languageIds` instead of `pluginDef.fileTypes`. Using the latter still works for now, but is deprecated and will print a warning to the console. For HTML and CSS, only the key needs to be changed, for JavaScript, `"js"` needs to be changed to `"javascript"`.

Known Issues
------------
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.

Community contributions to Brackets
-----------------------------------

Contributions _from_ the Brackets team
--------------------------------------

Bugs fixed in Sprint 23
-----------------------
For details on the bugs addressed, please refer to [closed sprint 23 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=10&state=closed). A few of the fixed bugs might not be caught by this search query, however.