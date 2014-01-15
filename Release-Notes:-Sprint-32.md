What's New in Sprint 32
-----------------------
* **Live Preview**
    * [Launch with non-HTML file selected](https://trello.com/c/gbBtpARq/709-1-define-default-html-file-for-live-development): Brackets will automatically find a nearby index.html file to use. No more switching away from your CSS file to start Live Preview!
* **Performance**
    * [Improved Find performance](https://github.com/adobe/brackets/pull/5303) when searching in large files
    * [Fixed lag while editing JS code inside a \<script> block](https://github.com/adobe/brackets/pull/5395)
* **Overall UI**
    * [Fixed](https://github.com/adobe/brackets-shell/pull/347) [bugs](https://github.com/adobe/brackets-shell/pull/345) in Windows dark-themed window chrome
    * [HTML syntax highlighting visible while cursor on tag](https://github.com/adobe/brackets/pull/5355): HTML close/open tag highlighting now permits the regular syntax highlighting to show through.
    * [Quick Edit results list easier to scan](https://github.com/adobe/brackets/pull/5230): File name is better distinguished from selector text.
* **Under the hood**
    * [Update to Node 0.10](https://trello.com/c/2fqQFO5J/999-2-upgrade-node-to-latest-0-10)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-31a...sprint-32#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-31a...sprint-32#commits_bucket)


API Changes
-----------
**Embedded Node server** - Upgraded from 0.8 to 0.10. Migrated from node-unzip to decompress-zip. (Only affects extensions that use NodeConnection to run code on the Node server side).

**Paths** - `FileUtils.getFilenameExtension()` has been **deprecated** in favor of the new `FileUtils.getFileExtension()`. The old API may be removed in a near-future sprint.

New/Improved Extensibility APIs
-------------------------------
**Linting/inspection** - Use `CodeInspection.inspectFile()` to headlessly (no UI updates) get linting results for any file.

**Paths** - New `NativeFileSystem.isRelativePath()` utility (given a string).


Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. [See workaround](https://support.google.com/chrome/answer/106010?hl=en).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Add CodeInspection.inspectFile() headless API](https://github.com/adobe/brackets/pull/5125) by [Martin Zagora](https://github.com/zaggino)
* [Fix #5310: Many items in 'Edit' menu do nothing on Linux](https://github.com/adobe/brackets/pull/5311) ([and](https://github.com/adobe/brackets/pull/5350)) by [Arzhan "kai" Kinzhalin (Intel Corporation)](https://github.com/busykai)
* [Fix #5362: Linux menu dropdowns appear below bottom panel headers](https://github.com/adobe/brackets/pull/5363) by [Arzhan "kai" Kinzhalin (Intel Corporation)](https://github.com/busykai)
* [Add <dialog> to HTML tag hinting](https://github.com/adobe/brackets/pull/5390) by [Oskar Tjoskar](https://github.com/tjoskar)
* [Highlight .jsm files as JavaScript](https://github.com/adobe/brackets/pull/5340) by [Neil Stansbury](https://github.com/nstansbury)
* [Highlight ASP.NET .master files as JavaScript](https://github.com/adobe/brackets/pull/5320) by [Clay Miller](https://github.com/smockle)
* [Highlight Dust.js (.dust) templates as generic EJS templates](https://github.com/adobe/brackets/pull/5370) by [Josh Parolin](https://github.com/joshparolin)
* [Re-enable unit tests for HTML Menu rendering](https://github.com/adobe/brackets/pull/5290) by [Lance Campbell](https://github.com/lkcampbell)
* [Disable 'Restart' button in Debug > Switch Language when language not changed](https://github.com/adobe/brackets/pull/5287) by [Marcel Gerber](https://github.com/SAPlayer)
* [Fix #5173: Find in Files titlebar doesn't support proper localization](https://github.com/adobe/brackets/pull/5274) by [Tomás Malbrán](https://github.com/TomMalbran)
* [German translation fix](https://github.com/adobe/brackets/pull/5285) by [Marcel Gerber](https://github.com/SAPlayer)
* [Turkish translation update](https://github.com/adobe/brackets/pull/4808) by [Muhammet Ali Bekiroglu](https://github.com/Cryptexx)
* [Czech translation update](https://github.com/adobe/brackets/pull/5127) by [kvarel](https://github.com/kvarel)
* [Czech translation update](https://github.com/adobe/brackets/pull/5227) by [Martin Starman](https://github.com/martinstarman)
* [Czech translation typo fix](https://github.com/adobe/brackets/pull/5142) by [Martin Starman](https://github.com/martinstarman)
* [Finnish translation screenshot](https://github.com/adobe/brackets/pull/5373) by [Wouter92](https://github.com/Wouter92)
* [Fix JSDoc parameter name](https://github.com/adobe/brackets/pull/5389) by [Ashok Gelal](https://github.com/ashokgelal)

#### Pulling source code from Git
* It is now possible to build brackets-shell with XCode 5.
* A new brackets-shell build is _required_ for this sprint (due to the Node upgrade). Be sure to rerun `grunt setup` before building.
* brackets-shell's Node dependencies have changed. Run `npm install` before rebuilding brackets-shell.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Sprint 32
-----------------------
For details on the bugs addressed, please refer to [closed sprint 32 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=19&state=closed). A few of the fixed bugs might not be caught by this search query, however.