_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 1.0 -- approximately November 4._

What's New in Release 1.0
--------------------------
* **Preferences**
    * [Configure keyboard shortcuts](https://trello.com/c/3mZwu1DE/352-user-specified-keyboard-shortcuts-for-commands-in-json): Users can now customize keyboard shortcuts for internal Brackets commands.  See https://github.com/adobe/brackets/wiki/User-Key-Bindings for more information.
* **Stylesheet Editing**
    * [Collapse unwanted Quick Edit results](https://trello.com/c/nxQ6eGGU/1031-3-css-quick-edit-grouping): Quickly collapse results from a particular CSS, SCSS, or LESS file. For example, when using a preprocessor you can hide results from the compiled CSS file to focus on results in your original SCSS/LESS code.
* **Hint List Searching**
    * [javascript hinting is more accurate](https://github.com/adobe/brackets/issues/3263): Hint-List searching now gives greater weight to Matching by Case so "function" is weighted higher than "Function".
    * [Auto-install bundled extensions](https://github.com/adobe/brackets/issues/9233): Add mechanism to automatically install extensions in `auto-install-extensions` to allow installation "bundles".

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-0.44...release-1.0#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-0.44...release-1.0#commits_bucket)


UI Changes
----------
* Brackets running on Yosemite (OSX 10.10) now has a Yosemite look with flat traffic light buttons. The full-screen button moves to the green traffic light button on Yosemite as well.

* Brackets now works with auto-hide Task Bars on Windows with Maximized windows.

API Changes
-----------
There were no API changes in this release. A wiki on best practices for programmatically opening files in Brackets was published: https://github.com/adobe/brackets/wiki/Programmatically-Opening-Files-in-Brackets 

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


Bugs fixed in Release 1.0
--------------------------
For details on the bugs addressed, please refer to [closed Release 1.0 bugs](https://github.com/adobe/brackets/issues?q=milestone%3A%22Brackets+1.0+%28Release+0.45%29%22+is%3Aclosed) . Not all fixed bugs will be caught by this search query, however.