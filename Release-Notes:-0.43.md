_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 0.43 -- approximately August 20._

What's New in Release 0.43
--------------------------
* **TBD...**
* **Search**
    * [Find/Replace: indicate current match index](https://trello.com/c/0GXqt5GF/1089-s-find-next-indicate-current-match-index)
    * [Quick Open: many small bug fixes](https://github.com/adobe/brackets/pull/7227)
* **File Types**
    * [Change language/syntax mode for entire file extension](https://github.com/adobe/brackets/pull/8444): After changing a file's language using the status bar switcher, you can save the change for _all_ files with that extension by choosing "Set as Default" from the same dropdown. (This was previously accessible by manually editing preferences).
* **Ongoing Research** (not implemented yet)
    * [Split view](https://trello.com/c/atD9BEDl/1281-m-splitview-implement-mainviewmanager-code-for-1x2-editors) (early implementation on branch)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-0.42...release-0.43#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-0.42...release-0.43#commits_bucket)


UI Changes
----------
**Quick Open** - The first item is automatically highlighted, making it more clear that you can press Enter immediately to jump to the top item (Down arrow not required). Highlighting the 2nd item in the result list now only requires pressing the Down arrow once (previously required two presses). Quick Find Definition no longer changes the scroll position until you start typing.

**Linux: Edit menu** - The _Edit > Cut/Copy/Paste_ menu items are removed on Linux, because they did not work. These menu items will be restored once [a native menu bar](https://trello.com/c/WMB6vtwO/893-linux-native-menus) is implemented.


API Changes
-----------
**Theme authoring** - Simplified how Find/Replace highlight colors are set. ...TODO...

New/Improved Extensibility APIs
-------------------------------


Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
TBA

#### Pulling source code from Git
* **TODO** - brackets-shell required?
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 0.43
--------------------------
For details on the bugs addressed, please refer to [closed Release 0.43 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+0.43%22). Not all fixed bugs will be caught by this search query, however.