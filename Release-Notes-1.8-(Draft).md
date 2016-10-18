_This is a draft!_
--------------------
_This page will not be finalized until the end of Release 1.8 -- approximately November 4._

What's New in Release 1.8
-------------------------
* _**TODO: draft**_
* **TODO: category heading**
   * _TODO: feature list_
* **Localization**
   * Translation updates for: _(TODO - updated locales in alphabetical order)_


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.7...release-1.8#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.7...release-1.8#commits_bucket)


UI Changes
----------
**TODO** - list notable changes in _existing_ features ...or:
No major changes to existing features.


API Changes
-----------
**TODO** - list breaking API changes

New/Improved Extensibility APIs
-------------------------------
**TODO** - list API expansions


Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Moved client_switches files under a new common folder](https://github.com/adobe/brackets-shell/pull/559) by [ficristo](https://github.com/ficristo)
* [VS2015: add link libraries](https://github.com/adobe/brackets-shell/pull/552) by [ficristo](https://github.com/ficristo)
* [Removed string_util since is unused](https://github.com/adobe/brackets-shell/pull/556) by [ficristo](https://github.com/ficristo)
* [Replaced util macros in favour of cef_helpers|cef_logging ones](https://github.com/adobe/brackets-shell/pull/557) by [ficristo](https://github.com/ficristo)
* [VS2015: fix some warnings](https://github.com/adobe/brackets-shell/pull/549) by [ficristo](https://github.com/ficristo)
* [Updated gyp](https://github.com/adobe/brackets-shell/pull/546) by [ficristo](https://github.com/ficristo)
* [Update strings.js](https://github.com/adobe/brackets/pull/12523) by [Kruno H](https://github.com/diomed)
* [Pull latest changes from release branch](https://github.com/adobe/brackets/pull/12563) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Replace the URL in URLCodeHints even if it is the current content of …](https://github.com/adobe/brackets/pull/11284) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Close CSS Code Hints after inputting property value](https://github.com/adobe/brackets/pull/10524) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add new pref: lineWiseCopyCut](https://github.com/adobe/brackets/pull/11706) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Show "Confirm Delete" dialog for files, too](https://github.com/adobe/brackets/pull/10258) by [Marcel Gerber](https://github.com/MarcelGerber)
* [fix some pt-br words](https://github.com/adobe/brackets/pull/12582) by [jorgelealbr](https://github.com/jorgelealbr)
* [Fix failing tests](https://github.com/adobe/brackets/pull/12615) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update CodeMirror](https://github.com/adobe/brackets/pull/12613) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Improve search tick mark positioning](https://github.com/adobe/brackets/pull/11293) by [Marcel Gerber](https://github.com/MarcelGerber)
* [CSS: remove unneeded prefixes, add standard properties, remove `px` from 0s](https://github.com/adobe/brackets/pull/12648) by [valtlai](https://github.com/valtlai)
* [Compress all .png files using Zopfli](https://github.com/adobe/brackets/pull/12628) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Addresses #12456 ](https://github.com/adobe/brackets/pull/12521) by [Patrick Oladimeji](https://github.com/thehogfather)
* [Use opn instead of open.](https://github.com/adobe/brackets/pull/12641) by [ficristo](https://github.com/ficristo)
* [Change path of xdg-open to match opn (for linux-1547)](https://github.com/adobe/brackets-shell/pull/564) by [ficristo](https://github.com/ficristo)
* [Change path of xdg-open to match opn](https://github.com/adobe/brackets-shell/pull/563) by [ficristo](https://github.com/ficristo)
* [Move most inline jslint directives to config files](https://github.com/adobe/brackets/pull/12661) by [ficristo](https://github.com/ficristo)
* [Avoid the edit icon to move in the file exclusion set](https://github.com/adobe/brackets/pull/12671) by [ficristo](https://github.com/ficristo)
* [Fix crawlComplete exception when running tests](https://github.com/adobe/brackets/pull/12659) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update compatibility.manifest for Windows 10](https://github.com/adobe/brackets-shell/pull/566) by [ficristo](https://github.com/ficristo)
* [Add another assertion to be on the safe side](https://github.com/adobe/brackets/pull/12676) by [Amin Ullah Khan](https://github.com/sprintr)
* [Fix to prevent moving files by renaming files using FileTreeView](https://github.com/adobe/brackets/pull/11862) by [tallandroid](https://github.com/tallandroid)
* [Remove code-padding from Codemirror pre's. Fixes #12630](https://github.com/adobe/brackets/pull/12635) by [petetnt](https://github.com/petetnt)
* [Rework how file selection is shown](https://github.com/adobe/brackets/pull/12636) by [Marcel Gerber](https://github.com/MarcelGerber)
* [get latest codemirror fixes](https://github.com/adobe/brackets/pull/12679) by [Martin Zagora](https://github.com/zaggino)
* [do nothing when trying to reset with the same content which is already present](https://github.com/adobe/brackets/pull/12681) by [Martin Zagora](https://github.com/zaggino)
* [Removed unnecessary prefixed property](https://github.com/adobe/brackets/pull/12533) by [AllThingsSmitty](https://github.com/AllThingsSmitty)
* [Fix the grayscale animation in our samples](https://github.com/adobe/brackets/pull/12683) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update .gitignore](https://github.com/adobe/brackets-shell/pull/569) by [ficristo](https://github.com/ficristo)
* [Rework a bit appshell gyp files](https://github.com/adobe/brackets-shell/pull/561) by [ficristo](https://github.com/ficristo)
* [Only add padding-left to main tree UL. Fix padding-bottom assignment since React was introduced](https://github.com/adobe/brackets/pull/11717) by [Matt Stow](https://github.com/stowball)
* [Enable "Click Through" for the file tree shadow](https://github.com/adobe/brackets/pull/12685) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Move resource(_util*) under a new browser folder](https://github.com/adobe/brackets-shell/pull/570) by [ficristo](https://github.com/ficristo)
* [Reuse parseQueryInfo defined in FindUtils for the FindReplace feature.](https://github.com/adobe/brackets/pull/12027) by [ficristo](https://github.com/ficristo)
* [Attach editor to LiveCSSDocument](https://github.com/adobe/brackets/pull/10522) by [Sebastian Salvucci (Intel Corp)](https://github.com/sebaslv)
* [Rename CONFIRM_FOLDER_DELETE_TITLE -> CONFIRM_DELETE_TITLE](https://github.com/adobe/brackets/pull/12693) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Remove basePadding from indentwrap hack](https://github.com/adobe/brackets/pull/12110) by [petetnt](https://github.com/petetnt)
* [Simulate indentation of the project tree using some divs](https://github.com/adobe/brackets/pull/12047) by [ficristo](https://github.com/ficristo)
* [Use the global RequireJS config for ExtensionLoader. Fixes #1087](https://github.com/adobe/brackets/pull/12041) by [petetnt](https://github.com/petetnt)
* [Update tests after #12047](https://github.com/adobe/brackets/pull/12700) by [ficristo](https://github.com/ficristo)
* [Fix the bug where errors during installation of a local .zip file wer…](https://github.com/adobe/brackets/pull/12702) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add client_types.h, match gtk gyp inclusion of cefclient](https://github.com/adobe/brackets-shell/pull/571) by [ficristo](https://github.com/ficristo)
* [Make file watching compatible with node version 6.x (runs on 0.10 too)](https://github.com/adobe/brackets/pull/12647) by [Martin Zagora](https://github.com/zaggino)
* [Compress .png files using Zopfli](https://github.com/adobe/brackets-shell/pull/565) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Switched from JSHint to ESLint](https://github.com/adobe/brackets-shell/pull/542) by [ficristo](https://github.com/ficristo)
* [ESLint: enable strict rule](https://github.com/adobe/brackets/pull/12718) by [ficristo](https://github.com/ficristo)
* [Copy npm-shrinkwrap in dist folder and cleanup the file a bit](https://github.com/adobe/brackets/pull/12717) by [ficristo](https://github.com/ficristo)
* [MultiBrowser Live Preview: Fix cases where url()s in CSS weren't resolved correctly](https://github.com/adobe/brackets/pull/12705) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix Inline Editor file name underline](https://github.com/adobe/brackets/pull/12701) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix for issue #8142: "Ctrl + Space" should move to the next entry when the code hint menu is open](https://github.com/adobe/brackets/pull/12251) by [bmax](https://github.com/bmax)
* [Update Tern & Acorn](https://github.com/adobe/brackets/pull/11569) by [Marcel Gerber](https://github.com/MarcelGerber)
* [ESLint: enable no-bitwise rule](https://github.com/adobe/brackets/pull/12719) by [ficristo](https://github.com/ficristo)
* [Fix: Find in Files results don't update to reflect external changes unless file is open](https://github.com/adobe/brackets/pull/11602) by [amrelnaggar](https://github.com/amrelnaggar)
* [Ensures fold-gutter is always inserted after the line-number gutter (if it exists)](https://github.com/adobe/brackets/pull/12673) by [Patrick Oladimeji](https://github.com/thehogfather)
* [release to master](https://github.com/adobe/brackets-shell/pull/573) by [Martin Zagora](https://github.com/zaggino)
* [Revert "Ensures fold-gutter is always inserted after the line-number gutter (if it exists)"](https://github.com/adobe/brackets/pull/12727) by [petetnt](https://github.com/petetnt)
* [fix invalid filename ](https://github.com/adobe/brackets/pull/12732) by [Martin Zagora](https://github.com/zaggino)
* [Rename ecma5 to ecmascript](https://github.com/adobe/brackets/pull/12736) by [ficristo](https://github.com/ficristo)
* [Resequence autosuggestions for CSS  'list-style'](https://github.com/adobe/brackets/pull/12738) by [albell](https://github.com/albell)
* [Monkeypatch ternServer.normalizeFilename so it doesn't break Mac/Linu…](https://github.com/adobe/brackets/pull/12734) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add cut/copy/paste to the context menu](https://github.com/adobe/brackets/pull/12674) by [ficristo](https://github.com/ficristo)
* [Enable paste command](https://github.com/adobe/brackets-shell/pull/567) by [ficristo](https://github.com/ficristo)
* [update shell node version to 6.3.1](https://github.com/adobe/brackets-shell/pull/543) by [Martin Zagora](https://github.com/zaggino)
* [node 6.3.1 for linux](https://github.com/adobe/brackets-shell/pull/574) by [Martin Zagora](https://github.com/zaggino)
* [Support initialDirectory and title in Linux' showOpenDialog](https://github.com/adobe/brackets-shell/pull/575) by [Marcel Gerber](https://github.com/MarcelGerber)
* [update CM before release](https://github.com/adobe/brackets/pull/12753) by [Martin Zagora](https://github.com/zaggino)
* [Travis: test against node 6](https://github.com/adobe/brackets/pull/12755) by [ficristo](https://github.com/ficristo)
* [Add an ISSUE_TEMPLATE](https://github.com/adobe/brackets/pull/12737) by [ficristo](https://github.com/ficristo)
* [Created a Guide for Common Linux Issues](https://github.com/adobe/brackets/pull/12761) by [NathanJPlummer](https://github.com/NathanJPlummer)
* [Fixed leak in menu.js file. Fixes #12765](https://github.com/adobe/brackets/pull/12767) by [daytonallen](https://github.com/daytonallen)
* [set eslints max-len to 120](https://github.com/adobe/brackets/pull/12780) by [Martin Zagora](https://github.com/zaggino)
* [Some cleanup](https://github.com/adobe/brackets-shell/pull/576) by [ficristo](https://github.com/ficristo)
* [Remove define OS_*](https://github.com/adobe/brackets-shell/pull/577) by [ficristo](https://github.com/ficristo)
* [Add missing include and fix path libcef.so](https://github.com/adobe/brackets-shell/pull/578) by [ficristo](https://github.com/ficristo)
* [Adds autofill detail tokens to the autocomplete attribute](https://github.com/adobe/brackets/pull/12721) by [Dandandans](https://github.com/Dandandans)
* [add font-display property](https://github.com/adobe/brackets/pull/12785) by [samuelcarreira](https://github.com/samuelcarreira)
* [Italian translation, correct sintax](https://github.com/adobe/brackets/pull/12800) by [dennybiasiolli](https://github.com/dennybiasiolli)
* [Ignore only toplevel folder and add out folder too](https://github.com/adobe/brackets-shell/pull/580) by [ficristo](https://github.com/ficristo)
* [ Caching $(".modal-body", $dlg) in to its own variable](https://github.com/adobe/brackets/pull/12805) by [mansimarkaur](https://github.com/mansimarkaur)
* [Don't let ESC key bubble in CodeHintList. Fixes #12799](https://github.com/adobe/brackets/pull/12802) by [petetnt](https://github.com/petetnt)
* [Fix MacOS folder icon handling. Fixes #12789](https://github.com/adobe/brackets/pull/12807) by [petetnt](https://github.com/petetnt)
* [Move AppShellExtensionHandler and StContextScope out of client_app](https://github.com/adobe/brackets-shell/pull/581) by [ficristo](https://github.com/ficristo)
* [Move fixupKey where it is used and fix headers inclusion](https://github.com/adobe/brackets-shell/pull/583) by [ficristo](https://github.com/ficristo)
* [Add ARIA Code Hints](https://github.com/adobe/brackets/pull/12471) by [Coder206](https://github.com/Coder206)
* [Import cefclient-2623 code and include it on Linux build only](https://github.com/adobe/brackets-shell/pull/584) by [ficristo](https://github.com/ficristo)
* [Adds code folding support for handlebars template files](https://github.com/adobe/brackets/pull/12675) by [Patrick Oladimeji](https://github.com/thehogfather)
* [Appshell helpers](https://github.com/adobe/brackets-shell/pull/585) by [ficristo](https://github.com/ficristo)
* [Init command_line directly in cefclient_<platform>](https://github.com/adobe/brackets-shell/pull/586) by [ficristo](https://github.com/ficristo)
* [Move more things from cefclient_gtk to appshell_helpers](https://github.com/adobe/brackets-shell/pull/588) by [ficristo](https://github.com/ficristo)
* [Update CSSProperties.json](https://github.com/adobe/brackets/pull/12822) by [Ollie-w](https://github.com/Ollie-w)
* [Dialog returns focus to previous element after closing](https://github.com/adobe/brackets/pull/12824) by [Jerhamre](https://github.com/Jerhamre)

#### Pulling source code from Git
_TODO: any brackets-shell updates? which of the below messages are applicable?_

* A new brackets-shell build is _required_ for this sprint. Be sure to rerun `grunt setup` before building.
* Recommended: rebuild or reinstall an updated brackets-shell (no critical updates, but there are bugfixes).
* Rebuilding/updating brackets-shell is _optional_ for this release.
* Rebuilding/updating brackets-shell is _not_ required for this release.
* brackets-shell's Node dependencies have changed. Run `npm install` before rebuilding brackets-shell.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and _then_ `git submodule update --init --recursive` to ensure your local source tree reflects the update.


Bugs fixed in Release 1.8
-------------------------
For details on the bugs addressed, please refer to [closed Release 1.8 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.8%22). Not all fixed bugs will be caught by this search query, however.