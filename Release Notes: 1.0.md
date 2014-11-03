What's New in Release 1.0
--------------------------
* **Preferences**
    * **[Configure keyboard shortcuts](https://trello.com/c/3mZwu1DE/352-user-specified-keyboard-shortcuts-for-commands-in-json)**: Customize your Brackets keyboard shortcuts via a JSON config file. [See documentation for details](https://github.com/adobe/brackets/wiki/User-Key-Bindings).
* **Quick Edit**
    * [Collapse unwanted Quick Edit results](https://trello.com/c/nxQ6eGGU/1031-3-css-quick-edit-grouping): Quickly collapse results from a particular CSS, SCSS, or LESS file. For example, when using a preprocessor you can hide results from the compiled CSS file to focus on results in your original SCSS/LESS code.
* **Code Hints & Search**
    * [url() code hints for SCSS/LESS](https://github.com/adobe/brackets/pull/9097): Code hints for files/folders within your project, as seen in CSS files.
    * [Improved case-sensitivity in code hints and Quick Open](https://github.com/adobe/brackets/issues/3263): Code Hints and Quick Open prefer results where the capitalization matches what you typed. For example, in JavaScript code hints "function" is now ranked above "Function" when you type in all lowercase.
    * [New "wordsOnly" setting for automatic word highlighting](https://github.com/codemirror/CodeMirror/pull/2791): See [preferences documentation](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#preferences) for how to use this setting.
* **Performance**
    * [Improved performance when switching files](https://github.com/adobe/brackets/pull/9507)
* **UI Appearance**
    * [Correct window sizing with auto-hide TaskBar on Windows](https://github.com/adobe/brackets-shell/pull/474)
    * [On OS X Yosemite (10.10), window 'traffic lights' use a flatter style](https://github.com/adobe/brackets-shell/pull/478)
    * [Dark theme refinements](https://github.com/adobe/brackets/pull/9560)
* **Linux**
    * ["Show in OS" reveals files in shell file browser](https://github.com/adobe/brackets-shell/pull/477) (previously, in some cases the file would open in its default instead)
* **Localization**
    * Translation updates for: Brazilian Portuguese, Croatian, Czech, Danish, Finnish, French, Galician, German, Italian, Japanese, Spanish, Swedish

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-0.44...release-1.0#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-0.44...release-1.0#commits_bucket)


UI/Behavior Changes
-------------------
**Max file size** - For performance and stability, files larger than 16 MB will not be opened by Brackets.

API Changes
-----------
**Version numbering** - While previous releases of Brackets used version numbers like "0.XX", this release is numbered "1.0" and the next release (due out within a month) will be "1.1".

**Deprecated APIs removed** - The following APIs, which had been deprecated for some time already, have been removed:

* `CollectionUtils` module
* `StringUtils.htmlEscape()`
* `FileUtils.canonicalizeFolderPath()`
* `ProjectManager.isBinaryFile()`
* `CSSAgent.getStylesheetURLs()` & `CSSDocument.getStyleSheetFromBrowser()` 
* Command ids `EDIT_FIND*`, `EDIT_ADD_NEXT_MATCH`, `EDIT_SKIP_CURRENT_MATCH`, `EDIT_REPLACE`
* Menu section ids `EDIT_FIND_COMMANDS`, `EDIT_REPLACE_COMMANDS`

Bugs have already been filed with all extensions that are believed to be using these APIs.

New/Improved Extensibility APIs
-------------------------------
**Themes** - Themes no longer need to include a dummy 'main.js' file. Theme authors may wish to continue including this file in the short term for compatibility with Brackets 0.44, however.

**Prepackaged extensions** - Distributions of Brackets can now include extensions that will be auto-installed in the user extensions folder on first launch. Place extension .zip packages in a folder named `auto-install-extensions` next to the `www` folder. Users can update & uninstall these extensions individually just like regular user-installed extensions.


Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Enable url() code hints for SCSS/LESS](https://github.com/adobe/brackets/pull/9097) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Remove main.js requirement for themes](https://github.com/adobe/brackets/pull/9434) by [Triangle717](https://github.com/le717)
* [Linux: Fix "Show in OS" for files](https://github.com/adobe/brackets-shell/pull/477) by [Rado Rodopski](https://github.com/radorodopski)
* [Fix gap that sometimes appears below sidebar](https://github.com/adobe/brackets/pull/9376) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Recognize Vagrantfile as Ruby](https://github.com/adobe/brackets/pull/9536) by [al8anp](https://github.com/al8anp)
* [Fix: Make Quick Docs content selectable again](https://github.com/adobe/brackets/pull/9595) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Keep query/replace values when switching between Replace, Replace in Files](https://github.com/adobe/brackets/pull/9601) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix #9363: Code hints keep closing when Find in Files has collapsed results](https://github.com/adobe/brackets/pull/9548) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix bug with Live Preview Highlight positioning](https://github.com/adobe/brackets/pull/8922) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Don't pollute prefs with "Show Errors in Status Bar" when unchanged](https://github.com/adobe/brackets/pull/9030) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Shift-click status bar error indicator to clear it](https://github.com/adobe/brackets/pull/8997) by [cheesypoof](https://github.com/cheesypoof)
* [Change 'Reload Without Extensions' to Cmd-Ctrl-R on Mac](https://github.com/adobe/brackets/pull/9663) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix unit tests for non-English locales](https://github.com/adobe/brackets/pull/8995) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix failing QuickOpen tests on Linux](https://github.com/adobe/brackets/pull/9453) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix: Search says "Over 100000 matches" when there are exactly 100000 matches)](https://github.com/adobe/brackets/pull/9385) by [jacksonweekes](https://github.com/jacksonweekes)
* [Finish updating Brackets version to 0.45](https://github.com/adobe/brackets/pull/9477) (note: version number later changed to 1.0) by [Triangle717](https://github.com/le717)
* [Cleanup: Streamline dependencies.js](https://github.com/adobe/brackets/pull/9488) by [TuurDutoit](https://github.com/TuurDutoit)
* [Cleanup: Remove some deprecated APIs (see list above)](https://github.com/adobe/brackets/pull/8960) (+ [part 2](https://github.com/adobe/brackets/pull/8992), [part 3](https://github.com/adobe/brackets/pull/9640), [part 4](https://github.com/adobe/brackets/pull/9674)) by [Triangle717](https://github.com/le717)
* [Brazilian Portuguese translation update](https://github.com/adobe/brackets/pull/9726) by [eliezerb](https://github.com/eliezerb)
* [Croatian translation update](https://github.com/adobe/brackets/pull/9564) by [Kruno H](https://github.com/diomed)
* [Czech translation update](https://github.com/adobe/brackets/pull/9497) by [kvarel](https://github.com/kvarel)
* [Danish translation image update](https://github.com/adobe/brackets/pull/9617) by [cyberomin](https://github.com/cyberomin)
* [Finnish translation update](https://github.com/adobe/brackets/pull/9655) (+ [part 2](https://github.com/adobe/brackets/pull/9683)) by [valtlait](https://github.com/valtlait)
* [French & Japanese translation updates)](https://github.com/adobe/brackets/pull/9733) by [pantkowiak](https://github.com/pantkowiak)
* [Galician translation update](https://github.com/adobe/brackets/pull/9498) by [Oscar Otero](https://github.com/oscarotero)
* [German translation update](https://github.com/adobe/brackets/pull/9728) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Italian translation update](https://github.com/adobe/brackets/pull/9527) by [Denisov21](https://github.com/Denisov21)
* [Spanish translation update](https://github.com/adobe/brackets/pull/9470) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Swedish translation update](https://github.com/adobe/brackets/pull/9685) (+ [part 2](https://github.com/adobe/brackets/pull/9738)) by [Mikael Jorhult](https://github.com/mikaeljorhult)
* [Use real dash instead of HTML entity in Getting Started](https://github.com/adobe/brackets/pull/9533) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix outdated docs links to CodeMirror GitHub](https://github.com/adobe/brackets/pull/9407) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix README typo](https://github.com/adobe/brackets/pull/9449) by [Denisov21](https://github.com/Denisov21)
* [Correct API docs typo](https://github.com/adobe/brackets/pull/9521) by [rsperberg](https://github.com/rsperberg)


#### Pulling source code from Git
* Recommended: rebuild or reinstall an updated brackets-shell (new functionality was added).
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 1.0
--------------------------
For details on the bugs addressed, please refer to [closed Release 1.0 bugs](https://github.com/adobe/brackets/issues?q=milestone%3A%22Brackets+1.0+%28Release+0.45%29%22+is%3Aclosed) . Not all fixed bugs will be caught by this search query, however.