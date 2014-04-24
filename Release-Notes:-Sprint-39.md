_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 39 -- approximately May 5._

What's New in Sprint 39
-----------------------
* **Code Editing**
    * [Switch language/syntax mode of a single file](https://github.com/adobe/brackets/pull/6409): Use the language indicator in the status bar as a dropdown to override the language Brackets chose to treat a file as. (These changes are _not_ yet saved when you close the file, or generalized to all files with a certain extension).
    * [Cut/Copy whole line when nothing is selected](https://github.com/marijnh/CodeMirror/issues/2382)
* **Extensions**
    * [Extension update notifications](https://github.com/adobe/brackets/pull/7330): The Extension Manager toolbar icon turns green when one or more of your installed extensions have a new version available.
    * [Admin features for Extension Registry](https://trello.com/c/NAtggRqE/1224-simple-admin-for-registry): (available since 4/17) An extension's author can delete the extension from the registry, mark the extension as incompatible with newer versions of Brackets, or transfer ownership to a different author.
* **OS Support**
    * **Windows XP** is no longer officially supported by Brackets. It is still possible to download and install Brackets on Windows XP, but we will no longer test it - so future versions may become incompatible due to native code changes.
* **Ongoing Research** (not implemented yet)
    * [Research: Split-view architecture](https://trello.com/c/8YAFyAZD/500-split-view-multiple-documents): See [architecture proposal](https://github.com/adobe/brackets/wiki/SplitView-Architecture-Notes).
    * [Research: Theming support](https://groups.google.com/forum/#!topic/brackets-dev/Rj-LhMSseKE)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-38...sprint-39#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-38...sprint-39#commits_bucket)


UI Changes
----------
**Find menu added** - All Find/Replace-related menu items have been moved from the Edit menu to a new Find top-level menu.

API Changes
-----------
**Find commands** - Find/Replace-related command ID constants (`Commands.EDIT_FIND*` and a few others) have been deprecated: use `Commands.CMD_FIND*` (and similar) instead. _The raw ID string values have changed_, so if you're using them instead of referencing the constants, your code will be broken immediately.

The menu item group constants `Menus.MenuSection.EDIT_FIND_COMMANDS`/`EDIT_REPLACE_COMMANDS` are deprecated: use `Menus.MenuSection.FIND_FIND_COMMANDS`/`FIND_REPLACE_COMMANDS` instead. Using the old constants will add your menu items to the end of the Edit menu.

New/Improved Extensibility APIs
-------------------------------
**Find menu** - Use `Menus.AppMenuBar.FIND_MENU` to add menu items to the new Find top-level menu.

Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. [See workaround](https://support.google.com/chrome/answer/106010?hl=en).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* [#7127](https://github.com/adobe/brackets/issues/7127): With Chrome 34, using Live Preview will show a console error: "'CSS.getAllStyleSheets' wasn't found". This is harmless and shouldn't affect Live Preview.

Community contributions to Brackets
-----------------------------------
TODO

#### Pulling source code from Git
TODO...
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.

Bugs fixed in Sprint 39
-----------------------
For details on the bugs addressed, please refer to [closed sprint 39 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=27&state=closed). Not all fixed bugs will be caught by this search query, however.