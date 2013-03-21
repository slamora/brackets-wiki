_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 22 -- approximately March 22._

What's New in Sprint 22
-----------------------
* Extensions
    * [Install extension from URL](https://trello.com/card/3-extension-installation-url/4f90a6d98f77505d7940ce88/789) in the Brackets UI via File > Install Extensions\u2026. A URL to a ZIP file or GitHub repo is required.
* Code Editing
    * [Word wrap](https://trello.com/card/2-word-wrap/4f90a6d98f77505d7940ce88/270) is now enabled by default. Use the View > Enable Word Wrap menu to toggle.
    * Show Active Line: Active line highlighting emphasizes which line the cursor is positioned on. Use the View > Show Active Line menu to toggle.
    * Line numbers can now be hidden. Use the View > Show Line Numbers menu to toggle.
* Live Development
    * [Improvements and bug fixes](https://trello.com/c/R0ZFrFV4) were made to make the connection to Chrome more reliable and to report errors more effectively.

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-21...sprint-22#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-21...sprint-22#commits_bucket)


UI Changes
----------
No major changes to existing features.


API Changes
-----------

New/Improved Extensibility APIs
-------------------------------
* Programming languages can now also be associated with file names (e.g. "Makefile") in addition to file extensions. Check `language/LanguageManager.js` for examples.
* Extensions can add file names and extensions to existing languages.

Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.


Community contributions to Brackets
-----------------------------------

* [Fix issue #2306 (Recent projects close arrow fail to close without mouse move)](https://github.com/adobe/brackets/pull/2590) by [Chema Balsas](https://github.com/jbalsas)
* [Preferences ID cleanups - Part II](https://github.com/adobe/brackets/pull/3101) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Adding support for multiple lineComment prefixes](https://github.com/adobe/brackets/pull/3121) by [Tomás Malbrán](https://github.com/TomMalbran)
* [add zh-cn language!](https://github.com/adobe/brackets/pull/3107) by [yodfz](https://github.com/yodfz)
* [Fix #1098: Some code uses $.each, some uses Array.prototype.forEach](https://github.com/adobe/brackets/pull/3087) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fixed: #1863 Selector width info is wrong when no space before '{'](https://github.com/adobe/brackets/pull/3082) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fixing issue #2923 - Getting mode from file extension won't always work](https://github.com/adobe/brackets/pull/3029) by [Mark Murphy](https://github.com/MarkMurphy)
* [edit nls/string.js and delete file](https://github.com/adobe/brackets/pull/3112) by [yodfz](https://github.com/yodfz)
* [Scroll line up/down - Take 3](https://github.com/adobe/brackets/pull/3068) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fixed: #3032 KeyBindingManager no longer warns / prevents binding collisions](https://github.com/adobe/brackets/pull/3081) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix issue 2078](https://github.com/adobe/brackets/pull/3058) by [Lance Campbell](https://github.com/lkcampbell)
* [Added Localisation for the Loading... message](https://github.com/adobe/brackets/pull/3020) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix for #3095 (German translation)](https://github.com/adobe/brackets/pull/3096) by [Markus Siemens](https://github.com/msiemens)
* [Updated italian translation](https://github.com/adobe/brackets/pull/2996) by [antonellopasella](https://github.com/antonellopasella)
* [Preferences id cleanup](https://github.com/adobe/brackets/pull/3018) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Persisting font size between Brackets Sessions](https://github.com/adobe/brackets/pull/) by [Lance Campbell](https://github.com/lkcampbell)
* [Cleanup for Adding the CodeMirror built-in Auto Close Brackets addon](https://github.com/adobe/brackets/pull/3075) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Adding the CodeMirror built-in Auto Close Brackets addon](https://github.com/adobe/brackets/pull/3040) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fixed: Issue #2878 Expanding/collapsing a folder tree node steals focus from editor](https://github.com/adobe/brackets/pull/2879) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Changed to check for case insensitive http: and https:](https://github.com/adobe/brackets/pull/3019) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fixed an Error showing up in the dev console when executing a disabled command](https://github.com/adobe/brackets/pull/3045) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fixed: #2278 Editor should not have upward dependencies on EditorManager](https://github.com/adobe/brackets/pull/2750) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)

Contributions _from_ Brackets
-----------------------------

Bugs fixed in Sprint 22
-----------------------
For details on the bugs addressed, please refer to [closed sprint 22 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=9&state=closed). A few of the fixed bugs might not be caught by this search query, however.