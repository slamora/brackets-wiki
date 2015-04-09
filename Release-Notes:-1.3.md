_This is a draft!_
--------------------
_This page will not be finalized until the end of Release 1.3 -- approximately April 15._

What's New in Release 1.3
-------------------------
* _**TODO: draft**_
* **Code Editing**
   * **[Expand/collapse blocks of code (aka code folding)](https://github.com/adobe/brackets/pull/10792):** Click the indicators next to the line numbers, or use the shortcuts in the View menu.
* **Brackets Health Report**
    * [Anonymous data to help improve Brackets](http://blog.brackets.io/2015/03/27/introducing-brackets-health-report/): You can preview the data that will be sent, and opt-out if desired. We've gone to great lengths to protect your privacy and maintain transparency about what data is sent - please read our [blog post](http://blog.brackets.io/2015/03/27/introducing-brackets-health-report/) for details.
* **Search**
    * [Quick Open: many small bug fixes](https://github.com/adobe/brackets/pull/7227)
* **Command Line Integration**
   * [Command Line tool integration on Windows and Mac](https://github.com/adobe/brackets/wiki.Command-Line-Arguments): You can now open files and folders, in Brackets just by typing `Brackets myFile.txt` or `Brackets myFolder` from command line. Also on Windows, you can launch Brackets from *Run Command* or open a file or a folder right from the explorer just by choosing "Open With Brackets" from it's context menu.

* **Localization**
   * Translation updates for: _(TODO - updated locales in alphabetical order)_


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.2...release-1.3#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.2...release-1.3#commits_bucket)


UI Changes
----------
**Quick Open** - The first item is automatically highlighted, making it more clear that you can press Enter immediately to jump to the top item (Down arrow not required). Pressing the Down arrow once highlights the 2nd item in the result (previously required two presses). Quick Find Definition no longer changes the scroll position before you start typing.

**Command Line Tool (Mac only)** - The new menu, accessible from *Debug->Add Brackets Command Line* menu, installs command line tool at `/usr/local/bin` required for launching Brackets from command line. This menu entry is present only on MAC.

API Changes
-----------
**TODO**

New/Improved Extensibility APIs
-------------------------------
**TODO**


Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* _**Special thanks to [Patrick Oladimeji](https://github.com/thehogfather)** for his major effort contributing the [code folding feature](https://github.com/adobe/brackets/pull/10792)._

**TODO**
*  [integration of code-folding extension into brackets](https://github.com/adobe/brackets/pull/10792) by [Patrick Oladimeji](https://github.com/thehogfather)
*  [Use CMs electric chars](https://github.com/adobe/brackets/pull/9387) by [Marcel Gerber](https://github.com/MarcelGerber)
*  [Use cm.findWordAt() in selectWordAt()](https://github.com/adobe/brackets/pull/9001) by [Marcel Gerber](https://github.com/MarcelGerber)
*  [Disable Spell Checking](https://github.com/adobe/brackets/pull/10321) by [Marcel Gerber](https://github.com/MarcelGerber)
*  [Render file tree without a delay on scroll to fix selection extension issues](https://github.com/adobe/brackets/pull/10689) by [Marcel Gerber](https://github.com/MarcelGerber)
*  [Exclude more unneeded files in dist builds](https://github.com/adobe/brackets/pull/10219) by [Marcel Gerber](https://github.com/MarcelGerber)
*  [Change 'split selection into lines' shortcut on Linux](https://github.com/adobe/brackets/pull/10742) by [Projjol](https://github.com/Projjol)
*  [Add @sourceFontFamily LESS variable and use instead of SourceCodePro font-family](https://github.com/adobe/brackets/pull/10727) by [David Humphrey](https://github.com/humphd)
*  [Remove some circular dependencies](https://github.com/adobe/brackets/pull/10641) by [Triangle717](https://github.com/le717)
*  [FR typo fix](https://github.com/adobe/brackets/pull/10625) by [pantkowiak](https://github.com/pantkowiak)
*  [Fix #10229. Add executable bit to Linux launcher.](https://github.com/adobe/brackets/pull/10267) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
*  [[#10228] Cmd+Delete now triggers Don't Save in Save Changes dialog](https://github.com/adobe/brackets/pull/10459) by [lucaslouca](https://github.com/lucaslouca)
*  [Fix Korean translation of split-view menu items](https://github.com/adobe/brackets/pull/10477) by [mixed](https://github.com/mixed)
*  [Update README.md](https://github.com/adobe/brackets/pull/10672) by [coliff](https://github.com/coliff)
*  [Update README.md](https://github.com/adobe/brackets/pull/10690) by [coliff](https://github.com/coliff)
*  [#10574  Adjust file tree click behavior, don't collapse parent folder](https://github.com/adobe/brackets/pull/10652) by [mellolikejello](https://github.com/mellolikejello)
*  [Fixed Bug #10632](https://github.com/adobe/brackets/pull/10694) by [PlanetVaster](https://github.com/PlanetVaster)
*  [Don't cut off parts of the new name when renaming](https://github.com/adobe/brackets/pull/10648) by [Marcel Gerber](https://github.com/MarcelGerber)
*  [Don't wrap file names in working set](https://github.com/adobe/brackets/pull/10709) by [Marcel Gerber](https://github.com/MarcelGerber)
*  [[#10166] Highlight 'text/ng-template' (Angular) in script tags as HTML](https://github.com/adobe/brackets/pull/10666) by [munxtwo](https://github.com/munxtwo)
*  [Make LiveDevMultiBrowser's Launcher instance configurable via public set Launcher() method](https://github.com/adobe/brackets/pull/10558) by [David Humphrey](https://github.com/humphd)
*  [std::auto_ptr will leak for array, using std::vector instead.](https://github.com/adobe/brackets-shell/pull/504) by [edwin0cheng](https://github.com/edwin0cheng)
*  [Update strings.js for HR(croatian)](https://github.com/adobe/brackets/pull/10736) by [Kruno H](https://github.com/diomed)
*  [Increase timeout for deleting temp directory](https://github.com/adobe/brackets/pull/10758) by [mdewangan](https://github.com/mdewangan)
*  [Add pt-br translate (update)](https://github.com/adobe/brackets/pull/10771) by [Henrique Aparecido Lavezzo](https://github.com/Rynaro)
*  [Indonesian translation updates](https://github.com/adobe/brackets/pull/10713) by [Resi Respati](https://github.com/resir014)
*  [Brackets logo is now perfectly centered in all translations](https://github.com/adobe/brackets/pull/10797) by [Pavel Dvořák](https://github.com/dvorapa)
*  [Brackets logo is now perfectly centered](https://github.com/adobe/brackets/pull/10434) by [electricduck](https://github.com/electricduck)
*  [Extended {APP_NAME} in root/strings.js](https://github.com/adobe/brackets/pull/10800) by [Pavel Dvořák](https://github.com/dvorapa)
*  [Updated Indonesian translation for Getting Started page](https://github.com/adobe/brackets/pull/10793) by [Resi Respati](https://github.com/resir014)
*  [Wordsmithing on Health Data strings](https://github.com/adobe/brackets/pull/10833) by [Marcel Gerber](https://github.com/MarcelGerber)
*  [Making External changes Dialog Box clearer](https://github.com/adobe/brackets/pull/10831) by [rroshan1](https://github.com/rroshan1)

#### Pulling source code from Git
_TODO: any brackets-shell updates? which of the below messages are applicable?_

* A new brackets-shell build is _required_ for this sprint. Be sure to rerun `grunt setup` before building.
* Recommended: rebuild or reinstall an updated brackets-shell (no critical updates, but there are bugfixes).
* Rebuilding/updating brackets-shell is _optional_ for this release.
* Rebuilding/updating brackets-shell is _not_ required for this release.
* brackets-shell's Node dependencies have changed. Run `npm install` before rebuilding brackets-shell.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and _then_ `git submodule update --init --recursive` to ensure your local source tree reflects the update.


Bugs fixed in Release 1.3
-------------------------
For details on the bugs addressed, please refer to [closed Release 1.3 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.3%22). Not all fixed bugs will be caught by this search query, however.