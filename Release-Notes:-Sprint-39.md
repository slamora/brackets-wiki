What's New in Sprint 39
-----------------------
* **File Types**
    * [Customize file extension -> language/syntax mode mapping](https://github.com/adobe/brackets/pull/7588): Set `language.fileExtensions` or `language.fileNames` to a map object whose keys are file extensions/names and whose values are [language IDs](https://github.com/adobe/brackets/blob/master/src/language/languages.json). See [how to edit your preferences file](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#preferences).
    * [Windows: Filter out binary files and unsupported encodings](https://trello.com/c/Sji5hLvW/1219-1s-automatically-ignore-exclude-binary-files): Speeds up Find in Files for certain projects, and reduces clutter in the Quick Open file list. Attempting to open a non-UTF-8 file now results in an error instead of showing garbled text. (This was already implemented on other platforms).
* **CSS Editing**
    * [Fuzzy/camelCase code hints](https://github.com/adobe/brackets/pull/7441): Code hints now use smart string matching, similar to Quick Open and JS code hints -- for example, you can type "btr" to see an autocompletion hint for `border-top-right-radius`.
* **Code Editing**
    * [Cut/Copy whole line when nothing is selected](https://github.com/marijnh/CodeMirror/issues/2382)
* **Extensions**
    * [Extension update notifications](https://github.com/adobe/brackets/pull/7330): The Extension Manager toolbar icon turns green when one or more of your installed extensions have a new version available.
    * [Admin features for Extension Registry](https://trello.com/c/NAtggRqE/1224-simple-admin-for-registry): (available since 4/17) An extension's author can delete the extension from the registry, mark the extension as incompatible with newer versions of Brackets, or transfer ownership to a different author.
* **Menus**
    * [New Find menu added](https://github.com/adobe/brackets/pull/7488), simplifying Edit menu (see below)
    * [Linux: Long menus are scrollable](https://github.com/adobe/brackets/pull/7731) instead of being cut off
* **Stability Improvements**
    * Fixed freezes/crashes some users encountered in projects using the Ionic framework
    * [Optional status bar indicator for internal errors](https://github.com/adobe/brackets/pull/7639): Enable via _Debug > Show Errors in Status Bar_.
* **Localization**
    * [Croatian translation added](https://github.com/adobe/brackets/pull/7567)
* **OS Support**
    * **Windows XP** is no longer officially supported by Brackets. It is still possible to download and install Brackets on Windows XP, but we will no longer test it - so future versions may become completely incompatible without warning.
* **Ongoing Research** (not implemented yet)
    * [Research: Split-view architecture](https://trello.com/c/8YAFyAZD/500-split-view-multiple-documents): See [architecture proposal](https://github.com/adobe/brackets/wiki/SplitView-Architecture-Notes).
    * [Research: Theming support](https://groups.google.com/forum/#!topic/brackets-dev/Rj-LhMSseKE)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-38...sprint-39#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-38...sprint-39#commits_bucket)


UI Changes
----------
**Find menu added** - All Find/Replace-related menu items have been moved from the Edit menu to a new Find top-level menu.

**Non UTF-8 encodings** - On Windows, earlier Brackets versions would open files with other encodings even though the file could not be displayed or saved correctly. Brackets now shows an error message and refuses to open the file, matching Mac versions of Brackets. (And the error message now specifically mentions the encoding).

**URL code hints** - When replacing path segments in an existing URL, the segments to the right of the cursor are preserved when you insert path code hints. When you insert a filename code hint, anything to the right is still overwritten as before, however.

API Changes
-----------
**Find commands** - Find/Replace-related command ID constants (`Commands.EDIT_FIND*` and a few others) have been deprecated: use `Commands.CMD_FIND*` (and similar) instead. _The raw ID string values have changed_, so if you're using them instead of referencing the constants, your code will be broken immediately.

The menu item group constants `Menus.MenuSection.EDIT_FIND_COMMANDS`/`EDIT_REPLACE_COMMANDS` are deprecated: use `Menus.MenuSection.FIND_FIND_COMMANDS`/`FIND_REPLACE_COMMANDS` instead. Using the old constants will add your menu items to the end of the Edit menu.

**XML/HTML tokens** - The tokens emitted by CodeMirror for XML/HTML code have changed: the angle brackets around each tag are now split into separate tokens with the style `"tag bracket"`. Extensions depending on the low-level CodeMirror token data may need an update ([example](https://github.com/adobe/brackets/pull/7545/files)).

New/Improved Extensibility APIs
-------------------------------
**Find menu** - Use `Menus.AppMenuBar.FIND_MENU` to add menu items to the new Find top-level menu.

**LESS stylesheets** - `ExtensionUtils.loadStyleSheet()` now supports LESS files that use `@import`.

**DocumentManager** - The `"currentDocumentChange"` event now passes the old and new Documents as arguments (similar to `"activeEditorChange"`).


Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.

Community contributions to Brackets
-----------------------------------
* [Extension update notifications](https://github.com/adobe/brackets/pull/7330) by [Martin Zagora](https://github.com/zaggino)
* [Move Find items to new top-level Find menu](https://github.com/adobe/brackets/pull/7488) by [Lance Campbell](https://github.com/lkcampbell)
* [Build script to generate Linux .tar.gz redistributable](https://github.com/adobe/brackets-shell/pull/433) by [Arron Washington](https://github.com/radicaled)
* [Support extension LESS files that use @import](https://github.com/adobe/brackets/pull/7522) ([and](https://github.com/adobe/brackets/pull/7612)) by [Martin Zagora](https://github.com/zaggino)
* [Optional status bar indicator for internal errors](https://github.com/adobe/brackets/pull/7639) by [Martin Zagora](https://github.com/zaggino)
* [Show full stack trace in console for uncaught Node-side errors](https://github.com/adobe/brackets-shell/pull/432) by [Martin Zagora](https://github.com/zaggino)
* [Fix #7283: Incorrect padding when line numbers turned off](https://github.com/adobe/brackets/pull/7641) by [Lance Campbell](https://github.com/lkcampbell)
* [Add Croatian translation](https://github.com/adobe/brackets/pull/7567) ([and](https://github.com/adobe/brackets/pull/7710)) by [Kruno H](https://github.com/diomed)
* [Update CSS code hints for latest CSS Shapes](https://github.com/adobe/brackets/pull/7761) ([and](https://github.com/adobe/brackets/pull/7763)) by [Bem Jones-Bey](https://github.com/bemjb)
* [Clear search filter when switching tabs in Extension Manager](https://github.com/adobe/brackets/pull/7388) by [Andrey Evstropov](https://github.com/EAndreyF)
* [Fix About dialog to show more than 30 contributors again](https://github.com/adobe/brackets/pull/7618) by [Marcel Gerber](https://github.com/SAPlayer) (shows top 100 for new, pending larger fix next sprint)
* [Fix AnimationUtils.animateUsingClass() to skip hidden elements](https://github.com/adobe/brackets/pull/7713) by [Martin Zagora](https://github.com/zaggino)
* [Fix #6808: CSS timing editor labels positioned wrong with non-default font size](https://github.com/adobe/brackets/pull/7742) by [Marcel Gerber](https://github.com/SAPlayer)
* [Fix #7396: Awkward slide-out animation when Replace bar has line-wrapped](https://github.com/adobe/brackets/pull/7743) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Add current and previous documents to "currentDocumentChange" event](https://github.com/adobe/brackets/pull/7509) by [Martin Zagora](https://github.com/zaggino)
* [Fix new update notification URL for languages without translations](https://github.com/adobe/brackets/pull/7811) by [Marcel Gerber](https://github.com/SAPlayer)
* [Fix bug with concurrent PromiseQueue instances](https://github.com/adobe/brackets/pull/7485) by [jayther](https://github.com/jayther)
* [Tweak update notification layout](https://github.com/adobe/brackets/pull/7636) by [Marcel Gerber](https://github.com/SAPlayer)
* [Cleanup: Revert unneeded change to Resizer behavior](https://github.com/adobe/brackets/pull/7526) by [Martin Zagora](https://github.com/zaggino)
* [Sort locales by display name](https://github.com/adobe/brackets/pull/7593) ([and](https://github.com/adobe/brackets/pull/7617)) by [Marcel Gerber](https://github.com/SAPlayer)
* [Turkish translation update](https://github.com/adobe/brackets/pull/7690) by [Mahmut Bulut](https://github.com/vertexclique)
* [German translation update](https://github.com/adobe/brackets/pull/7715) by [Marc Bodmer](https://github.com/m-bodmer)
* [German translation update](https://github.com/adobe/brackets/pull/7813) by [Marcel Gerber](https://github.com/SAPlayer)
* [Brazilian Portuguese translation update](https://github.com/adobe/brackets/pull/7470) by [Henrique Aparecido Lavezzo](https://github.com/Rynaro)
* [Spanish translation update](https://github.com/adobe/brackets/pull/7780) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Italian translation update](https://github.com/adobe/brackets/pull/7792) by [Christopher Pecoraro](https://github.com/chrispecoraro)
* [Rename en-uk to en-gb](https://github.com/adobe/brackets/pull/7599) by [Martin Zagora](https://github.com/zaggino)


#### Pulling source code from Git
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.
* A new brackets-shell build is _only_ required for the new Windows binary-file detection (and minor bug fixes).

Bugs fixed in Sprint 39
-----------------------
For details on the bugs addressed, please refer to [closed sprint 39 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=27&state=closed). Not all fixed bugs will be caught by this search query, however.