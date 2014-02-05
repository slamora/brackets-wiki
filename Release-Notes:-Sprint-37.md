_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 37 -- approximately February 17._

What's New in Sprint 37
-----------------------
TODO

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-36...sprint-37#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-36...sprint-37#commits_bucket)


UI Changes
----------
No major changes to existing features.


API Changes
-----------
**File System** - Paths with unsupported Windows-style separators ("\" instead of "/") are now explicitly rejected immediately; previously they were accepted by FileSystem but behaved brokenly in various ways.

New/Improved Extensibility APIs
-------------------------------
**Optional requirejs-config.json** - Allows extensions to define their own RequireJS configuration file `requirejs-config.json` before the `main.js` entry point loads. See https://github.com/adobe/brackets/pull/6671. File format must follow http://requirejs.org/docs/api.html#config.

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
TODO


Bugs fixed in Sprint 37
-----------------------
For details on the bugs addressed, please refer to [closed sprint 37 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=24&state=closed). Not all fixed bugs will be caught by this search query, however.
