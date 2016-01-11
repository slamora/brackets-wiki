_This is a draft!_
--------------------
_This page will not be finalized until the end of Release 1.6 -- approximately January 14._

What's New in Release 1.6
-------------------------
* _**TODO: draft**_
* **TODO: category heading**
   * _TODO: feature list_
* **Localization**
   * Translation updates for: _(TODO - updated locales in alphabetical order)_


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.5...release-1.6#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.5...release-1.6#commits_bucket)


UI Changes
----------
**TODO** - list notable changes in _existing_ features ...or:
No major changes to existing features.


API Changes
-----------
**TODO** - list breaking API changes

New/Improved Extensibility APIs
-------------------------------
**TODO** - list API expansions


Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
**TODO**

#### Pulling source code from Git
_TODO: any brackets-shell updates? which of the below messages are applicable?_

* A new brackets-shell build is _required_ for this sprint. Be sure to rerun `grunt setup` before building.
* Recommended: rebuild or reinstall an updated brackets-shell (no critical updates, but there are bugfixes).
* Rebuilding/updating brackets-shell is _optional_ for this release.
* Rebuilding/updating brackets-shell is _not_ required for this release.
* brackets-shell's Node dependencies have changed. Run `npm install` before rebuilding brackets-shell.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and _then_ `git submodule update --init --recursive` to ensure your local source tree reflects the update.


Bugs fixed in Release 1.6
-------------------------
For details on the bugs addressed, please refer to [closed Release 1.6 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.6%22). Not all fixed bugs will be caught by this search query, however.