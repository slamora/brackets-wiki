_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 0.40 -- approximately June 9._

What's New in Release 0.40
--------------------------
* **Search**
    * [Save multiple file exclusion filters](https://trello.com/c/4EQI1XwC/1137-2s-save-edit-multiple-different-file-exclusion-sets): Instead of a single file/folder search filter, you can name and save multiple different filters for quick use.
    * [Highlight all occurrences of word under cursor](https://github.com/adobe/brackets/pull/7748): Set the `highlightSelectionMatches` [preference](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#preferences) to enable this
* **Extension Development**
    * [Online API documentation](http://brackets.io/docs/current/) is now available - mirroring the JSDocs in the Brackets source code
* **Code Editing**
    * [Toggle line/block comment for CoffeeScript & Lua](https://github.com/adobe/brackets/pull/7135/files#diff-1) (and other languages where the line-comment syntax is a prefix of the block-comment syntax, or the block-comment open & close tokens are identical)
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
* [Preference to highlight all occurrences of word under cursor](https://github.com/adobe/brackets/pull/7748) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Support line-comment syntax that is prefix of block-comment syntax, & identical block-comment open & close tokens. Enable Toggle Line/Block Comment for CoffeeScript & Lua.](https://github.com/adobe/brackets/pull/7135) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Extension Manager: decrease green update count when an extension is marked for update](https://github.com/adobe/brackets/pull/7863) by [Marcel Gerber](https://github.com/SAPlayer)
* [Fix #5226/#7458: Scroll view if needed after Edit > Move Line Up/Down](https://github.com/adobe/brackets/pull/7829) by [Lance Campbell](https://github.com/lkcampbell)
* [Fix #7598: Ensure opening a dialog always blurs code editor](https://github.com/adobe/brackets/pull/7677) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #7905: Project tree shouldn't gray out dotfiles](https://github.com/adobe/brackets/pull/8013) by [Steffen Bruchmann](https://github.com/sbruchmann)
* [Don't allow creating empty file filters in New/Edit Exclusion Set dialog](https://github.com/adobe/brackets/pull/7965) by [Marcel Gerber](https://github.com/SAPlayer)
* [Treat .cljs and .cljx files as Clojure](https://github.com/adobe/brackets/pull/7854) by [Julien Eluard](https://github.com/jeluard)
* [Extension Manager: show dates in locale-appropriate format](https://github.com/adobe/brackets/pull/7745) by [Marcel Gerber](https://github.com/SAPlayer)
* [Update Less to 1.7.0](https://github.com/adobe/brackets/pull/6730) ([and](https://github.com/adobe/brackets/pull/7956)) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Add 'Help > Brackets Homepage' menu item](https://github.com/adobe/brackets/pull/7746) ([and](https://github.com/adobe/brackets/pull/7870)) by [Bernd Schwarzenbacher](https://github.com/BerndSchwarzenbacher)
* [Improve wording of file encoding error message](https://github.com/adobe/brackets/pull/7932) by [HighwayChile](https://github.com/HighwayChile)
* [Fix UpdateNotification unit test](https://github.com/adobe/brackets/pull/7819) by [Marcel Gerber](https://github.com/SAPlayer)
* [Fix CSS so striped tables in Brackets UI can contain non-striped nested tables](https://github.com/adobe/brackets/pull/7779) by [Fez Vrasta](https://github.com/FezVrasta)
* [Cleanup: Remove unneeded brackets-shell API existence check](https://github.com/adobe/brackets/pull/7885) by [Triangle717](https://github.com/le717)
* [German translation update](https://github.com/adobe/brackets/pull/8000) by [Marcel Gerber](https://github.com/SAPlayer)
* [Czech translation update](https://github.com/adobe/brackets/pull/7565) by [kvarel](https://github.com/kvarel)
* [Polish translation update](https://github.com/adobe/brackets/pull/7574) by [Olgierd Grzyb](https://github.com/winek)
* [Brazilian Portuguese translation update](https://github.com/adobe/brackets/pull/7847) by [Rodrigo Tavares](https://github.com/rodrigost23)
* [Norwegian translation update](https://github.com/adobe/brackets/pull/7924) by [steinmortenhugubakken](https://github.com/steinmortenhugubakken)
* [Croatian translation update](https://github.com/adobe/brackets/pull/7940) ([and](https://github.com/adobe/brackets/pull/7871)) by [Kruno H](https://github.com/diomed)
* [Spanish translation update](https://github.com/adobe/brackets/pull/8041) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Russian translation update](https://github.com/adobe/brackets/pull/7837) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Russian translation typo fix](https://github.com/adobe/brackets/pull/8027) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Fix capitalization typo in README](https://github.com/adobe/brackets/pull/7876) by [Zach Ziccardi](https://github.com/zziccardi)

#### Pulling source code from Git
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.
* A new brackets-shell build is not required, but is recommended for Windows bug fixes.


Bugs fixed in Release 0.40
--------------------------
For details on the bugs addressed, please refer to [closed sprint 40 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=28&state=closed). Not all fixed bugs will be caught by this search query, however.