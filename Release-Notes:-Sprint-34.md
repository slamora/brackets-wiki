_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 34 -- approximately November 19._

What's New in Sprint 34
-----------------------
* **Overall UI**
    * [Dark themed window chrome on Mac](https://trello.com/c/oyGfEvrK/900-3-into-darkness-shell-osx): Similar to the update on Windows in Sprint 31.

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-33...sprint-34#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-33...sprint-34#commits_bucket)


UI Changes
----------
**Dark-themed window chrome on Mac** - the Mac shell now has a dark window chrome that visually complements the Brackets UI. (The Windows shell received a similar update in Sprint 31).


API Changes
-----------

**File System API** - TODO


New/Improved Extensibility APIs
-------------------------------
TODO


Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. [See workaround](https://support.google.com/chrome/answer/106010?hl=en).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
TODO

#### Pulling source code from Git
* A new brackets-shell build is _required_ for this sprint (due to new CEF libraries, Dark Shell changes on Mac and Win). Be sure to rerun `grunt setup` before building.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Sprint 34
-----------------------
For details on the bugs addressed, please refer to [closed sprint 34 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=21&state=closed). A few of the fixed bugs might not be caught by this search query, however.