_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 0.40 -- approximately June 9._

What's New in Release 0.40
--------------------------
* **Search**
    * [Save multiple file exclusion filters](https://trello.com/c/4EQI1XwC/1137-2s-save-edit-multiple-different-file-exclusion-sets): Instead of a single file/folder search filter, you can name and save multiple different filters for quick use.
* **Extension Development**
    * [Online API documentation](http://brackets.io/docs/current/) is now available - mirroring the JSDocs in the Brackets source code
* **Code Editing**
    * [Toggle line/block comment for CoffeeScript & Lua](https://github.com/adobe/brackets/pull/7135/files#diff-1) (and other languages where the line-comment syntax is a prefix of the block-comment syntax)
* **Ongoing Research** (not implemented yet)
    * [Split view](https://trello.com/c/2DWV5tEX/1277-splitview-migrate-workingset-management-to-mainviewmanager) (early implementation on branch)
    * [Research: Theming Support](https://trello.com/c/LHhAcbcU/1260-c-editor-themes) (pull request in progress)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-39...sprint-40#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-39...sprint-40#commits_bucket)


UI Changes
----------
No major changes to existing features.


API Changes
-----------
**LESS** - Updated to 1.7.0 (from 1.4.2). No known compatibility issues in Brackets - see [changelog](https://github.com/less/less.js/blob/master/CHANGELOG.md).

New/Improved Extensibility APIs
-------------------------------
None added this sprint.


Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
TODO

#### Pulling source code from Git
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.
* A new brackets-shell build is not required, but is recommended for Windows bug fixes.


Bugs fixed in Release 0.40
--------------------------
For details on the bugs addressed, please refer to [closed sprint 40 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=28&state=closed). Not all fixed bugs will be caught by this search query, however.