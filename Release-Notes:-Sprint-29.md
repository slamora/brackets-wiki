_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 29 -- approximately August 9._

What's New in Sprint 29
-----------------------
* **Live Preview**
    * [Live preview HTML changes as you type](https://trello.com/c/cc8kk9zG/927-5-live-development-html-initial-implementation)
* **Research**
    * [Design extensibility API revamp](https://trello.com/c/rnN0XwK0/876-3-research-extension-api-design)
    * [Investigate crashes blocking CEF upgrade](https://trello.com/c/gIwbocii/938-3-cef-crash-issues)
    * [Continue work on Windows titlebar restyling](https://trello.com/c/d77Fd4F9/874-5-into-darkness-shell-windows)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-28...sprint-29#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-28...sprint-29#commits_bucket)


UI Changes
----------
No major changes to existing features.


API Changes
-----------

New/Improved Extensibility APIs
-------------------------------


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

Contributions _from_ the Brackets community
-------------------------------------------
* [Fix for issue #4562](https://github.com/adobe/brackets/pull/4569) by [Lance Campbell](https://github.com/lkcampbell)
* [Implementation of beforeAll/afterAll](https://github.com/adobe/brackets/pull/4581) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Improvements to the Inline Editor Tests](https://github.com/adobe/brackets/pull/4598) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Confirm folder delete - fixes issue #4446](https://github.com/adobe/brackets/pull/4515) by [Greg Palmer](https://github.com/g-palmer)
* [Upgrade JSLint to the commit we were using before switching to a submodule](https://github.com/adobe/brackets/pull/4642) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Find in Files Improvements (Part 2: Pagination)](https://github.com/adobe/brackets/pull/4303) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Added Finnish language](https://github.com/adobe/brackets/pull/4506) by [valtlait](https://github.com/valtlait)
* [sprint 27 - updated czech language](https://github.com/adobe/brackets/pull/4398) by [kvarel](https://github.com/kvarel)
* [Make swedish translation up-to-date.](https://github.com/adobe/brackets/pull/4605) by [Mikael Jorhult](https://github.com/mikaeljorhult)
* [Fix bug #4536, No tooltip for author name in extension manager](https://github.com/adobe/brackets/pull/4622) by [Luan Pham](https://github.com/thanhluan001)
* [Update German Localization](https://github.com/adobe/brackets/pull/4520) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [sprint 28 - czech language update](https://github.com/adobe/brackets/pull/4607) by [kvarel](https://github.com/kvarel)

Bugs fixed in Sprint 29
-----------------------
For details on the bugs addressed, please refer to [closed sprint 29 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=16&state=closed). A few of the fixed bugs might not be caught by this search query, however.