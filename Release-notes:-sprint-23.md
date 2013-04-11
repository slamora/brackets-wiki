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
    * [Czech translation](https://github.com/adobe/brackets/pull/3150)
    * [Polish translation](https://github.com/adobe/brackets/pull/3235)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-22...sprint-23#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-22...sprint-23#commits_bucket)


UI Changes
----------
**TODO - toolbar**


API Changes
-----------
**Code Hints** - Code hint providers are now registered to a list of _language IDs_ rather than a list of CodeMirror modes. For some languages (like HTML and CSS) these strings are the same; for others, you must update your code (e.g. for JavaScript, use `"javascript"` instead of `"js"`).

**Quick Open** - The `fileTypes` member of Quick Open plugins (passed to `QuickOpen.addQuickOpenPlugin()`) is deprecated. Please migrate to `languageIds` instead. For some languages, the language id is the same as the file extension; for others, you must update the string.

New/Improved Extensibility APIs
-------------------------------
**Languages** - File extensions can now contain a dot to support conventions like ".mustache.html" or ".coffee.md".


Known Issues
------------
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.

Community contributions to Brackets
-----------------------------------
* [Added HtmlSpecialCharsCodeHints as default extension](https://github.com/adobe/brackets/pull/3237) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix #3253: Active line highlight doesn't disappear in unfocused editor](https://github.com/adobe/brackets/pull/3274) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Add Brackets license to package.json](https://github.com/adobe/brackets/pull/3258) by [Lee Leathers](https://github.com/theoreticaLee)
* [Fix #3094: Refactor JSLint into an extension](https://github.com/adobe/brackets/pull/3134) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Saving the tabs settings per file/project](https://github.com/adobe/brackets/pull/3039) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Removed a warning when getLanguageForPath was called](https://github.com/adobe/brackets/pull/3169) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Add support for complex file extensions like ".coffee.md" or ".html.erb"](https://github.com/adobe/brackets/pull/3122) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Mention the possibility of numbers in language IDs in the docs](https://github.com/adobe/brackets/pull/3172) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Zh cn/yodfz](https://github.com/adobe/brackets/pull/3199) by [yodfz](https://github.com/yodfz)
* [Added the ruby html extension .erb to be reconized as html](https://github.com/adobe/brackets/pull/3083) by [madebytrolls](https://github.com/madebytrolls)
* [Fixed the usage of literal "..." instead of \u2026 in translations](https://github.com/adobe/brackets/pull/3282) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [czech translation - sprint 21](https://github.com/adobe/brackets/pull/3150) by [kvarel](https://github.com/kvarel)
* [Add support for complex file extensions like ".coffee.md" or ".html.erb"](https://github.com/adobe/brackets/pull/3285) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Another two straightforward refactoring tasks for Language support](https://github.com/adobe/brackets/pull/3301) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Remove welcomeProjectManager cleanup code](https://github.com/adobe/brackets/pull/3248) by [Albert Xing](https://github.com/albertxing)
* [Exported Resizer DIRECTION_* and POSITION_* constants](https://github.com/adobe/brackets/pull/3290) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Polish Language](https://github.com/adobe/brackets/pull/3235) by [Rafal Borowski](https://github.com/rafalborowski)
* [Fix #3228: Codehints selection popup is positioned wrong when the font size is not standard](https://github.com/adobe/brackets/pull/3232) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Move the JSLint strings](https://github.com/adobe/brackets/pull/3315) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fixing the adjust font restore scroll and adding unit tests](https://github.com/adobe/brackets/pull/3300) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Use language id instead of CodeMirror mode in CodeHintManager](https://github.com/adobe/brackets/pull/3270) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Corrected some mistakes in Polish translation](https://github.com/adobe/brackets/pull/3360) by [niu tech](https://github.com/niutech)
* [Corrected some mistakes in Polish translation](https://github.com/adobe/brackets/pull/3361) by [niu tech](https://github.com/niutech)
* [More Editor Option Handlers Tests](https://github.com/adobe/brackets/pull/3231) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix for Issue 3339](https://github.com/adobe/brackets/pull/3340) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fixed: Issue: #2761 Project tree selection is incorrect after opening a file by means other than sidebar](https://github.com/adobe/brackets/pull/2869) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Spanish strings for sprint 23](https://github.com/adobe/brackets/pull/3393) by [Chema Balsas](https://github.com/jbalsas)

Contributions _from_ the Brackets community
-------------------------------------------
**Contributions to [CodeMirror](https://github.com/marijnh/CodeMirror):**
* [Don't auto-close apostrophes in comments](https://github.com/marijnh/CodeMirror/commit/c1b7ea4)

**Other:**
* Dropzone: [Make Dropzone message clickable](https://github.com/enyo/dropzone/pull/91)

Bugs fixed in Sprint 23
-----------------------
For details on the bugs addressed, please refer to [closed sprint 23 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=10&state=closed). A few of the fixed bugs might not be caught by this search query, however.