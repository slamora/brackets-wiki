**_Note: Linux builds of Brackets 1.2 will be delayed by 2 weeks._**

What's New in Release 1.2 
-------------------------

* **Code Editing**
    * [Drag & drop to move selected text](https://github.com/adobe/brackets/pull/9584): After enabling the `dragDropText` preference, drag and drop any selected text to move to another location. Even works with [multiple selections](https://github.com/adobe/brackets/wiki/Working-with-Multiple-Selections)! (by [Marcel Gerber](https://github.com/MarcelGerber))
    * [CSS code hints for named colors](https://github.com/adobe/brackets/pull/10410), with color swatch preview. (by [Marcel Gerber](https://github.com/MarcelGerber))
    * [SVG code hints](https://github.com/adobe/brackets/pull/10294): For tags, attributes, and named colors. (by [Amin Ullah Khan](https://github.com/sprintr))
    * [Dart syntax highlighting](https://github.com/adobe/brackets/pull/10308) (by [Marcel Gerber](https://github.com/MarcelGerber))
* **Search**
    * [Highlight scrollbar tickmark for the current Find match](https://github.com/adobe/brackets/pull/10413)
* **UI Appearance**
    * [Windows High DPI support](https://github.com/adobe/brackets-shell/pull/502): Brackets automatically renders text, icons, and images more crisply on high-DPI displays (similar to the appearance on Mac Retina screens).
* **Key Bindings**
    * [Use additional named keys in custom key bindings](https://github.com/adobe/brackets/pull/10247): PageUp, PageDown, Home, End, Insert, Delete. Can be used by extensions or in user custom key bindings.
* **Stability**
    * [Windows: Multi-monitor window positioning](https://github.com/adobe/brackets-shell/pull/498): Brackets no longer starts up positioned off screen after a monitor is unplugged or screen resolution is changed.
* **Linux**
    * [Remember last-opened file in File > Open dialog box](https://github.com/adobe/brackets-shell/pull/496): Now matches existing Mac & Windows behavior. (by [Marcel Gerber](https://github.com/MarcelGerber))
* **Localization**
    * Translation updates for: Chinese (Simplified), Czech, Finnish, French, German, Japanese, Romanian, Serbian. (by [Hanhua Hong](https://github.com/mistyhua), [Pavel Dvořák](https://github.com/dvorapa), [valtlait](https://github.com/valtlait), [Marcel Gerber](https://github.com/MarcelGerber), [Goran Vasić](https://github.com/goranvasic), [Micleușanu Nicu](https://github.com/micnic))

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.1...release-1.2#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.1...release-1.2#commits_bucket) 



UI Changes 
---------- 
**Split View** - The editor pane that is inactive is no longer dimmed. To restore the dimming behavior, install the [Zen Pane extension](https://github.com/stowball/brackets-zen-pane).


API Changes 
----------- 
**jQuery** - Upgraded from 2.1.1 to 2.1.3

**Event listeners** - Deprecation warnings occur when extensions use `$().on()/off()` on Brackets core modules and model objects. Call `.on()/off()` directly instead, as introduced in Brackets 1.1. (For DOM elements, continue to use jQuery events).


New/Improved Extensibility APIs 
------------------------------- 
**Colors** - Use `ColorUtils.COLOR_NAMES` for a list of standard CSS/SVG named colors.


Known Issues 
------------ 
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text. 
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead. 


Community contributions to Brackets 
----------------------------------- 
* [SVG Code Hints](https://github.com/adobe/brackets/pull/10294) ([part 2](https://github.com/adobe/brackets/pull/10403), [part 3](https://github.com/adobe/brackets/pull/10499)) by [Amin Ullah Khan](https://github.com/sprintr)
* [New preference: `dragDropText` to enable drag & drop selected text in editor](https://github.com/adobe/brackets/pull/9584) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Code hints for CSS named colors](https://github.com/adobe/brackets/pull/10410), [with color swatch previews](https://github.com/adobe/brackets/pull/10425) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Linux: Remember last-opened file and show proper title in Open/Save pickers](https://github.com/adobe/brackets-shell/pull/496) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Enable Dart syntax highlighting](https://github.com/adobe/brackets/pull/10308) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add menu item to enable experimental Multi-Browser Live Preview](https://github.com/adobe/brackets/pull/10285) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [New preference: `uppercaseColors` to use uppercase color picker hex codes](https://github.com/adobe/brackets/pull/9596) by [Triangle717](https://github.com/le717)
* [Correctly highlight CSS hint matches in dark themes](https://github.com/adobe/brackets/pull/10389) by [Amin Ullah Khan](https://github.com/sprintr)
* [Fix several issues with color picker's text editing](https://github.com/adobe/brackets/pull/10401) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Linux: Darker menu-item font weight to improve readability](https://github.com/adobe/brackets/pull/9829) by [Mark Simulacrum](https://github.com/Mark-Simulacrum)
* [Fix vertical sizing/layout issues when viewing large image files](https://github.com/adobe/brackets/pull/10514) by [Edward Januszewski](https://github.com/EJanuszewski)
* [Ensure Find in Files match is in the visible part of the line in results panel](https://github.com/adobe/brackets/pull/9743) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix clicking an Extension Manager tab while still loading](https://github.com/adobe/brackets/pull/9594) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Multi-Browser Live Preview: fix Live Highlight fade-out on Firefox](https://github.com/adobe/brackets/pull/10151) (note: feature disabled by default) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Improve TokenUtils performance via caching](https://github.com/adobe/brackets/pull/9964) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add `ColorUtils.COLOR_NAMES` array](https://github.com/adobe/brackets/pull/10303) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update jQuery 2.1.1 to 2.1.3](https://github.com/adobe/brackets/pull/10519) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Improved error reporting when unzipping extensions fails](https://github.com/adobe/brackets/pull/10343) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix memory leak in CSS & SVG code hints](https://github.com/adobe/brackets/pull/10463) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix memory leak when opening/closing Extension Manager](https://github.com/adobe/brackets/pull/10551) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix localization marker visible on Getting Started page](https://github.com/adobe/brackets/pull/10296) by [Pooya Parsa Dadashi](https://github.com/datamweb)
* [Fix error logged after uninstalling/disabling the currently active theme](https://github.com/adobe/brackets/pull/10236) ([part 2](https://github.com/adobe/brackets/pull/10243)) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix Linux crash in new CEF branch](https://github.com/adobe/brackets-shell/pull/497) (not released yet) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Fix deprecation warnings in Multi-Browser Live Preview](https://github.com/adobe/brackets/pull/10500) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Chinese (Simplified) translation update](https://github.com/adobe/brackets/pull/10525) by [Hanhua Hong](https://github.com/mistyhua)
* [Czech translation update](https://github.com/adobe/brackets/pull/10503) ([part 2](https://github.com/adobe/brackets/pull/10577)) by [Pavel Dvořák](https://github.com/dvorapa)
* [Finnish translation update](https://github.com/adobe/brackets/pull/10480) by [valtlait](https://github.com/valtlait)
* [German translation update](https://github.com/adobe/brackets/pull/10528) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Romanian translation update](https://github.com/adobe/brackets/pull/10312) by [Micleusanu Nicu](https://github.com/micnic)
* [Serbian translation update](https://github.com/adobe/brackets/pull/10350) by [Goran Vasić](https://github.com/goranvasic)
* [Update KeyBindingManager docs](https://github.com/adobe/brackets/pull/10225) by [Martin Zagora](https://github.com/zaggino)
* [Fixed typos in MainViewFactory docs](https://github.com/adobe/brackets/pull/10546) by [Amin Ullah Khan](https://github.com/sprintr)
* [Update Copyright year to 2015](https://github.com/adobe/brackets/pull/10295) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update URL to Jasmine (Fixes #10259)](https://github.com/adobe/brackets/pull/10260) by [Steffen Bruchmann](https://github.com/sbruchmann)

#### Pulling source code from Git 
* Highly recommended: rebuild or reinstall an updated brackets-shell (no critical updates, but there are significant bug fixes and new features).
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date. 


Bugs fixed in Release 1.2 
------------------------- 
For details on the bugs addressed, please refer to [closed Release 1.2 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.2%22). Not all fixed bugs will be caught by this search query, however.