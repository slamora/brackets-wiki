_This is a draft!_ 
-------------------- 
_This page will not be finalized until the end of Release 1.2 -- approximately February 19._ 

What's New in Release 1.2 
------------------------- 

* **Code Editing & Hinting**
    * [Drag & drop to move multiple selected text (Experimental):](https://github.com/adobe/brackets/pull/9584) Select multiple segments of code in the editor (Ctrl+ click and drag over multiple text segments). You can start a drag gesture from any of the selected segment and drop it anywhere on the editor to move the selected text.  You can try out this feature by setting “dragDropText” preference to true. ([Marcel Gerber](https://github.com/MarcelGerber))
    * [SVG code hints: ](https://github.com/adobe/brackets/pull/10294)Brackets now supports code hints for SVG files. (Note that SVG code hints don’t work with SVG code mixed in HTML files).([Amin Ullah Khan](https://github.com/sprintr))
    * [Dart language mode support:](https://github.com/adobe/brackets/pull/10308).([Marcel Gerber](https://github.com/MarcelGerber))
    * [Code hints are now provided for color keywords](https://github.com/adobe/brackets/pull/10410) along with its color swatch. ([Marcel Gerber](https://github.com/MarcelGerber))
* **UI Appearance**
    * [Windows High DPI support:](https://github.com/adobe/brackets-shell/pull/502) Brackets in now HiDPI ready on devices running Windows 7 or higher. Text and images would be rendered crisper in win 8.1+ devices when OS scaling is applied. (Brackets is not optimized for per monitor DPI settings in a multimonitor setup).
* **Live Preview**
    * [Experimental Live Preview:](https://github.com/adobe/brackets/pull/10285)  Experimental multi-browser live preview can be enabled/disabled Under “File -> Enable Experimental Live Preview” menu (This does not currently support pages that use your own [custom server](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#lp-custom-server)). ([Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai))
* **Bug Fixes**
    * [Multi-monitor window positioning](https://github.com/adobe/brackets-shell/pull/498): Brackets no longer starts up positioned off screen after a monitor is unplugged or screen resolution is changed.
* **Localization**
    * Translation updates for: Chinese (Simplified), Czech, Finnish , French, German, Serbian , Romanian. ([Hanhua Hong](https://github.com/mistyhua), [Pavel Dvořák](https://github.com/dvorapa), [valtlait](https://github.com/valtlait), [Marcel Gerber](https://github.com/MarcelGerber), [Goran Vasić](https://github.com/goranvasic), [Micleușanu Nicu](https://github.com/micnic))

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.1...release-1.2#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.1...release-1.2#commits_bucket) 

_Linux builds for 1.2 will be delayed by 2 weeks._

UI Changes 
---------- 
In prior releases, when multiple files were opened in editor-split view, we used to dim the editor pane that was not in focus. This made it hard to read text in dark themes. We changed this- to dim only the editor pane header; instead of dimming the whole content.


API Changes 
----------- 
**CodeMirror**: Upgraded CM to 4.11

**jQuery**: Upgraded from 2.1.1 to 2.1.3 

**InlineColorEditor**: Replacing InlineColorEditor(color, startBookmark, endBookMark) with InlineColorEditor(color, marker)                                                                              Using TextMarker object for InlineColorEditor rather than bookmark for start and end position.

New/Improved Extensibility APIs 
------------------------------- 
LiveDevMultiBrowser.close() now returns a resolved promise for API compatibility.


Known Issues 
------------ 
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text. 
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead. 


Community contributions to Brackets 
----------------------------------- 
* [SVG Code Hints (#10084)](https://github.com/adobe/brackets/pull/10294) by [Amin Ullah Khan](https://github.com/sprintr)
* [Implement CM Drag & Drop](https://github.com/adobe/brackets/pull/9584) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add Dart mode](https://github.com/adobe/brackets/pull/10308) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Improve TokenUtils performance through caching](https://github.com/adobe/brackets/pull/9964) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Hint color names in CSS Code Hints](https://github.com/adobe/brackets/pull/10410) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update 'addBinding' documentation in KeyBindingManager.](https://github.com/adobe/brackets/pull/10225) by [Martin Zagora](https://github.com/zaggino)
* [Don't throw an exception if currentTheme is undefined](https://github.com/adobe/brackets/pull/10236) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Handle undefined theme the right way](https://github.com/adobe/brackets/pull/10243) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update URL to Jasmine (Fixes #10259)](https://github.com/adobe/brackets/pull/10260) by [Steffen Bruchmann](https://github.com/sbruchmann)
* [Update index.html](https://github.com/adobe/brackets/pull/10296) by [Pooya Parsa Dadashi](https://github.com/datamweb)
* [Update Copyright year to 2015](https://github.com/adobe/brackets/pull/10295) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add -lpthread to LIBS (libraries).](https://github.com/adobe/brackets-shell/pull/497) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Support initialDirectory and title in Linux' showOpenDialog](https://github.com/adobe/brackets-shell/pull/496) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Send the error over to the callback if decompress-zip fails](https://github.com/adobe/brackets/pull/10343) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix #10142. LiveDev2: Firefox highlight does not have fade-away effect.](https://github.com/adobe/brackets/pull/10151) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Live Preview MultiBrowser improvements (#10206, #10239)](https://github.com/adobe/brackets/pull/10285) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Highlight matched hints](https://github.com/adobe/brackets/pull/10389) by [Amin Ullah Khan](https://github.com/sprintr)
* [Use upper/lowercase colors in Inline Color Editor](https://github.com/adobe/brackets/pull/9596) by [Triangle717](https://github.com/le717)
* [Put an array of all the color names in ColorUtils](https://github.com/adobe/brackets/pull/10303) by [Marcel Gerber](https://github.com/MarcelGerber)
* [SVG code hints cleanup](https://github.com/adobe/brackets/pull/10403) by [Amin Ullah Khan](https://github.com/sprintr)
* [Romanian locale update](https://github.com/adobe/brackets/pull/10312) by [Micleusanu Nicu](https://github.com/micnic)
* [Multiple ColorEditor fixes](https://github.com/adobe/brackets/pull/10401) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Smarter line clipping in Find in Files](https://github.com/adobe/brackets/pull/9743) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Show colored swatch in CSS Code Hints](https://github.com/adobe/brackets/pull/10425) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix memory leak in CSS & SVG Code Hints](https://github.com/adobe/brackets/pull/10463) by [Marcel Gerber](https://github.com/MarcelGerber)
* [For #5852: Clicking an Extension Manager tab while still loading will show it](https://github.com/adobe/brackets/pull/9594) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Change font weight of menu items to 400 instead of 200, improves readability](https://github.com/adobe/brackets/pull/9829) by [Mark Simulacrum](https://github.com/Mark-Simulacrum)
* [Fix for ImageView bug 5960 & 10180](https://github.com/adobe/brackets/pull/10514) by [EJanuszewski](https://github.com/EJanuszewski)
* [Get rid of deprecation warnings in LiveDevMultiBrowser](https://github.com/adobe/brackets/pull/10500) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Update translation.](https://github.com/adobe/brackets/pull/10525) by [mistyhua](https://github.com/mistyhua)
* [Update jQuery -> 2.1.3](https://github.com/adobe/brackets/pull/10519) by [Marcel Gerber](https://github.com/MarcelGerber)
* [German Translation](https://github.com/adobe/brackets/pull/10528) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add missing attribute](https://github.com/adobe/brackets/pull/10499) by [Amin Ullah Khan](https://github.com/sprintr)
* [Fixed typos in MainViewFactory docs](https://github.com/adobe/brackets/pull/10546) by [Amin Ullah Khan](https://github.com/sprintr)
* [Fix memory leak in ExtensionManagerViewModel](https://github.com/adobe/brackets/pull/10551) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update for Serbian translation](https://github.com/adobe/brackets/pull/10350) by [goranvasic](https://github.com/goranvasic)
* [Finnish translation for 1.2](https://github.com/adobe/brackets/pull/10480) by [valtlait](https://github.com/valtlait)
* [Czech translation for v1.0](https://github.com/adobe/brackets/pull/10503) by [Pavel Dvořák](https://github.com/dvorapa)
* [FR and JP translation fix for Getting Started index.html file](https://github.com/adobe/brackets/pull/10572) by [pantkowiak](https://github.com/pantkowiak)
* [Czech translation for v1.1](https://github.com/adobe/brackets/pull/10577) by [Pavel Dvořák](https://github.com/dvorapa)

#### Pulling source code from Git 
* Recommended: rebuild or reinstall an updated brackets-shell (no critical updates, but there are significant bugfixes).
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date. 


Bugs fixed in Release 1.2 
------------------------- 
For details on the bugs addressed, please refer to [closed Release 1.2 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.2%22). Not all fixed bugs will be caught by this search query, however.