_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 31 -- approximately September 18._

What's New in Sprint 31
-----------------------
* **Live Preview**
    * [Live Preview HTML changes](https://trello.com/c/ya9wexlA/998-2-improve-html-live-development-performance): Live Preview updates in real time as you type in HTML files. Updates pause whenever the HTML is not syntactically valid.
* **Overall UI**
    * [Dark titlebar on Windows](https://trello.com/card/5-into-darkness-shell-windows/4f90a6d98f77505d7940ce88/874): A similar change will be [coming soon for Mac](https://trello.com/card/into-darkness-shell-osx/4f90a6d98f77505d7940ce88/900) too.
    * [Keyboard shortcut to switch between recent projects](https://github.com/adobe/brackets/pull/4546): Press Ctrl+Alt+R (&#x2325;⌘R) to open the "recent projects" dropdown, use the up/down arrow keys to navigate the list, Enter key to select project, and Esc key to cancel.
* **CSS Code Hints in SCSS**
    * [Code hints for CSS property names & values](https://github.com/adobe/brackets/pull/4931): The same code hints you see in CSS files will now also appear in SCSS files.
    * [Quick Docs for CSS properties](https://github.com/adobe/brackets/pull/5069): Ditto &ndash; just like in CSS.
* **HTML Matching Tag Highlighting**
    * [Highlight matching open/close tag](https://github.com/adobe/brackets/pull/4504)
* **Search**
    * [Search result tickmarks](https://github.com/adobe/brackets/pull/5191): Find results are visually mapped out as yellow tickmarks along the scroll bar track.
    * [Find in Files results auto-update while typing](https://github.com/adobe/brackets/pull/4729)
* **Linting**
    * [Extensibility for linting tools](https://github.com/adobe/brackets/pull/4588)
* **Arch Linux Support**
    * [Brackets is now available for Arch Linux on the Arch User Repository (AUR).](https://github.com/adobe/brackets-shell/pull/316)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-30...sprint-31#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-30...sprint-31#commits_bucket)


UI Changes
----------
**Dark Titlebar on Windows** - the Windows shell now has a dark titlebar that visually complements the Brackets UI.

**Live HTML UI for Invalid State** - during Live HTML Development, Brackets indicates when code
isn't updating due to being in an invalid state by changing the background color of the line number
where the error occurs to red. The color of the Live Development icon is also changed to red
and the tooltip indicates "not updating due to syntax error".


API Changes
-----------
`KeyBindingManager` now supports `platform: "all"` on key bindings for clearer semantics.

TODO: `CodeHintManager` hint provider callback `getHints()` return object added `query` property used for preventing a race condition as described in [issue #5003](https://github.com/adobe/brackets/issues/5003).


New/Improved Extensibility APIs
-------------------------------
* **Linting** - Use `CodeInspection.register()` to provide linting/inspection for a given Language. Just like the built-in JSLint functionality, the provider is invoked whenever a file is opened or saved, and its results are displayed in a panel below the editor (providers may be run more frequently in the future, however). Currently, only one provider is accepted per language, although extensions _can_ replace the default JSLint provider for JavaScript.

* **Code Hints** - The behavior of the CodeHintList on tab key events is now configurable on a global and per-CodeHintProvider basis. The CodeHintManager provider registration API has been generalized with an additional optional property, `insertHintOnTab`, that indicates whether the CodeHintManager should request that the provider of the current session insert the currently selected hint on tab key events, or if instead a tab character should be inserted into the editor. See [#5084](https://github.com/adobe/brackets/pull/5084 "Make code hint insertion on tab key configurable") for more details.

* **Menus** - `removeMenu()` and `getAllMenus()` methods have been implemented.

Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. See workaround https://support.google.com/chrome/answer/106010?hl=en.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Failing with useful message on mac build step](https://github.com/adobe/brackets-shell/pull/325) by [jsoverson](https://github.com/jsoverson).
* [Support for chromium browser in linux](https://github.com/adobe/brackets-shell/pull/317) by [macie](https://github.com/macie).
* [Update swedish translation](https://github.com/adobe/brackets/pull/4964) by [mikaeljorhult](https://github.com/mikaeljorhult).
* [Update Russian index.html](https://github.com/adobe/brackets/pull/4999) by [morozd](https://github.com/morozd).
* [[UX] Issue #4174 - Add find next / previous buttons for easy navigation](https://github.com/adobe/brackets/pull/5002) by [rajeshsegu](https://github.com/rajeshsegu).
* [Working set context menu pops closed when right-clicking file that hasn't been opened yet](https://github.com/adobe/brackets/pull/5004) by [lkcampbell](https://github.com/lkcampbell).
* [Added .ASP files highlighting](https://github.com/adobe/brackets/pull/5010) by [Fr3nzzy](https://github.com/Fr3nzzy).
* [Update strings.js](https://github.com/adobe/brackets/pull/5012), [and again](https://github.com/adobe/brackets/pull/5025) by [SAPlayer](https://github.com/SAPlayer).
* [.idea added to gitignore](https://github.com/adobe/brackets/pull/5018) by [Fr3nzzy](https://github.com/Fr3nzzy).
* [Tree context menu behaves unpredictably while naming new file](https://github.com/adobe/brackets/pull/5114) by [lkcampbell](https://github.com/lkcampbell).
* [Unit Test Failing: ExtensionManager](https://github.com/adobe/brackets/pull/5115) by [TomMalbran](https://github.com/TomMalbran).
* [Renewed German translation](https://github.com/adobe/brackets/pull/5123) by [SAPlayer](https://github.com/SAPlayer).
* [Update German translation](https://github.com/adobe/brackets/pull/5129) by [SAPlayer](https://github.com/SAPlayer).
* [Change: UNKOWN -> UNKNOWN](https://github.com/adobe/brackets/pull/5133) by [SAPlayer](https://github.com/SAPlayer).
* [Update Japanese translation](https://github.com/adobe/brackets/pull/5140) by [kanreisa](https://github.com/kanreisa).
* [Update 'de' locale for HTML preview, more minor changes](https://github.com/adobe/brackets/pull/5145) by [mynetx](https://github.com/mynetx).
* [Document file and handler function for each command](https://github.com/adobe/brackets/pull/5155) by [lkcampbell](https://github.com/lkcampbell).
* [Update strings.js, merge new translates strings for latest version](https://github.com/adobe/brackets/pull/5161) by [michaeljayt](https://github.com/michaeljayt).
* [HTML tag highlight added](https://github.com/adobe/brackets/pull/4504) by [sathyamoorthi](https://github.com/sathyamoorthi).
* [Followup updates to #4581 and #4598](https://github.com/adobe/brackets/pull/4629) by [TomMalbran](https://github.com/TomMalbran).
* [Added projectRefresh event when project tree is refreshed but project root has not changed](https://github.com/adobe/brackets/pull/4815) by [zaggino](https://github.com/zaggino).
* [Tab and modality fixes for the modal dialogs](https://github.com/adobe/brackets/pull/4714) by [TomMalbran](https://github.com/TomMalbran).
* [Update to Finnish localization](https://github.com/adobe/brackets/pull/4741) by [ghost](https://github.com/ghost).
* [Do not show Active Line Highlight when editor has selection](https://github.com/adobe/brackets/pull/4878) by [lkcampbell](https://github.com/lkcampbell).
* [File Rename should complete when clicking anywhere in project panel](https://github.com/adobe/brackets/pull/4934) by [vladnicula](https://github.com/vladnicula), [lkcampbell](https://github.com/lkcampbell).
* [Implementation of Navigate Recent Projects](https://github.com/adobe/brackets/pull/4546) by [TomMalbran](https://github.com/TomMalbran).
* [Updated the flex property for the latest chrome version to fix #4602 and #4870](https://github.com/adobe/brackets/pull/4940) by [TomMalbran](https://github.com/TomMalbran).
* [[ARCH] Image preview draft](https://github.com/adobe/brackets/pull/4492) by [warabe](https://github.com/warabe).
* [Fix issue #3556: Consolidate link handling code](https://github.com/adobe/brackets/pull/4718) by [TomMalbran](https://github.com/TomMalbran).
* [Updated documentation on sidebar view events](https://github.com/adobe/brackets/pull/4804) by [lkcampbell](https://github.com/lkcampbell).
* [Remove strings for commands that are not user facing](https://github.com/adobe/brackets/pull/4306) by [WebsiteDeveloper](https://github.com/WebsiteDeveloper).
* [Slovak language support](https://github.com/adobe/brackets/pull/4856) by [erichstark](https://github.com/erichstark).
* [Clean up _getFileExtension() method and fix all its callers](https://github.com/adobe/brackets/pull/4846) by [megatolya](https://github.com/megatolya).
* [Update Less to 1.4.2](https://github.com/adobe/brackets/pull/4476) by [WebsiteDeveloper](https://github.com/WebsiteDeveloper).
* [Find in Files Improvements (Part 3: Auto-update)](https://github.com/adobe/brackets/pull/4729) by [TomMalbran](https://github.com/TomMalbran).
* [Spanish strings and sample translations](https://github.com/adobe/brackets/pull/5221) by [TomMalbran](https://github.com/TomMalbran).
* [Adding Menus.removeMenu() and Menus.getAllMenus()](https://github.com/adobe/brackets/pull/5217) by [lkcampbell](https://github.com/lkcampbell).
* [Show more info about failure in console](https://github.com/adobe/brackets/pull/5223) by [zaggino](https://github.com/zaggino).
* [Support for chromium browser in linux](https://github.com/adobe/brackets-shell/pull/317) by [macie](https://github.com/macie).

Bugs fixed in Sprint 31
-----------------------
For details on the bugs addressed, please refer to [closed sprint 31 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=18&state=closed). A few of the fixed bugs might not be caught by this search query, however.