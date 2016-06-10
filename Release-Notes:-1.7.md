What's New in Release 1.7
-------------------------
 * CEF shell upgrade to 2623: Brackets app shell is now upgraded to CEF 2623. The long standing issue with mouse scroll being too fast on Windows is now fixed with the CEF upgrade.

 * Recent Files Navigation dialog: Ctrl + Tab is now going to bring up the new \"Recent Files\" navigation dialog showing a history of all opened files and allowing one to switch to a file visually. The new dialog also tracks opened files that are not in the working set.

 *  64 bit on MAC: Brackets on MAC is now 64 bit!

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.6...release-1.7#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.6...release-1.7#commits_bucket)

UI Changes
----------
**TODO** - list notable changes in _existing_ features ...or:
No major changes to existing features.


Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------

* [Fixed htmlmixed highlighting.](https://github.com/adobe/brackets/pull/12422) by [ficristo](https://github.com/ficristo)
* [Added unit testing for issue #8208](https://github.com/adobe/brackets/pull/12390) by [PatrickDallarosa](https://github.com/PatrickDallarosa)
* [Fix of not picking file modifications.](https://github.com/adobe/brackets/pull/12353) by [mfatekho](https://github.com/mfatekho)
* [Wrap indent-key to parentheses in .eslintrc](https://github.com/adobe/brackets/pull/12053) by [petetnt](https://github.com/petetnt)
* [Update license year range to latest release](https://github.com/adobe/brackets-shell/pull/545) by [pra85](https://github.com/pra85)
* [Update year to 2016 in LICENSE and about-dialog.html](https://github.com/adobe/brackets/pull/12092) by [pra85](https://github.com/pra85)
* [Korean translation update](https://github.com/adobe/brackets/pull/12151) by [naradesign](https://github.com/naradesign)
* [jasmine.sh: add some log to say which platforms are support](https://github.com/adobe/brackets/pull/12024) by [ficristo](https://github.com/ficristo)
* [Use SVG on image preview backgrounds](https://github.com/adobe/brackets/pull/12165) by [valtlait](https://github.com/valtlait)
* [Update finnish translation for 1.7 release](https://github.com/adobe/brackets/pull/12074) by [petetnt](https://github.com/petetnt)
* [Czech translation for v1.6](https://github.com/adobe/brackets/pull/12046) by [Pavel Dvořák](https://github.com/dvorapa)
* [Add period on line 741](https://github.com/adobe/brackets/pull/12085) by [jshen212](https://github.com/jshen212)
* [Few Portuguese update, spelling and punctuation correction](https://github.com/adobe/brackets/pull/12191) by [MauricioCarmelo](https://github.com/MauricioCarmelo)
* [Line comment bug fix](https://github.com/adobe/brackets/pull/11954) by [borax12](https://github.com/borax12)
* [Update React to 0.14.7 and Immutable to 3.7.6.](https://github.com/adobe/brackets/pull/12035) by [petetnt](https://github.com/petetnt)
* [[HOTFIX] Only update stat and clear contents when old stat is newer than current stat.](https://github.com/adobe/brackets/pull/12175) by [petetnt](https://github.com/petetnt)
* [Rename .eslintrc to .eslintrc.json](https://github.com/adobe/brackets/pull/12237) by [ficristo](https://github.com/ficristo)
* [Add gyp and gypi file extensions to python language](https://github.com/adobe/brackets/pull/12238) by [ficristo](https://github.com/ficristo)
* [[HOTFIX] Only update stat and clear contents when old stat is newer than current stat.](https://github.com/adobe/brackets/pull/12195) by [petetnt](https://github.com/petetnt)
* [Update German translation](https://github.com/adobe/brackets/pull/12068) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add @mfatekho to CLA exceptions.](https://github.com/adobe/brackets/pull/12321) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Add "gradle" to Groovy file extensions](https://github.com/adobe/brackets/pull/12333) by [petetnt](https://github.com/petetnt)
* [Show an error message when trying to rename a file outside of the project](https://github.com/adobe/brackets/pull/12234) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Italian strings.js for 1.7](https://github.com/adobe/brackets/pull/12081) by [Denisov21](https://github.com/Denisov21)
* [Optimize and clean up SVG icons](https://github.com/adobe/brackets/pull/12164) by [valtlait](https://github.com/valtlait)
* [Updated first steps in portuguese language (pt-br)](https://github.com/adobe/brackets/pull/12334) by [nbfontana](https://github.com/nbfontana)
* [Linted CSS](https://github.com/adobe/brackets/pull/12250) by [AllThingsSmitty](https://github.com/AllThingsSmitty)
* [Remove dropshadow from flip-view-buttons](https://github.com/adobe/brackets/pull/12124) by [petetnt](https://github.com/petetnt)
* [Update index.html](https://github.com/adobe/brackets/pull/12344) by [JoshLWScott](https://github.com/JoshLWScott)
* [Update CodeMirror](https://github.com/adobe/brackets/pull/12177) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Remove a number of unnecessary file associations](https://github.com/adobe/brackets-shell/pull/547) by [Triangle717](https://github.com/le717)
* [Update Polish translation (checked and tested)](https://github.com/adobe/brackets/pull/12372) by [jurkian](https://github.com/jurkian)
* [Fix the misaligned selection extension when creating a folder](https://github.com/adobe/brackets/pull/10402) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix for #3296: add deprecation warning for global Mustache](https://github.com/adobe/brackets/pull/11616) by [ficristo](https://github.com/ficristo)
* [Remove a warning when reloading the app](https://github.com/adobe/brackets/pull/12048) by [ficristo](https://github.com/ficristo)
* [Require PathUtils instead of using the global one](https://github.com/adobe/brackets/pull/12203) by [ficristo](https://github.com/ficristo)
* [Fix CSSUtils detection of @import urls](https://github.com/adobe/brackets/pull/12393) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Added Bulgarian translation.](https://github.com/adobe/brackets/pull/12357) by [lyubomirv](https://github.com/lyubomirv)
* [Set Bash files syntax highlighting](https://github.com/adobe/brackets/pull/11558) by [Amin Ullah Khan](https://github.com/sprintr)
* [In CSSUtils, detect when in an @ rule correctly](https://github.com/adobe/brackets/pull/12397) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Remove .jsx from Javascript file extensions and create an jsx-mode](https://github.com/adobe/brackets/pull/12052) by [petetnt](https://github.com/petetnt)
* [Some unit test fixes](https://github.com/adobe/brackets/pull/12437) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Refactoring QuickOpenHTML, CSS, and JavaScript extensions](https://github.com/adobe/brackets/pull/12214) by [jacobsone](https://github.com/jacobsone)
* [Update a couple of dependencies in package.json at the root](https://github.com/adobe/brackets/pull/12059) by [ficristo](https://github.com/ficristo)

#### Pulling source code from Git
_TODO: any brackets-shell updates? which of the below messages are applicable?_

* A new brackets-shell build is _required_ for this sprint. Be sure to rerun `grunt setup` before building.
* Recommended: rebuild or reinstall an updated brackets-shell (no critical updates, but there are bugfixes).
* Rebuilding/updating brackets-shell is _optional_ for this release.
* Rebuilding/updating brackets-shell is _not_ required for this release.
* brackets-shell's Node dependencies have changed. Run `npm install` before rebuilding brackets-shell.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and _then_ `git submodule update --init --recursive` to ensure your local source tree reflects the update.


Bugs fixed in Release 1.7
-------------------------
For details on the bugs addressed, please refer to [closed Release 1.7 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.7%22). Not all fixed bugs will be caught by this search query, however.