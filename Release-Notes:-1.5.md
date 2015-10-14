
What's New in Release 1.5
-------------------------
* **Fold Selected Text**: Select any text block in the editor and fold markers will appear to collapse the selected text.

* **Instant Search (Improved Stability and Performance)**: Major stability and performance improvements. Find in Files can now index and search robustly in thousands of files instantly.

* **CSS Code Hints in PHP**: CSS code hints are now available for style elements inside PHP files.

* **Remember Collapsed Linters**: Remember collapsed state of linters on switching files.

* **Improve JS Code Hints performance in minified files**: JS Code Hints now perform more efficiently with minified files

* **Resolve Brackets freezing/crashing on windows on reload**: Fixed the crash when reloading Brackets through "Debug > Reload With Extensions" menu

* **El Capitan Fixes**: Fixed full-screen bugs with Brackets on El Capitan.

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.4...release-1.5#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.4...release-1.5#commits_bucket)


Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------

Special thanks to [Patrick Oladimeji](https://github.com/thehogfather) for [Selection Based Code Collapse](https://github.com/adobe/brackets/pull/11538) feature and [ficristo](https://github.com/ficristo) for [Remember Collapsed Linters](https://github.com/adobe/brackets/pull/11641)

* [Allow code-folding for selected text in editor](https://github.com/adobe/brackets/pull/11538) by [Patrick Oladimeji](https://github.com/thehogfather)
* [Resolve Brackets freezing/crashing on windows on reload.](https://github.com/adobe/brackets/pull/11505) by [hussainb](https://github.com/hussainb)
* [Allow CSS Code Hints in PHP] (https://github.com/adobe/brackets/pull/11751) by [Amin Ullah Khan](https://github.com/sprintr)
* [Remember linter collapsed](https://github.com/adobe/brackets/pull/11641) by [ficristo](https://github.com/ficristo)
* [Fixed a bug when there is only a linter error and it is collapsed](https://github.com/adobe/brackets/pull/11721) by [ficristo](https://github.com/ficristo)
* [Do not run jshint tasks twice](https://github.com/adobe/brackets/pull/11664) by [ficristo](https://github.com/ficristo)
* [Improve JS Code Hints performance in minified files](https://github.com/adobe/brackets/pull/11123) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add --disable-default-apps-flag for live preview](https://github.com/adobe/brackets-shell/pull/533) by [petetnt](https://github.com/petetnt)
* [Update Lodash -> 3.10.0](https://github.com/adobe/brackets/pull/11474) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update CodeMirror](https://github.com/adobe/brackets/pull/11528) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update LESS -> 2.5.1](https://github.com/adobe/brackets/pull/10240) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update phantomjs -> 1.9.18](https://github.com/adobe/brackets/pull/11695) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update CodeMirror](https://github.com/adobe/brackets/pull/11652) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add ES6 keywords](https://github.com/adobe/brackets/pull/11645) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add grunt task to check for correct nls keys](https://github.com/adobe/brackets/pull/11299) by [Marcel Gerber](https://github.com/MarcelGerber)
* [fixes #11552](https://github.com/adobe/brackets/pull/11553) by [Martin Zagora](https://github.com/zaggino)
* [Convert numbers to strings, so StringMatch can match it.](https://github.com/adobe/brackets/pull/11484) by [Amin Ullah Khan](https://github.com/sprintr)
* [Add more translatable keys](https://github.com/adobe/brackets/pull/11224) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Split Handlebars into its own language](https://github.com/adobe/brackets/pull/11295) by [Amin Ullah Khan](https://github.com/sprintr)
* [Highlight .xsd files as XML](https://github.com/adobe/brackets/pull/11506) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Move getInitialQuery from FindUtils to FindBar](https://github.com/adobe/brackets/pull/11640) by [ficristo](https://github.com/ficristo)
* [Remove deprecated file system APIs](https://github.com/adobe/brackets/pull/9622) by [Triangle717](https://github.com/le717)
* [Move PathUtils to Brackets core code](https://github.com/adobe/brackets/pull/11734) by [ficristo](https://github.com/ficristo)
* [Revert "Move PathUtils to Brackets core code"](https://github.com/adobe/brackets/pull/11745) by [Martin Zagora](https://github.com/zaggino)
* [Correct URL](https://github.com/adobe/brackets/pull/11500) by [rafaelstz](https://github.com/rafaelstz)
* [Update note in readme](https://github.com/adobe/brackets-shell/pull/531) by [stevemao](https://github.com/stevemao)
* [Minor spelling correction](https://github.com/adobe/brackets/pull/11678) by [jiimaho](https://github.com/jiimaho)
* [Update strings.js](https://github.com/adobe/brackets/pull/11507) by [xxxtonixxx](https://github.com/xxxtonixxx)
* [Avbröst changed to Avbröts](https://github.com/adobe/brackets/pull/11711) by [robertkarlsson](https://github.com/robertkarlsson)
* [Finnish translation, release 1.5](https://github.com/adobe/brackets/pull/11690) by [valtlait](https://github.com/valtlait)
* [PT-BR translation update](https://github.com/adobe/brackets/pull/11583) by [Henrique Aparecido Lavezzo](https://github.com/Rynaro)
* [Update zh-TW translation](https://github.com/adobe/brackets/pull/11655) by [Pei-Tang Huang](https://github.com/tan9)
* [Update Portuguese string](https://github.com/adobe/brackets/pull/11546) by [rafaelstz](https://github.com/rafaelstz)
* [Update German Translation](https://github.com/adobe/brackets/pull/11716) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update Croation Translation](https://github.com/adobe/brackets/pull/11522) by [Kruno H](https://github.com/diomed)

#### Pulling source code from Git

* A new brackets-shell build is _required_ for this sprint. Be sure to rerun `grunt setup` before building.
* Recommended: rebuild or reinstall an updated brackets-shell (no critical updates, but there are bugfixes).
* Rebuilding/updating brackets-shell is _optional_ for this release.
* Rebuilding/updating brackets-shell is _not_ required for this release.
* brackets-shell's Node dependencies have changed. Run `npm install` before rebuilding brackets-shell.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and _then_ `git submodule update --init --recursive` to ensure your local source tree reflects the update.


Bugs fixed in Release 1.5
-------------------------
For details on the bugs addressed, please refer to [closed Release 1.5 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.5%22). Not all fixed bugs will be caught by this search query, however.