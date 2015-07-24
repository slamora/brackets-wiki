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
**TODO**
* [Czech translation for v1.3](https://github.com/adobe/brackets/pull/11022) by [Pavel Dvořák](https://github.com/dvorapa)
* [Don't use null for comparefn, rely on undefined per spec http://www.ecma-international.org/ecma-262/5.1/#sec-15.4.4.11](https://github.com/adobe/brackets/pull/10907) by [David Humphrey](https://github.com/humphd)
* [Add .webp support](https://github.com/adobe/brackets/pull/11015) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Make AnimationUtils react to supported transitionend event](https://github.com/adobe/brackets/pull/9542) by [Triangle717](https://github.com/le717)
* [Update Version to 1.4](https://github.com/adobe/brackets/pull/11053) by [mdewangan](https://github.com/mdewangan)
* [Drastically improve QuickView performance for long lines](https://github.com/adobe/brackets/pull/10882) by [Marcel Gerber](https://github.com/MarcelGerber)
* ["Brackets" translation spelling corrected](https://github.com/adobe/brackets/pull/11042) by [tobimo](https://github.com/tobimo)
* [Fix bug in StringMatch where separator and slash collided](https://github.com/adobe/brackets/pull/10878) by [Marcel Gerber](https://github.com/MarcelGerber)
* [HTTPS health URL: staging has been upgraded to SSL](https://github.com/adobe/brackets/pull/11066) by [abose](https://github.com/abose)
* [Fix #10965: wrap access to sheet.cssRules in try/catch for Firefox SecurityError exception](https://github.com/adobe/brackets/pull/10990) by [David Humphrey](https://github.com/humphd)
* [Update React library to latest 0.13.1](https://github.com/adobe/brackets/pull/10813) by [Martin Zagora](https://github.com/zaggino)
* [Fix two spelling errors.](https://github.com/adobe/brackets/pull/11077) by [Equinox](https://github.com/Equinox)
* [Update CodeMirror](https://github.com/adobe/brackets/pull/11071) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Escape RegExp specials in _createSpecialLineExp](https://github.com/adobe/brackets/pull/11107) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Clean-up servers when closing LiveDevelopment](https://github.com/adobe/brackets/pull/10453) by [Sebastian Salvucci (Intel Corp)](https://github.com/sebaslv)
* [Italian translation review](https://github.com/adobe/brackets/pull/11117) by [Fredev](https://github.com/Fredev)
* [Clean up dependencies in brackets.js](https://github.com/adobe/brackets/pull/10596) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Shim Error.prototype.stack for browsers that lack it.](https://github.com/adobe/brackets/pull/11124) by [David Humphrey](https://github.com/humphd)
* [added Slack channel and re-ordered contact order](https://github.com/adobe/brackets/pull/11148) by [Andrew MacKenzie](https://github.com/mackenza)
* [Rename CodeMirror2 folder to CodeMirror](https://github.com/adobe/brackets/pull/11150) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix #10786 - Allow QuickView image preview for arbitrary URLs](https://github.com/adobe/brackets/pull/10788) by [David Humphrey](https://github.com/humphd)
* [Update CodeMirror](https://github.com/adobe/brackets/pull/11167) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Sample projects: remove trailing whitespace on non-blank lines, fix arrow](https://github.com/adobe/brackets/pull/10926) by [valtlait](https://github.com/valtlait)
* [Fix #10821. Allow page reloads, navigating away and back etc.](https://github.com/adobe/brackets/pull/10822) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [.t is the standard extension of test scripts in Perl](https://github.com/adobe/brackets/pull/11189) by [szabgab](https://github.com/szabgab)
* [Make font settings react to pref changes](https://github.com/adobe/brackets/pull/11190) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix horizontal scrolling of code hints](https://github.com/adobe/brackets/pull/11195) by [Amin Ullah Khan](https://github.com/sprintr)
* [Preferences Code Hints](https://github.com/adobe/brackets/pull/11130) by [Amin Ullah Khan](https://github.com/sprintr)
* [Define all preferences in core](https://github.com/adobe/brackets/pull/11197) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Revert "Fix horizontal scrolling of code hints"](https://github.com/adobe/brackets/pull/11199) by [Amin Ullah Khan](https://github.com/sprintr)
* [Fix errors in src/utils/AppInit.js documentation](https://github.com/adobe/brackets/pull/11225) by [petetnt](https://github.com/petetnt)
* [Enable stylus support](https://github.com/adobe/brackets/pull/11234) by [Amin Ullah Khan](https://github.com/sprintr)
* [Allow JavaScript Code Hints in PHP Files](https://github.com/adobe/brackets/pull/11245) by [Amin Ullah Khan](https://github.com/sprintr)
* [implement sendCommandProgress and progress callback](https://github.com/adobe/brackets-shell/pull/509) by [Martin Zagora](https://github.com/zaggino)
* [Implement progress callback so node domains are able to notify the Brackets before they finish](https://github.com/adobe/brackets/pull/10761) by [Martin Zagora](https://github.com/zaggino)
* [Move descriptions to their preferences](https://github.com/adobe/brackets/pull/11201) by [Amin Ullah Khan](https://github.com/sprintr)
* [Korean translation update](https://github.com/adobe/brackets/pull/11101) by [Yongmin Hong](https://github.com/revi)
* [Remove trailing whitespace in Korean translation](https://github.com/adobe/brackets/pull/11278) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix unittests](https://github.com/adobe/brackets/pull/11275) by [Amin Ullah Khan](https://github.com/sprintr)
* [Docs for PreferencesManager.definePreference](https://github.com/adobe/brackets/pull/11262) by [Amin Ullah Khan](https://github.com/sprintr)
* [Add CodeMirror simple-mode addon](https://github.com/adobe/brackets/pull/11280) by [Amin Ullah Khan](https://github.com/sprintr)
* [Add deprecation warning for convertPreferences()](https://github.com/adobe/brackets/pull/11174) by [Triangle717](https://github.com/le717)
* [Detect keyword 'linear' on lines containing word 'animation' too](https://github.com/adobe/brackets/pull/10989) by [petetnt](https://github.com/petetnt)
* [Update Dutch translation](https://github.com/adobe/brackets/pull/11208) by [coreice](https://github.com/coreice)
* [Don't try to handle protocol-relative URLs from Tern](https://github.com/adobe/brackets/pull/10647) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Allow JavaScript Code Hints only in its context](https://github.com/adobe/brackets/pull/11263) by [Amin Ullah Khan](https://github.com/sprintr)   [Fixed a few typos](https://github.com/adobe/brackets/pull/11302) by [Amin Ullah Khan](https://github.com/sprintr)
* [Update strings.js](https://github.com/adobe/brackets/pull/11312) by [Pavel Dvořák](https://github.com/dvorapa)
* [Use Ctrl+PageUp/PageDown to traverse files in list order](https://github.com/adobe/brackets/pull/11223) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Latest source code pro with Russian / Cyrillic / Greek support. ](https://github.com/adobe/brackets/pull/11301) by [abose](https://github.com/abose)

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