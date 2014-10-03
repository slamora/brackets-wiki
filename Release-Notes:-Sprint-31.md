What's New in Sprint 31
-----------------------
* **Live Preview**
    * [Live Preview HTML changes](https://trello.com/c/ya9wexlA/998-2-improve-html-live-development-performance): Live Preview updates in real time as you type in HTML files. Updates pause whenever the HTML is not syntactically valid.
* **Overall UI**
    * [Dark Themed Window Chrome on Windows](https://trello.com/card/5-into-darkness-shell-windows/4f90a6d98f77505d7940ce88/874): A similar change will be [coming soon for Mac](https://trello.com/card/into-darkness-shell-osx/4f90a6d98f77505d7940ce88/900) too.
    * [Keyboard shortcut to switch between recent projects](https://github.com/adobe/brackets/pull/4546): Press Ctrl+Alt+R (&#x2325;⌘R) to open the "recent projects" dropdown, use the up/down arrow keys to navigate the list, Enter key to select project, and Esc key to cancel.
* **SCSS Code Hints**
    * [Code hints for CSS property names & values](https://github.com/adobe/brackets/pull/4931): The same code hints you see in CSS files will now also appear in SCSS files.
    * [Quick Docs for CSS properties](https://github.com/adobe/brackets/pull/5069): Ditto &ndash; just like in CSS.
* **HTML Matching Tag Highlighting**
    * [Highlight matching open/close tag](https://github.com/adobe/brackets/pull/4504)
* **Search**
    * [Search result tickmarks](https://github.com/adobe/brackets/pull/5191): Find results are visually mapped out as yellow tickmarks along the scroll bar track.
    * [Find in Files results auto-update while typing](https://github.com/adobe/brackets/pull/4729)
* **Linting**
    * [Extensibility for linting tools](https://github.com/adobe/brackets/pull/4588)
* **Localization**
    * [Slovak translation added](https://github.com/adobe/brackets/pull/4856)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-30...sprint-31a#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-30...sprint-31a#commits_bucket)


UI Changes
----------
**Dark-themed window chrome on Windows** - the Windows shell now has a dark window chrome that visually complements the Brackets UI. (The Mac shell is expected to receive a similar update next sprint).

**Live HTML UI for invalid state** - during Live HTML Development, Brackets indicates when code
isn't updating due to being in an invalid state. The Live Development icon turns red, and the offending line of code is highlighted with a red background under the line number.


API Changes
-----------
**LESS** - updated to 1.4.2 (from 1.3.3). May require minor changes to LESS stylesheets (e.g. wrapping math expressions in parens).

**Key bindings** - Cross-platform behavior is now better defined when specifying a set of bindings for one command. Bindings with no platform specified are not used if another binding in the set explicitly specifies the current platform. Bindings can specify a new platform `"all"` to ensure they are used on all platforms regardless of other platform-specific bindings in the set. [See brief examples](https://github.com/adobe/brackets/issues/4265#issuecomment-23831957).


New/Improved Extensibility APIs
-------------------------------
* **Linting** - Use `CodeInspection.register()` to provide linting/inspection for a given Language. Just like the built-in JSLint functionality, the provider is invoked whenever a file is opened or saved, and its results are displayed in a panel below the editor (providers may be run more frequently in the future, however). Currently, only one provider is accepted per language, although extensions _can_ replace the default JSLint provider for JavaScript.

* **Code Hints** - The behavior when pressing Tab is now configurable on a global and per-provider basis. Use the optional `insertHintOnTab` flag when registering with `registerHintProvider()` to make Tab insert the currently selected hint (instead of inserting a Tab character, the default behavior). Use `CodeHintManager.setInsertHintOnTab()` to globally change the default behavior. See [#5084](https://github.com/adobe/brackets/pull/5084 "Make code hint insertion on tab key configurable") for more.

* **Web links** - Clicking `<a>` tag in the UI will automatically open its link in the user's default browser (_unless_ it is a bare anchor, i.e. it starts with "#"). The click event will still bubble to any further handlers.

* **Menus** - `removeMenu()` and `getAllMenus()` methods added.

* **ProjectManager** - New `"projectRefresh"` event, triggered when the tree is refreshed without switching projects (useful for extensions that "decorate" the tree with custom UI).

* **Commands** - Use `CommandManager.registerInternal()` to register a command that can never be invoked by the end user. The command has no name/label (only an ID) and will never appear in any UI (e.g. future UI to customize keyboard shortcuts).

Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. See workaround https://support.google.com/chrome/answer/106010?hl=en.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* A last minute fix was taken for Sprint 31, so when opening the About dialog, instead of seeing "(On branch master,master [SHA])", this build will have "(On branch sprint-31-hotfix,sprint-31-hotfix [SHA])".


Community contributions to Brackets
-----------------------------------
* [Highlight matching HTML open/close tags](https://github.com/adobe/brackets/pull/4504) by [Sathyamoorthi](https://github.com/sathyamoorthi)
* [Update 'Find in Files' results as you type](https://github.com/adobe/brackets/pull/4729) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Add Next/Prev buttons to Find bar](https://github.com/adobe/brackets/pull/5002) (previously accessible only via menu/shortcut) by [Rajesh Segu](https://github.com/rajeshsegu)
* [Keyboard shortcut to switch between recent projects](https://github.com/adobe/brackets/pull/4546) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Linux: Use Chromium for live development if Chrome not installed](https://github.com/adobe/brackets-shell/pull/317) by [Maciej Żok](https://github.com/macie)
* [Highlight ASP files as plain HTML & highlight VBS files](https://github.com/adobe/brackets/pull/5010) by [Dmitry Ananichev](https://github.com/Fr3nzzy)
* [Add Slovak translation](https://github.com/adobe/brackets/pull/4856) by [Erich Stark](https://github.com/erichstark)
* [Update to LESS 1.4.2](https://github.com/adobe/brackets/pull/4476) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Add Menus.removeMenu(), getAllMenus() APIs](https://github.com/adobe/brackets/pull/5217) by [Lance Campbell](https://github.com/lkcampbell)
* [Add ProjectManager "projectRefresh" event](https://github.com/adobe/brackets/pull/4815) by [Martin Zagora](https://github.com/zaggino)
* [API to register internal-only Commands that will never be exposed to user](https://github.com/adobe/brackets/pull/4306) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Re-fix #4602/#4870: Long file names truncated in search results header](https://github.com/adobe/brackets/pull/4940) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #5062: Update notification is no longer modal](https://github.com/adobe/brackets/pull/5085) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #1910: Context menu pops closed when right-clicking file that hasn't been opened yet](https://github.com/adobe/brackets/pull/5004) by [Lance Campbell](https://github.com/lkcampbell)
* [Fix #4693: Unpredictable context menu behavior while naming new file](https://github.com/adobe/brackets/pull/5114) by [Lance Campbell](https://github.com/lkcampbell)
* [Fix #3720: Complete file Rename when clicking anywhere in project panel](https://github.com/adobe/brackets/pull/4934) by [Lance Campbell](https://github.com/lkcampbell)
* [Better error reporting for problems loading internal Node modules](https://github.com/adobe/brackets/pull/5223) by [Martin Zagora](https://github.com/zaggino)
* [Better error handling for Mac build problems](https://github.com/adobe/brackets-shell/pull/325) by [Jarrod Overson](https://github.com/jsoverson)
* [Consolidate link-handling code](https://github.com/adobe/brackets/pull/4718) ([and](https://github.com/adobe/brackets/pull/5115)) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Unit test API & code cleanups](https://github.com/adobe/brackets/pull/4629) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix mis-named string key](https://github.com/adobe/brackets/pull/5133) ([and](https://github.com/adobe/brackets/pull/5130)) by [Marcel Gerber](https://github.com/SAPlayer)
* [German translation updates](https://github.com/adobe/brackets/pull/5212) (part [2](https://github.com/adobe/brackets/pull/5129), [3](https://github.com/adobe/brackets/pull/5123), [4](https://github.com/adobe/brackets/pull/5025), [5](https://github.com/adobe/brackets/pull/5012)) by [Marcel Gerber](https://github.com/SAPlayer)
* [German translation update](https://github.com/adobe/brackets/pull/5145) by [mynetx](https://github.com/mynetx)
* [Spanish translation update](https://github.com/adobe/brackets/pull/5221) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Chinese translation update](https://github.com/adobe/brackets/pull/5161) by [Michael J.](https://github.com/michaeljayt)
* [Russian translation update](https://github.com/adobe/brackets/pull/4999) by [Denis Morozov](https://github.com/morozd)
* [Swedish translation update](https://github.com/adobe/brackets/pull/4964) by [Mikael Jorhult](https://github.com/mikaeljorhult)
* [Finnish translation update](https://github.com/adobe/brackets/pull/4741) by [ghost](https://github.com/ghost)
* [Improve docs for Command handlers](https://github.com/adobe/brackets/pull/5155) by [Lance Campbell](https://github.com/lkcampbell)
* [Fix docs SidebarView show/hide events](https://github.com/adobe/brackets/pull/4804) by [Lance Campbell](https://github.com/lkcampbell)
* [Localization records update](https://github.com/adobe/brackets/pull/5242) by [Attila Oláh](https://github.com/NoNameProvided)
* [.gitignore update: ignore WebStorm project files](https://github.com/adobe/brackets/pull/5018) by [Dmitry Ananichev](https://github.com/Fr3nzzy)


#### Pulling source code from Git
* _Warning:_ it is not yet possible to build brackets-shell with XCode 5. Hold off on upgrading XCode if you need to build brackets-shell (should be possible soon though).
* A new brackets-shell build is _optional_ for this sprint. Be sure to rerun `grunt setup` before building.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Sprint 31
-----------------------
For details on the bugs addressed, please refer to [closed sprint 31 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=18&state=closed). A few of the fixed bugs might not be caught by this search query, however.