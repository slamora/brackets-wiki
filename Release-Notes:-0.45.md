_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 0.45 -- approximately November 3._

What's New in Release 0.45
--------------------------
* **Preferences**
    * [Configure keyboard shortcuts](https://trello.com/c/3mZwu1DE/352-user-specified-keyboard-shortcuts-for-commands-in-json): ...
* **Stylesheet Editing**
    * [Collapse unwanted Quick Edit results](https://trello.com/c/nxQ6eGGU/1031-3-css-quick-edit-grouping): Quickly collapse results from a particular CSS, SCSS, or LESS file. For example, when using a preprocessor you can hide results from the compiled CSS file to focus on results in your original SCSS/LESS code.


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-0.44...release-0.45#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-0.44...release-0.45#commits_bucket)


UI Changes
----------
TODO


API Changes
-----------
TODO

New/Improved Extensibility APIs
-------------------------------
**Themes** - Themes no longer need to include a dummy 'main.js' file. Theme authors may wish to continue including this file in the short term for compatibility with Brackets 0.44, however.


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


Bugs fixed in Release 0.45
--------------------------
For details on the bugs addressed, please refer to [closed Release 0.45 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+0.45%22). Not all fixed bugs will be caught by this search query, however.