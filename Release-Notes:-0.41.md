What's New in Release 0.41
--------------------------
* **Search**
    * **[Replace across multiple files](https://trello.com/c/NbNEOs4S/264-replace-across-multiple-files)**: You can see all search matches first and uncheck any you don't wish to replace. Supports the same exclusion filtering as Find in Files.
* **Stability**
    * [JavaScript code hinting crash prevention](https://github.com/adobe/brackets/pull/8155): Brackets automatically stops processing problematic JavaScript files that previously could cause a crash during code-hint analysis.
* **Files & Folders**
    * [Rename on second click](https://github.com/adobe/brackets/issues/1976): Click the currently selected file in the tree view a second time to rename it, similar to the gesture available in Windows Explorer and OS X Finder.
* **Localization**
    * [Danish translation added](https://github.com/adobe/brackets/pull/8237)
* **Ongoing Research** (not implemented yet)
    * [Split view](https://trello.com/c/2DWV5tEX/1277-splitview-migrate-workingset-management-to-mainviewmanager) (early implementation on branch)
    * [Research: Theming Support](https://trello.com/c/LHhAcbcU/1260-c-editor-themes) (pull request in progress)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-40...sprint-41#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-40...sprint-41#commits_bucket)


UI Changes
----------
No major changes to existing features.

API Changes
-----------
No known changes to public APIs.

New/Improved Extensibility APIs
-------------------------------
**Search** - Use `FindInFilesUI.searchAndShowResults()` to programmatically trigger a 'Find in Files' operation and show its results in the bottom panel.

Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Include `<main>` tag in HTML code hints](https://github.com/adobe/brackets/pull/8020) by [coliff](https://github.com/coliff)
* [Start Extension Manager on Installed tab if updates are available](https://github.com/adobe/brackets/pull/8058) by [Matt Stow](https://github.com/stowball)
* [Fix #8028: Quick View doesn't recognize linear-gradient if no space before it](https://github.com/adobe/brackets/pull/8057) by [HighwayChile](https://github.com/HighwayChile)
* [Fix bug in new file tree click-to-rename gesture](https://github.com/adobe/brackets/pull/8194) by [Marcel Gerber](https://github.com/SAPlayer)
* [Recognize .command files as Bash scripts](https://github.com/adobe/brackets/pull/8129) by [Eric Newport](https://github.com/kethinov)
* [Create Danish translation](https://github.com/adobe/brackets/pull/8237) by [bodhiBit](https://github.com/bodhiBit)
* [Usability: Only show checkmark next to Working Files sort commands when Automatic Sort is on](https://github.com/adobe/brackets/pull/7845) by [Tomás Malbrán](https://github.com/TomMalbran)
* [About screen shows build timestamp (in dist builds)](https://github.com/adobe/brackets/pull/8033) by [HighwayChile](https://github.com/HighwayChile)
* [Prevent dialogs closed via Enter key from inserting newline into editor](https://github.com/adobe/brackets/pull/7493) by [Martin Zagora](https://github.com/zaggino)
* [Fix #8131: Installing 'Brackets Git' extension can cause Brackets to hang](https://github.com/adobe/brackets/pull/8210) by [Tomás Malbrán](https://github.com/TomMalbran)
* [JSDoc syntax fixes](https://github.com/adobe/brackets/pull/8126) by [Chema Balsas](https://github.com/jbalsas) (see [generated documentation](http://brackets.io/docs/current))
* [Rename Project Settings dismiss button from OK to Done](https://github.com/adobe/brackets/pull/7508) by [Lucas K Allmon](https://github.com/LucasKA)
* [Spanish translation update](https://github.com/adobe/brackets/pull/8245) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Italian translation update](https://github.com/adobe/brackets/pull/8061) by [Denisov21](https://github.com/Denisov21)
* [Finnish translation update](https://github.com/adobe/brackets/pull/6243) ([and](https://github.com/adobe/brackets/pull/8229)) by [valtlait](https://github.com/valtlait)
* [Swedish translation update](https://github.com/adobe/brackets/pull/8239) by [Mikael Jorhult](https://github.com/mikaeljorhult)
* [Turkish translation update](https://github.com/adobe/brackets/pull/8230) by [valtlait](https://github.com/valtlait)
* [Russian translation fixes](https://github.com/adobe/brackets/pull/7998) by [JustAndrei](https://github.com/JustAndrei)
* [Czech translation fix](https://github.com/adobe/brackets/pull/8035) by [kamenitxan](https://github.com/kamenitxan)


#### Pulling source code from Git
* A new brackets-shell build is _not_ required this sprint.
* brackets-shell's Node dependencies have changed. So if you do rebuild it, run `npm install` before rebuilding brackets-shell.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 0.41
--------------------------
For details on the bugs addressed, please refer to [closed sprint 41 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=29&state=closed). Not all fixed bugs will be caught by this search query, however.