What's New in Sprint 23
-----------------------
* **Live Preview**
    * [Highlight elements in browser from HTML code](https://trello.com/card/5-live-development-highlight-html-elements-in-browser-from-html/4f90a6d98f77505d7940ce88/565): While Live Preview is open, as you move the cursor around in an HTML file Brackets will highlight the corresponding element in the browser. Use "File > Live Highlight" to toggle this off.
* **Overall UI**
    * [Toolbar redesign](https://trello.com/card/2-ux-implement-toolbar/4f90a6d98f77505d7940ce88/785): The horizontal top toolbar has been replaced with a vertical side toolbar to make better use of precious vertical real estate. See below for details.
* **Code Editing**
    * [Code hints for HTML entities](https://github.com/adobe/brackets/pull/3237) (for example `&nbsp;`)
    * [.sh code coloring](https://github.com/adobe/brackets/pull/3348/files)
* **Search**
    * [Quick Open performance](https://github.com/adobe/brackets/pull/3184): now more responsive on large projects
* **Localization**
    * [Czech translation](https://github.com/adobe/brackets/pull/3150)
    * [Polish translation](https://github.com/adobe/brackets/pull/3235)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-22...sprint-23#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-22...sprint-23#commits_bucket)


UI Changes
----------
**Toolbar** - The white horizontal toolbar above the editor has been removed. Icons (such as the Live Preview lightning bolt) now appear in a vertical toolbar on the right. The current file name now appears in the window titlebar. This maximizes the vertical space available for viewing code.


API Changes
-----------
**Code Hints** - Code hint providers are now registered to a list of _language IDs_ rather than a list of CodeMirror modes. For some languages (like HTML and CSS) these strings are the same; for others, you must update your code (e.g. for JavaScript, use `"javascript"` instead of `"js"`).

**Quick Open** - The `fileTypes` member of Quick Open plugins (passed to `QuickOpen.addQuickOpenPlugin()`) is deprecated. Please migrate to `languageIds` instead. For some languages, the language id is the same as the file extension; for others, you must update the string.

**Toolbar Layout** - Although extensions that add new toolbar icons generally still work with the new toolbar, several potential incompatibilities may arise:
* Dark colored icons may now be too hard to see
* Icon placement differs (previously, earlier in the DOM order meant further to the right, away from the default Brackets icons; now it means closer to the top, closer to the default icons)
* Extensions that add new horizontal toolbars need to be updated. Rather than inserting content inside or next to `#main-toolbar`, insert content above `#editor-holder`.
* Resizer's `forcepadding` option has been replaced with `forceLeft`

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
* [Code hints for HTML entities](https://github.com/adobe/brackets/pull/3237) ([and](https://github.com/adobe/brackets/pull/3340)) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Polish translation](https://github.com/adobe/brackets/pull/3235) by [Rafal Borowski](https://github.com/rafalborowski) ([plus](https://github.com/adobe/brackets/pull/3360) [fixes](https://github.com/adobe/brackets/pull/3361) by [niu tech](https://github.com/niutech))
* [Czech translation](https://github.com/adobe/brackets/pull/3150) by [kvarel](https://github.com/kvarel)
* [Migrate QuickOpen APIs and CSS inline editor implementation to new LangugeManager APIs](https://github.com/adobe/brackets/pull/3301) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Migrate CodeHintManager APIs to new LanguageManager APIs](https://github.com/adobe/brackets/pull/3270) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix #3253: Active line highlight doesn't disappear in unfocused editor](https://github.com/adobe/brackets/pull/3274) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #3228: Code hints popup positioned wrong with non-default font size](https://github.com/adobe/brackets/pull/3232) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Maintain scroll position when adjusting font size](https://github.com/adobe/brackets/pull/3300) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Cleanup: refactor JSLint into a default extension](https://github.com/adobe/brackets/pull/3143) ([and](https://github.com/adobe/brackets/pull/3315)) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Cleanup: Recent Projects code](https://github.com/adobe/brackets/pull/3213) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Cleanup: only Live Preview server provider should check file extension](https://github.com/adobe/brackets/pull/3218) by [Jonathan Rowny](https://github.com/jrowny)
* [Fix #3271: Expose constants for Resizer APIs](https://github.com/adobe/brackets/pull/3290) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Unit tests for editor options: highlight active line, word wrap, auto-close braces, hide line numbers](https://github.com/adobe/brackets/pull/3231) by [Tomás Malbrán](https://github.com/TomMalbran)
* [German translation update](https://github.com/adobe/brackets/pull/3236) by [Ben Schley](https://github.com/bschley)
* [Spanish translation update](https://github.com/adobe/brackets/pull/3393) by [Chema Balsas](https://github.com/jbalsas)
* [Chinese translation update](https://github.com/adobe/brackets/pull/3287) by [yodfz](https://github.com/yodfz)
* [Fix elipses char in strings](https://github.com/adobe/brackets/pull/3282) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Add Brackets license to package.json](https://github.com/adobe/brackets/pull/3258) by [Lee Leathers](https://github.com/theoreticaLee)


Contributions _from_ the Brackets community
-------------------------------------------
**Contributions to [CodeMirror](https://github.com/marijnh/CodeMirror):**
* [Don't auto-close apostrophes in comments](https://github.com/marijnh/CodeMirror/commit/c1b7ea4)

**Other:**
* Dropzone: [Make Dropzone message clickable](https://github.com/enyo/dropzone/pull/91)

Bugs fixed in Sprint 23
-----------------------
For details on the bugs addressed, please refer to [closed sprint 23 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=10&state=closed). A few of the fixed bugs might not be caught by this search query, however.