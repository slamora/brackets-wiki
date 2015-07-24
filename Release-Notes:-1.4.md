What's New in Release 1.4
-------------------------
* **Instant Search in Files**: Searching across files is faster than ever. Results are now displayed incrementally and updated as you type.

* **Easier Preferences Editing**: All preferences are now available as code hints while editing the `brackets.json` preferences file. Opening a preferences file (**Debug-> Open Preferences file**) will also open the default preferences in a second pane in split mode. This default preferences file will give hand on to user about the preferences and how to use and set values.

* **Disable Extensions Individually**: Extensions can now be individually enabled/disabled from the Extension Manager. Here each extension which has been used will be provided with an option to enable, disable and remove extension from the Extension Manager. 

* **Improved Text Rendering on Mac**: Crisp text rendering on Mac with subpixel antialiasing enabled by default. You can use the older text rendering by setting the `fonts.fontSmoothing` preference to `antialiased`.

* **Greek and Cyrillic Character support**: Updated editor font (Source Code Pro) with improved support for Greek and Cyrillic character sets.

* **Traverse files in List Order**: Files opened in the working set can be traversed in the list order with CTRL+PageUp and CTRL+PageDown.

* **Localization**
   * Translation updates for: Czech, German, Finnish, French, Japanese, Italian, Swedish


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.3...release-1.4#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.3...release-1.4#commits_bucket)

Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* **Special thanks to [Amin Ullah Khan](https://github.com/sprintr) and [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)** for their major effort contributing to *Preferences Code Hints* and *Enable/Disable extensions individually* features.
    
* [Preferences Code Hints](https://github.com/adobe/brackets/pull/11130) by [Amin Ullah Khan](https://github.com/sprintr)
* [Docs for PreferencesManager.definePreference](https://github.com/adobe/brackets/pull/11262) by [Amin Ullah Khan](https://github.com/sprintr)
* [Define all preferences in core](https://github.com/adobe/brackets/pull/11197) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Move descriptions to their preferences](https://github.com/adobe/brackets/pull/11201) by [Amin Ullah Khan](https://github.com/sprintr)
* [Disable and enable extensions](https://github.com/adobe/brackets/pull/11184) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Use Ctrl+PageUp/PageDown to traverse files in list order](https://github.com/adobe/brackets/pull/11223) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix #10786 - Allow QuickView image preview for arbitrary URLs](https://github.com/adobe/brackets/pull/10788) by [David Humphrey](https://github.com/humphd)
* [Update CodeMirror](https://github.com/adobe/brackets/pull/11167) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update CodeMirror](https://github.com/adobe/brackets/pull/11071) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Escape RegExp specials in _createSpecialLineExp](https://github.com/adobe/brackets/pull/11107) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Clean-up servers when closing LiveDevelopment](https://github.com/adobe/brackets/pull/10453) by [Sebastian Salvucci (Intel Corp)](https://github.com/sebaslv)
* [Italian translation review](https://github.com/adobe/brackets/pull/11117) by [Fredev](https://github.com/Fredev)
* [Clean up dependencies in brackets.js](https://github.com/adobe/brackets/pull/10596) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Shim Error.prototype.stack for browsers that lack it.](https://github.com/adobe/brackets/pull/11124) by [David Humphrey](https://github.com/humphd)
* [added Slack channel and re-ordered contact order](https://github.com/adobe/brackets/pull/11148) by [Andrew MacKenzie](https://github.com/mackenza)
* [Rename CodeMirror2 folder to CodeMirror](https://github.com/adobe/brackets/pull/11150) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Sample projects: remove trailing whitespace on non-blank lines, fix arrow](https://github.com/adobe/brackets/pull/10926) by [valtlait](https://github.com/valtlait)
* [Fix #10821. Allow page reloads, navigating away and back etc.](https://github.com/adobe/brackets/pull/10822) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [.t is the standard extension of test scripts in Perl](https://github.com/adobe/brackets/pull/11189) by [szabgab](https://github.com/szabgab)
* [Make font settings react to pref changes](https://github.com/adobe/brackets/pull/11190) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix horizontal scrolling of code hints](https://github.com/adobe/brackets/pull/11195) by [Amin Ullah Khan](https://github.com/sprintr)
* [Revert "Fix horizontal scrolling of code hints"](https://github.com/adobe/brackets/pull/11199) by [Amin Ullah Khan](https://github.com/sprintr)
* [Fix errors in src/utils/AppInit.js documentation](https://github.com/adobe/brackets/pull/11225) by [petetnt](https://github.com/petetnt)
* [Enable stylus support](https://github.com/adobe/brackets/pull/11234) by [Amin Ullah Khan](https://github.com/sprintr)
* [Allow JavaScript Code Hints in PHP Files](https://github.com/adobe/brackets/pull/11245) by [Amin Ullah Khan](https://github.com/sprintr)
* [implement sendCommandProgress and progress callback](https://github.com/adobe/brackets-shell/pull/509) by [Martin Zagora](https://github.com/zaggino)
* [Implement progress callback so node domains are able to notify the Brackets before they finish](https://github.com/adobe/brackets/pull/10761) by [Martin Zagora](https://github.com/zaggino)
* [Korean translation update](https://github.com/adobe/brackets/pull/11101) by [Yongmin Hong](https://github.com/revi)
* [Remove trailing whitespace in Korean translation](https://github.com/adobe/brackets/pull/11278) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix unittests](https://github.com/adobe/brackets/pull/11275) by [Amin Ullah Khan](https://github.com/sprintr)
* [Add CodeMirror simple-mode addon](https://github.com/adobe/brackets/pull/11280) by [Amin Ullah Khan](https://github.com/sprintr)
* [Add deprecation warning for convertPreferences()](https://github.com/adobe/brackets/pull/11174) by [Triangle717](https://github.com/le717)
* [Detect keyword 'linear' on lines containing word 'animation' too](https://github.com/adobe/brackets/pull/10989) by [petetnt](https://github.com/petetnt)
* [Update Dutch translation](https://github.com/adobe/brackets/pull/11208) by [coreice](https://github.com/coreice)
* [Don't try to handle protocol-relative URLs from Tern](https://github.com/adobe/brackets/pull/10647) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Allow JavaScript Code Hints only in its context](https://github.com/adobe/brackets/pull/11263) by [Amin Ullah Khan](https://github.com/sprintr)
* [Fixed a few typos](https://github.com/adobe/brackets/pull/11302) by [Amin Ullah Khan](https://github.com/sprintr)
* [Update strings.js](https://github.com/adobe/brackets/pull/11312) by [Pavel Dvořák](https://github.com/dvorapa)
* [QuickView for named colors in Stylus](https://github.com/adobe/brackets/pull/11331) by [Amin Ullah Khan](https://github.com/sprintr)
* [US misspellings and typos](https://github.com/adobe/brackets/pull/11343) by [Pavel Dvořák](https://github.com/dvorapa)
* [style typo](https://github.com/adobe/brackets/pull/11349) by [Pavel Dvořák](https://github.com/dvorapa)
* [Update German translation](https://github.com/adobe/brackets/pull/11325) by [Marcel Gerber](https://github.com/MarcelGerber)
* [String change for "Next/Previous document" in list order](https://github.com/adobe/brackets/pull/11330) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Czech translation for v1.4](https://github.com/adobe/brackets/pull/11347) by [Pavel Dvořák](https://github.com/dvorapa)
* [Simplify PerfUtils.getValueAsString](https://github.com/adobe/brackets/pull/11381) by [Marcel Gerber](https://github.com/MarcelGerber)
* [German translation](https://github.com/adobe/brackets/pull/11371) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Another load of typos](https://github.com/adobe/brackets/pull/11372) by [Pavel Dvořák](https://github.com/dvorapa)
* [Translate in Italian for Brackets 1.4](https://github.com/adobe/brackets/pull/11318) by [Denisov21](https://github.com/Denisov21)
* [Fixes for German translation](https://github.com/adobe/brackets/pull/11390) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Travis: Use container-based infrastructure](https://github.com/adobe/brackets/pull/11391) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update descriptions of preferences](https://github.com/adobe/brackets/pull/11388) by [Amin Ullah Khan](https://github.com/sprintr)
* [German translation](https://github.com/adobe/brackets/pull/11400) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Czech translation for 473f8f0](https://github.com/adobe/brackets/pull/11399) by [Pavel Dvořák](https://github.com/dvorapa)
* [Czech translation of Getting Started project](https://github.com/adobe/brackets/pull/11398) by [Pavel Dvořák](https://github.com/dvorapa)
* [Spanish translation update for 1.4](https://github.com/adobe/brackets/pull/11451) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Latest typos in root/strings.js](https://github.com/adobe/brackets/pull/11444) by [Pavel Dvořák](https://github.com/dvorapa)
* [Typo in path to correspond with src/nls/root/strings.js](https://github.com/adobe/brackets/pull/11445) by [Pavel Dvořák](https://github.com/dvorapa)
* [Update translations for zh_CN](https://github.com/adobe/brackets/pull/11140) by [Michael J.](https://github.com/michaeljayt)
* [Czech translation for ed57a2c](https://github.com/adobe/brackets/pull/11454) by [Pavel Dvořák](https://github.com/dvorapa)
* [Update German translation](https://github.com/adobe/brackets/pull/11459) by [Marcel Gerber](https://github.com/MarcelGerber)

#### Pulling source code from Git
_TODO: any brackets-shell updates? which of the below messages are applicable?_

* A new brackets-shell build is _required_ for this sprint. Be sure to rerun `grunt setup` before building.
* Recommended: rebuild or reinstall an updated brackets-shell (no critical updates, but there are bugfixes).
* Rebuilding/updating brackets-shell is _optional_ for this release.
* Rebuilding/updating brackets-shell is _not_ required for this release.
* brackets-shell's Node dependencies have changed. Run `npm install` before rebuilding brackets-shell.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and _then_ `git submodule update --init --recursive` to ensure your local source tree reflects the update.


Bugs fixed in Release 1.4
-------------------------
For details on the bugs addressed, please refer to [closed Release 1.4 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.3%22). Not all fixed bugs will be caught by this search query, however.