_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 0.41 -- approximately June 26._

What's New in Release 0.41
--------------------------
* **Search**
    * **[Replace across multiple files](https://trello.com/c/NbNEOs4S/264-replace-across-multiple-files)**: You can see all search matches first and uncheck any you don't wish to replace. Supports the same exclusion filtering as Find in Files.
* **Code Hints**
    * **[JavaScript Code Hinting Crash Prevention](https://github.com/adobe/brackets/pull/8155)**: Brackets automatically stops processing problematic JavaScript files that previously could cause a crash.
* **Documentation**
    * **[Brackets API Documentation](http://brackets.io/docs)**: Docs for the current master branch is now posted at http://brackets.io/docs and updated for every release.
* **Ongoing Research** (not implemented yet)
    * [Split view](https://trello.com/c/2DWV5tEX/1277-splitview-migrate-workingset-management-to-mainviewmanager) (early implementation on branch)
    * [Research: Theming Support](https://trello.com/c/LHhAcbcU/1260-c-editor-themes) (pull request in progress)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-40...sprint-41#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-40...sprint-41#commits_bucket)


UI Changes
----------


API Changes
-----------

New/Improved Extensibility APIs
-------------------------------


Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
...TODO...

* [Include `<main>` tag in HTML code hints](https://github.com/adobe/brackets/pull/8020) by [coliff](https://github.com/coliff)
* [Change Project Settings dismiss button from OK to Done](https://github.com/adobe/brackets/pull/7508) by [Lucas K Allmon](https://github.com/LucasKA)
* [Russian translation fixes](https://github.com/adobe/brackets/pull/7998) by [JustAndrei](https://github.com/JustAndrei)


#### Pulling source code from Git
* TODO: new brackets-shell build required?
* brackets-shell's Node dependencies have changed. Run `npm install` before rebuilding brackets-shell.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 0.41
--------------------------
For details on the bugs addressed, please refer to [closed sprint 41 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=29&state=closed). Not all fixed bugs will be caught by this search query, however.