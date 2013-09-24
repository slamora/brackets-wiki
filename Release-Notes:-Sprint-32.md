_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 32 -- approximately October 4._

What's New in Sprint 23
-----------------------
* **Images**
    * [Preview image files](https://trello.com/c/l9AcILkC/24-8-preview-images): Select in image in the file tree (or via Quick Open) to see a preview in the editor area.
* **Live Preview**
    * [Launch with non-HTML file selected](https://trello.com/c/gbBtpARq/709-1-define-default-html-file-for-live-development): Brackets will automatically find a nearby index.html file to use. No more switching way from your CSS file to start Live Preview!
* **Overall UI**
    * [Dark Themed Window Chrome on Mac](https://trello.com/c/oyGfEvrK/900-3-into-darkness-shell-osx): Similar to the update on Windows last sprint.
    * [Close Others and Close Others Above/Below in Working Files context menu](https://github.com/adobe/brackets/pull/4590): Quickly close batches of files with these new commands.
* **Under the hood**
    * [Update to Node 0.10](https://trello.com/c/2fqQFO5J/999-2-upgrade-node-to-latest-0-10)


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-31a...sprint-32#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-31a...sprint-32#commits_bucket)


UI Changes
----------
No major changes to existing features.


API Changes
-----------
**Image files** - TODO

**Embedded Node server** - Upgraded from 0.8 to 0.10. Migrated from node-unzip to decompress-zip.

New/Improved Extensibility APIs
-------------------------------
**Linting/inspection** - Use `CodeInspection.inspectFile()` to headlessly (no UI updates) get linting results for any file.

**Closing files** - TODO: new DocumentCommandHandlers API for closing multiple files in a batch

**Paths** - New `NativeFileSystem.isRelativePath()` utility (given a string)

Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. [See workaround](https://support.google.com/chrome/answer/106010?hl=en).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
TODO

#### Pulling source code from Git
TODO
* A new brackets-shell build is _optional_ for this sprint. ??


Bugs fixed in Sprint 32
-----------------------
For details on the bugs addressed, please refer to [closed sprint 32 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=19&state=closed). A few of the fixed bugs might not be caught by this search query, however.