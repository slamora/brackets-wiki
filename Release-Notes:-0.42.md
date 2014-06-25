_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 0.42 -- approximately July 22._

What's New in Release 0.42
--------------------------
* ...TBD...
* **Search**
    * [Quick Open: many small bug fixes](https://github.com/adobe/brackets/pull/7227)
* **File Types**
    * [Switch language/syntax mode of a single file](https://github.com/adobe/brackets/pull/6409): Use the language indicator in the status bar as a dropdown to override the language Brackets chose to treat a file as. (These changes are _not_ saved when you close the file; use [preferences](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#preferences) to permanently treat all files with a certain extension as a different language).


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-41...sprint-42#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-41...sprint-42#commits_bucket)


UI Changes
----------
**Quick Open** - The first item is automatically highlighted, making it more clear that you can press Enter immediately to jump to the top item (Down arrow not required). Highlighting the 2nd item in the result list now only requires pressing the Down arrow once (previously required two presses). Quick Find Definition no longer changes the scroll position until you start typing.


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
TBD

#### Pulling source code from Git
* TODO: new brackets-shell build required?
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 0.42
--------------------------
For details on the bugs addressed, please refer to [closed sprint 42 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=30&state=closed). Not all fixed bugs will be caught by this search query, however.