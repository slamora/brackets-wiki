_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 37 -- approximately February 27._

What's New in Release #37
-----------------------
* **Installation**
    * [Code-signed builds (Mac & Win)](https://trello.com/c/g5ZY1lKY/1131-code-signing-on-win-and-mac): Eliminates some warnings on launch and install. Gatekeeper no longer prevents launching Brackets by default on Mac.
* **Find**
    * [Configurable exclusions from Find in Files](https://trello.com/c/7Svh6B4Z/1085-exclude-files-folders-from-an-individual-find-in-files-operation): Choose file names, paths, or wildcards to exclude from Find in Files search.
* **Preferences**
    * [Revamped preferences APIs ready for use](https://github.com/adobe/brackets/pull/6715): TODO
    * [Migrate view state to new preference storage format](https://trello.com/c/IuFGyICH/1155-preferences-view-state-migration): TODO
* **Ongoing Research** (not enabled yet)
    * [Multiple cursor/selection support](https://trello.com/c/urTCdTZj/1156-multiple-cursors-initial-implementation-on-branch): Initial implementation on a branch, not exposed in this release of Brackets yet.
    * [Research JS Code Hints Cleanup](https://trello.com/c/heHZlATB/1158-research-js-code-hints-cleanup): Investigate how to simplify the code, improve performance & maintainability, and unify with new preferences system.

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-36...sprint-37#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-36...sprint-37#commits_bucket)


UI Changes
----------
No major changes to existing features.


API Changes
-----------
**Preferences** - TODO

**File System** - Paths with unsupported Windows-style separators ("\" instead of "/") are now explicitly rejected immediately; previously they were accepted by FileSystem but behaved brokenly in various ways.

**Editor** - The `optionChange` event now uses the Brackets names for the options, rather than the CodeMirror names (for example "wordWrap" rather than "lineWrapping"). See Editor.js for [the complete list](https://github.com/adobe/brackets/blob/master/src/editor/Editor.js#L79).

New/Improved Extensibility APIs
-------------------------------
**Optional requirejs-config.json** - Allows extensions to define their own RequireJS configuration file `requirejs-config.json` before the `main.js` entry point loads. File format must follow http://requirejs.org/docs/api.html#config. [Read more...](https://github.com/adobe/brackets/pull/6671)


Known Issues
------------
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
