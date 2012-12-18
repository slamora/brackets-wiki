_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 18 -- approximately December 20._

What's New in Sprint 18
-----------------------
* **TODO: category heading**
    * TODO: feature list


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-17...sprint-18#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-17...sprint-18#commits_bucket)


UI Changes
----------


API Changes
-----------

**NativeFileSystem**
- **requestNativeFileSystem successCallback** - https://github.com/adobe/brackets/pull/2158
The successCallback now returns a FileSystem object with a root:DirectoryEntry property. Previously the callback returned the root:DirectoryEntry itself.
- **Replace FileError with NativeFileError** (implements DOMError) - https://github.com/adobe/brackets/pull/2318
The latest w3 spec passes specifies that error callbacks pass a DOMError object instead of FileError. FileError.code is replaced with DOMError.name. The deprecated enumeration for FileError types has been replaced with an enumeration in NativeFileError (module: file/NativeFileError). This pull request also fixes some consistency issues with error callbacks
- **Check examples and documentation** - https://github.com/adobe/brackets/pull/2063

New/Improved Extensibility APIs
-------------------------------

Known Issues
------------
* [#1551](https://github.com/adobe/brackets/issues/1551): Changes within an extension (or a unit test) are not reflected by a simple "Debug > Reload Brackets." Workarounds:
    * Quit and re-launch Brackets to pick up the changes.
    * Open Developer Tools, click the gear icon in the lower-right, and select "Disable cache." This setting is remembered, but is only in effect so long as the Developer Tools browser tab remains open.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* _Debug > Show Developer Tools_ opens in a new tab in Chrome, rather than a new window in Brackets.
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight sometimes jiggles when horizontally resizing window.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.


Community contributions to Brackets
-----------------------------------
* [title](https://github.com/adobe/brackets/issues/0000) by [name](https://github.com/user)
* [Color Editor CSS cleanups](https://github.com/adobe/brackets/issues/2225) by ["J.M."](https://github.com/mynetx)
* [Go to the most recently visited file when closing a file](https://github.com/adobe/brackets/issues/2122) by [Tomás Malbrán](https://github.com/TomMalbran)
* [HTML attributes code hinting filter](https://github.com/adobe/brackets/issues/2263) by [Chema Balsas](https://github.com/jbalsas)
* [FileSystem Implementation](https://github.com/adobe/brackets/issues/2158) by [Chema Balsas](https://github.com/jbalsas)
* [Fix: If first line in selection is a line comment, Block Comment does nothing](https://github.com/adobe/brackets/issues/2121) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Spanish translation Sprint17](https://github.com/adobe/brackets/issues/2273) by [Chema Balsas](https://github.com/jbalsas)
* [Use \u2026 for ellipsis in menu items](https://github.com/adobe/brackets/issues/2240) by ["J.M."](https://github.com/mynetx)
* [CSS and HTML Line Comment](https://github.com/adobe/brackets/issues/2133) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Create files on collapsed folders](https://github.com/adobe/brackets/issues/2198) by [Chema Balsas](https://github.com/jbalsas)
* [Use Windows key bindings on linux](https://github.com/adobe/brackets/issues/2331) and [#2000](https://github.com/adobe/brackets/pull/2000) by [Pritam Baral](https://github.com/pritambaral)
* [Russsian translation](https://github.com/adobe/brackets/pull/2268) by ["noway421"](https://github.com/noway421) and reviewed by ["TurboTurkey"](https://github.com/TurboTurkey) 
* [Change dialog label for Quick Open based on mode](https://github.com/adobe/brackets/pull/2298) and [#1706](https://github.com/adobe/brackets/pull/1706) by ["sagarsane"](https://github.com/sagarsane)

Contributions _from_ Brackets
-----------------------------

Bugs fixed in Sprint 18
-----------------------
For details on the bugs addressed, please refer to [closed sprint 18 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=5&state=closed). A few of the fixed bugs might not be caught by this search query, however.