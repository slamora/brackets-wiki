_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 39 -- approximately May 5._

What's New in Sprint 39
-----------------------
* **Code Editing**
    * [Switch language/syntax mode of a single file](https://github.com/adobe/brackets/pull/6409): Use the language indicator in the status bar as a dropdown to override the language Brackets chose to treat a file as. (These changes are _not_ yet saved when you close the file, or generalized to all files with a certain extension).
* **Search**
    * [File exclusion filters for Quick Open, Quick Edit, and JS Code Hints](https://trello.com/c/zd77LxFS/1229-file-exclusion-filtering-beyond-find-in-files): Similar to the filtering already available for Find in Files. **TODO: details**
* **Extensions**
    * [Admin features for Extension Registry](https://trello.com/c/NAtggRqE/1224-simple-admin-for-registry): An extension's author can delete the extension from the registry, mark the extension as incompatible with newer versions of Brackets, or transfer ownership to a different author.
* **Ongoing Research** (not implemented yet)
    * [Research: Split-view architecture](https://trello.com/c/8YAFyAZD/500-split-view-multiple-documents): See [architecture proposal](https://github.com/adobe/brackets/wiki/SplitView-Architecture-Notes).

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-38...sprint-39#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-38...sprint-39#commits_bucket)


UI Changes
----------
TODO - Edit/Search menu change

API Changes
-----------
TODO

New/Improved Extensibility APIs
-------------------------------
TODO

Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* On Windows XP, Brackets will not detect external file changes instantly. It behaves similarly to Sprint 35 and earlier releases - changes are detected upon window activation, and the folder tree must be manually refreshed.
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