What's New in Release 0.43
--------------------------
* **SCSS/LESS Editing**
    * **[Quick Edit, Quick Find Definition, & Live Preview Highlight support](https://trello.com/c/KoK6kPtp/1190-m-simple-support-for-less-scss-in-key-brackets-features)**: All these features now work the same for SCSS & LESS as they do for plain CSS.
    * Live Preview supports SCSS & LESS editing if you use a third-party "file watcher" to automatically recompile your CSS on save. Brackets notices the changed CSS file and updates the page (without reloading). You can also use a Brackets extension such as [Brackets SASS](https://github.com/jasonsanjose/brackets-sass) or [LESS AutoCompile](https://github.com/jdiehl/brackets-less-autocompile) for this.
* **Themes**
    * [Improved 'dark mode'](https://github.com/adobe/brackets/pull/8731): Dark themes now cause the overall Brackets UI to become darker, not just the editor area.
    * [Separate Extension Manager tab](https://github.com/adobe/brackets/pull/8759): Themes are listed separately in Extension Manager so they're easier to find, and the list of extensions stays cleaner.
* **Search**
    * [Find/Replace: indicate current match index](https://trello.com/c/0GXqt5GF/1089-s-find-next-indicate-current-match-index): The Find/Replace bar now shows "3 of 28" instead of just "28."
* **File Types**
    * [Change file extension -> language/syntax mode mapping](https://github.com/adobe/brackets/pull/8444): After changing a file's language using the status bar switcher, you can save the change for _all_ files with that extension by choosing "Set as Default" from the same dropdown. (This was previously accessible only by manually editing preferences).
* **Live Preview**
    * Support for .xhtml files.
    * Improved reliability when using [iframes](https://github.com/adobe/brackets/pull/8144) or [files not listed in "Working Files"](https://github.com/adobe/brackets/pull/8605).
* **Ongoing Research** (not available yet)
    * [Split view](https://trello.com/c/atD9BEDl/1281-m-splitview-implement-mainviewmanager-code-for-1x2-editors) (early implementation on branch)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-0.42...release-0.43#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-0.42...release-0.43#commits_bucket)


UI Changes
----------
**Keyboard shortcut changes**

* _Replace in Files_ changed to Ctrl-Shift-H (Win) or Cmd-Alt-Shift-F (Mac), to parallel the single-file Replace shortcut better.
* _Show/Hide Sidebar_ changed to Ctrl-Alt-H (Win) or Cmd-Shift-H (Mac).

**Linux: Edit menu** - The _Edit > Cut/Copy/Paste_ menu items are removed on Linux, because they did not work. These menu items will be restored once [a native menu bar](https://trello.com/c/WMB6vtwO/893-linux-native-menus) is implemented. Keyboard shortcuts for cut/copy/paste still work.


API Changes
-----------
**Theme authoring** - Simplified how Find/Replace highlight colors are set. [Read more...](https://github.com/adobe/brackets/wiki/Creating-Themes#theme-styles)

**window.open()** - Only permits 'file://' URLs and 'about:blank', since brackets-shell is not a secure general-purpose web browser. Use `NativeApp.openURLInDefaultBrowser()` to open webpages in the user's regular browser.


Known Issues
------------
* **[#9002](https://github.com/adobe/brackets/issues/9002): Brackets freezes** during Live Preview if you place the cursor on a line within a CSS/SCSS/LESS block comment that contains nothing but "}" with no indent. (Will be fixed in next release).
* **[#8966](https://github.com/adobe/brackets/issues/8966): Inline editor is blank** if your CSS rule contains a vendor-prefixed property that uses an rgb()/rgba()/hsl()/hsla() color value. (Will be fixed in next release).
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Add Themes tab to Extension Manager](https://github.com/adobe/brackets/pull/8759) (+ [part 2](https://github.com/adobe/brackets/pull/8857)) by [Miguel Castillo](https://github.com/MiguelCastillo)
* [Fix Live Preview bugs when page contains iframe](https://github.com/adobe/brackets/pull/8144) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Create Ukrainian translation](https://github.com/adobe/brackets/pull/8881) by [Maks Lyashuk](https://github.com/probil)
* [Polyfill String.trimRight() to support IE10+ in-browser](https://github.com/adobe/brackets/pull/7231) in part by [David Humphrey](https://github.com/humphd)
* [Improve undo batching for inline CSS Bezier/timing editor's changes](https://github.com/adobe/brackets/pull/8589) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix 8572: Dragging in color picker square causes hue to change](https://github.com/adobe/brackets/pull/8588) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix 4920 & 5999: Don't terminate Working Files drag prematurely](https://github.com/adobe/brackets/pull/7820) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Suport generic CodeMirror themes when using `ThemeManager.loadFile()`](https://github.com/adobe/brackets/pull/8673) (+ [part 2](https://github.com/adobe/brackets/pull/8677)) by [Miguel Castillo](https://github.com/MiguelCastillo)
* [Fix 8712: Make title of Themes dialog localizable](https://github.com/adobe/brackets/pull/8715) by [Denisov21](https://github.com/Denisov21)
* [Fix code hint capitalization of meta tag's `name="keywords"`](https://github.com/adobe/brackets/pull/8533) by [Triangle717](https://github.com/le717)
* [Fix spurious focus ring shown on inline editors](https://github.com/adobe/brackets/pull/8663) by [Marcel Gerber](https://github.com/MarcelGerber)
* [When prepopulating Find in Files query from editor selection, only use first line of text (as in Find)](https://github.com/adobe/brackets/pull/8470) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Immediately hide Quick View popover when mouse leaves editor area](https://github.com/adobe/brackets/pull/8740) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add keywords for Ubuntu desktop search](https://github.com/adobe/brackets-shell/pull/461) by [Mark-Simulacrum](https://github.com/Mark-Simulacrum)
* [Add unit tests for `FileUtils.convertToNativePath()`](https://github.com/adobe/brackets/pull/7957) by [chirayu11](https://github.com/chirayu11)
* [Cleanup: better way to strip whitespace in JSLint extension](https://github.com/adobe/brackets/pull/8674) by [Martin Mädler](https://github.com/MartinMa)
* [Cleanup: Optimize 'Getting Started' sample project PNGs](https://github.com/adobe/brackets/pull/8299) by [Jamie Stevens](https://github.com/jamiestevens)
* [Dutch translation for Getting Started](https://github.com/adobe/brackets/pull/8436) by [githrdw](https://github.com/githrdw)
* [Italian translation update](https://github.com/adobe/brackets/pull/8701) (+ [part 2](https://github.com/adobe/brackets/pull/8756)) by [Denisov21](https://github.com/Denisov21)
* [Finnish translation update](https://github.com/adobe/brackets/pull/8819) (+ [part 2](https://github.com/adobe/brackets/pull/8820)) by [valtlait](https://github.com/valtlait)
* [Chinese (Simplified) translation update](https://github.com/adobe/brackets/pull/8687) by [airycanon](https://github.com/airycanon)
* [Brazilian Portuguese translation update](https://github.com/adobe/brackets/pull/8841) by [Brunno Pleffken](https://github.com/brunnopleffken)
* [German translation update](https://github.com/adobe/brackets/pull/8879) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Spanish translation update](https://github.com/adobe/brackets/pull/8893) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Improve localization README.md](https://github.com/adobe/brackets/pull/8438) by [Pei-Tang Huang](https://github.com/tan9)
* [Fix typos in localization README.md](https://github.com/adobe/brackets/pull/8722) by [Denisov21](https://github.com/Denisov21)
* [Fix typo in LocalizationExample README.md](https://github.com/adobe/brackets/pull/8774) by [Denisov21](https://github.com/Denisov21)
* [Documentation cleanup in `StringUtils.truncate()`](https://github.com/adobe/brackets/pull/8707) by [Triangle717](https://github.com/le717)
* [Cleanup: Update documentation/community URLs to canonical version](https://github.com/adobe/brackets/pull/8532) (+ [part 2](https://github.com/adobe/brackets/pull/8606)) by [Eric J](https://github.com/wormeyman)


#### Pulling source code from Git
* Rebuilding/updating brackets-shell is _optional_ for this release (only change is [one](https://github.com/adobe/brackets-shell/pull/450) Mac-only bug fix).
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 0.43
--------------------------
For details on the bugs addressed, please refer to [closed Release 0.43 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+0.43%22). Not all fixed bugs will be caught by this search query, however.