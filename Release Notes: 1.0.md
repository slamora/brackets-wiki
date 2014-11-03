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
TODO

#### Pulling source code from Git
* Recommended: rebuild or reinstall an updated brackets-shell (new functionality was added).
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 1.0
--------------------------
For details on the bugs addressed, please refer to [closed Release 1.0 bugs](https://github.com/adobe/brackets/issues?q=milestone%3A%22Brackets+1.0+%28Release+0.45%29%22+is%3Aclosed) . Not all fixed bugs will be caught by this search query, however.