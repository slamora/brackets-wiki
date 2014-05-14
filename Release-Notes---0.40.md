_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 0.40 -- approximately ???._

What's New in Release 0.40
--------------------------
* **Search**
    * **[Replace across multiple files](https://trello.com/c/NbNEOs4S/264-replace-across-multiple-files)**: You can see all search matches first and uncheck any you don't wish to replace. Supports the same exclusion filtering as Find in Files.
    * [Save multiple file exclusion filters](https://trello.com/c/4EQI1XwC/1137-2s-save-edit-multiple-different-file-exclusion-sets): Instead of a single file/folder search filter, you can name and save multiple different filters for quick use.
    * [Quick Open: many small bug fixes](https://github.com/adobe/brackets/pull/7227)
* **File Types**
    * [Switch language/syntax mode of a single file](https://github.com/adobe/brackets/pull/6409): Use the language indicator in the status bar as a dropdown to override the language Brackets chose to treat a file as. (These changes are _not_ saved when you close the file; use the preferences above to permanently treat all files with a certain extension as a different language).
* **Ongoing Research** (not implemented yet)
    * [Split view](https://trello.com/c/2DWV5tEX/1277-splitview-migrate-workingset-management-to-mainviewmanager) (early implementation on branch)
    * [Research: Theming Support](https://trello.com/c/LHhAcbcU/1260-c-editor-themes) (pull request in progress)

common categories: Live Preview, Code Editing, Visual Editing, Search, Files and Folders, Overall UI, Localization, Extensions, Developer Workflow
less common: Code Hinting, General Code Editing, HTML Code Editing, Performance

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-39...sprint-40#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-39...sprint-40#commits_bucket)


UI Changes
----------
**Quick Open** - The first item is automatically highlighted, making it more clear that you can press Enter immediately to jump to the top item (Down arrow not required). Highlighting the 2nd item in the result list now only requires pressing the Down arrow once (previously required two presses). Quick Find Definition no longer changes the scroll position until you start typing.


API Changes
-----------
**LESS** - Updated to 1.7.0 (from 1.4.2). No known compatibility issues - see [changelog](https://github.com/less/less.js/blob/master/CHANGELOG.md).

New/Improved Extensibility APIs
-------------------------------


Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
TODO

#### Pulling source code from Git
* TODO: new brackets-shell build required?
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 0.40
--------------------------
For details on the bugs addressed, please refer to [closed sprint 40 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=28&state=closed). Not all fixed bugs will be caught by this search query, however.