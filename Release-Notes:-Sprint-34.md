_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 34 -- approximately November 19._

What's New in Sprint 34
-----------------------
* **Installation**
    * [Automatically replace older versions](https://trello.com/c/xxabXFIG/1017-2-brackets-update-in-place-via-installer): The Brackets installer on Windows now automatically overwrites previous versions, with no need to manually uninstall them. On all platforms, the app name no longer contains the sprint number. If you have manually assigned Brackets to any file associations, they will no longer need to be reassigned for each new build. (Note: these changes will not be evident until the first update, i.e. _next sprint_ - Sprint 35).
* **Live Development**
    * [Mac: No browser restart needed](https://github.com/adobe/brackets-shell/pull/359): Launching Live Preview no longer requires closing and reopening Chrome on Mac. (This was [already](https://github.com/adobe/brackets/wiki/Release-Notes:-Sprint-25) true on Windows).
* **Overall UI**
    * [Dark themed window chrome on Mac](https://trello.com/c/oyGfEvrK/900-3-into-darkness-shell-osx): Similar to the update on Windows in Sprint 31.
    * Notable bug fixed in Win dark-themed window chrome
* **Code Hints**
    * Improved JavaScript code hints accuracy: More code hints provided for APIs in medium-sized files (500-2000 lines) and in cases where a module's exports have recently been updated.
* **Linting**
    * Multiple linting providers per Language: If multiple providers are registered, they will all be run and the results consolidated.
* **Under the hood**
    * [New file system API](https://trello.com/c/PO7CIgqf/1050-1-merge-new-file-system): Some [API changes for extensions](https://github.com/adobe/brackets/wiki/File-System-API-Migration) -- see below. Slightly improves performance of some features, like Find in Files (larger improvements to come later!).
* **Localization**
    * Updated translations: ....


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-33...sprint-34#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-33...sprint-34#commits_bucket)


UI Changes
----------
**Dark-themed window chrome on Mac** - the Mac shell now has a dark window chrome that visually complements the Brackets UI. (The Windows shell received a similar update in Sprint 31).

**Installation** - see above. Starting _next_ sprint, newer versions of Brackets will overwrite previously installed versions.

**Extensions** - the Extension Manager 'Available' and 'Installed' tabs have switched placed.


API Changes
-----------
**File System API** - TODO

**Linting** - TODO: new way needed to replace default JS linter

**Closing Files** - TODO: mention `Commands.FILE_CLOSE_LIST` once API cleaned up


New/Improved Extensibility APIs
-------------------------------
**Quick Open** - TODO: extensions can specify a label

**Documents** - TODO: new, faster getDocumentText() API


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
TODO: is new brackets-shell build required?
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Sprint 34
-----------------------
For details on the bugs addressed, please refer to [closed sprint 34 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=21&state=closed). A few of the fixed bugs might not be caught by this search query, however.