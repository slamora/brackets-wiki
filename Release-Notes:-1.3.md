_This is a draft!_
--------------------
_This page will not be finalized until the end of Release 1.3 -- approximately April 23._

What's New in Release 1.3
-------------------------
* **Code Folding**
    * **[Expand/collapse blocks of code (code folding)](https://github.com/adobe/brackets/pull/10792):** Click the indicators next to the line numbers, or use the shortcuts in the View menu.
* **Command Line Integration**
    * [Launch Brackets from command line](https://github.com/adobe/brackets/wiki/Command-Line-Arguments): On Windows & Mac, you can open files and folders in Brackets by typing `brackets myFile.txt` or `brackets myFolder` on the command line. To enable this optional feature:
        - Mac: choose _File > Install Command Line Shortcut_
        - Windows: when installing, leave _Add "brackets" launcher to PATH for command line use_ selected
    * On Windows, you can also opt to make Brackets accessible from the Explorer context menu for all files and folders - leave _Add "Open with Brackets" to Explorer context menus for all files and folders_ selected when installing
* **Brackets Health Report**
    * [Anonymous data to help improve Brackets](https://github.com/adobe/brackets/wiki/Health-Data): You can preview the data that will be sent, and opt-out if desired. We've gone to great lengths to protect your privacy and maintain transparency about what data is sent - please read our [blog post](http://blog.brackets.io/2015/03/27/introducing-brackets-health-report/) for details.
* **Stability**
    * [Windows: Mouse wheel scrolls at normal speed](https://github.com/adobe/brackets/pull/10681): Restores scrolling speed to the behavior of Brackets 1.0.
    * [Quick Open: many small bug fixes](https://github.com/adobe/brackets/pull/7227)
* **Code Editing**
    * [Highlight 'text/ng-template' Angular templates as plain HTML](https://github.com/adobe/brackets/pull/10666)
    * [Better auto-unindent when ending a block](https://github.com/adobe/brackets/pull/9387): Works with multiple cursors, better indentation in JS `switch` statements, etc.
* **Localization**
    * Translation updates for: Brazilian, Croatian, Farsi, Finnish, French, German, Indonesian, Italian, Japanese, Korean


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.2...release-1.3#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.2...release-1.3#commits_bucket)


UI Changes
----------
**Code Folding** - Expand/collapse indicators now appear in the gutter next to the line numbers. To remove these from the UI, [change your preferences](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#preferences) to set `code-folding.enabled` to false.

**Find menu** - The menu items _Find in Selected File/Folder_ and _Replace in Selected File/Folder_ were removed to avoid confusion. To run Find/Replace in Files on a specific folder or single file, right click the item in the project tree and choose _Find in..._ or _Replace in..._

**Unsaved changes** (Mac only) - Cmd-Delete (Backspace) now chooses "Don't Save," as is common in other Mac apps.

**Split Selection into Lines** (Linux only) - Keyboard shortcut is now Ctrl-Shift-L (instead of Ctrl-Alt-L, which conflicted with the OS shortcut for lock/sleep).

**Quick Open** - The first item is automatically highlighted, making it more clear that you can press Enter immediately to jump to the top item (Down arrow not required). Pressing the Down arrow once highlights the 2nd item in the result (previously required two presses). Quick Find Definition no longer changes the scroll position before you start typing.

API Changes
-----------
**Themes** - Themes should include appropriate color styles for the new code folding UI: `.CodeMirror-foldgutter-open:after`, `.CodeMirror-foldgutter-folded:after`, `.CodeMirror.over-gutter .CodeMirror-foldgutter-open:after`, `.CodeMirror-activeline .CodeMirror-foldgutter-open:after`, and `.CodeMirror-foldmarker`. All these elements appear overlaid on top of the theme's editor (or editor gutter) background color.

New/Improved Extensibility APIs
-------------------------------
No new extensibility APIs added this release.


Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* _**Special thanks to [Patrick Oladimeji](https://github.com/thehogfather)** for his major effort contributing the code folding feature_
    * Pull requests: [1](https://github.com/adobe/brackets/pull/10792) [2](https://github.com/adobe/brackets/pull/10850) [3](https://github.com/adobe/brackets/pull/10914)
* [Restore Windows mouse wheel scrolling to Brackets 1.0 speed](https://github.com/adobe/brackets/pull/10930) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Reduce install size by ~1.2MB - exclude unneeded files](https://github.com/adobe/brackets/pull/10219) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Highlight Angular templates ('text/ng-template' script tags) as plain HTML](https://github.com/adobe/brackets/pull/10666) by [munxtwo](https://github.com/munxtwo)
* [Haskell syntax highlighting](https://github.com/adobe/brackets/pull/10844) by [Venessa Johansen Barrera](https://github.com/VenessaJohansenBarrera)
* [Cmd-Delete to trigger Don't Save in Save Changes dialog](https://github.com/adobe/brackets/pull/10459) by [Lucas Louca](https://github.com/lucaslouca)
* [Change Linux 'Split Selection into Lines' shortcut to avoid conflict with OS 'Lock' shortcut](https://github.com/adobe/brackets/pull/10742) by [Projjol Banerji](https://github.com/Projjol)
* [Disable spell-checking red squiggles in UI](https://github.com/adobe/brackets/pull/10321) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix multiple bugs with unindenting at end of blocks](https://github.com/adobe/brackets/pull/9387) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix: Don't cut off ending when typing quickly during renaming](https://github.com/adobe/brackets/pull/10648) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix lag in file tree selection highlight while scrolling](https://github.com/adobe/brackets/pull/10689) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Show error message for invalid .brackets.json project preferences files](https://github.com/adobe/brackets/pull/10819) by [Rakesh Roshan](https://github.com/rroshan1)
* [Make right-click selection agree with double-click](https://github.com/adobe/brackets/pull/9001) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix: Don't line-wrap file names in working set](https://github.com/adobe/brackets/pull/10709) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix memory leak when launching Live Preview](https://github.com/adobe/brackets-shell/pull/504) by [edwin0cheng](https://github.com/edwin0cheng)
* [Fix: Multi-browser Live Preview didn't launch in dev builds](https://github.com/adobe/brackets/pull/10267) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Make Multi-browser Live Preview's browser launching easily overridable in forks](https://github.com/adobe/brackets/pull/10558) by [David Humphrey](https://github.com/humphd)
* [Cleanup: Remove some circular dependencies](https://github.com/adobe/brackets/pull/10641) by [Triangle717](https://github.com/le717)
* [Cleanup: Use LESS variables for font name lists in more places](https://github.com/adobe/brackets/pull/10727) by [David Humphrey](https://github.com/humphd)
* [Wordsmithing on Health Data messages](https://github.com/adobe/brackets/pull/10833) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Clarify strings in External Changes dialog box](https://github.com/adobe/brackets/pull/10831) by [Rakesh Roshan](https://github.com/rroshan1)
* [Fix typos in Command Line Shortcut messages](https://github.com/adobe/brackets/pull/10875) by [Denisov21](https://github.com/Denisov21)
* [Cleanup: Use `APP_NAME` placeholder instead of "Brackets" in more strings](https://github.com/adobe/brackets/pull/10800) by [Pavel Dvořák](https://github.com/dvorapa)
* [Brazilian Portuguese translation update](https://github.com/adobe/brackets/pull/10771) by [Henrique Aparecido Lavezzo](https://github.com/Rynaro)
* [Croatian translation update](https://github.com/adobe/brackets/pull/10736) by [Kruno H](https://github.com/diomed)
* [Farsi translation update](https://github.com/adobe/brackets/pull/10254) by [Mohammad Reza Mahmoudi](https://github.com/Rezaaa)
* [Finnish translation update](https://github.com/adobe/brackets/pull/10892) by [valtlait](https://github.com/valtlait)
* [German translation update](https://github.com/adobe/brackets/pull/10925) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Indonesian translation update](https://github.com/adobe/brackets/pull/10713) ([part 2](https://github.com/adobe/brackets/pull/10793)) by [Resi Respati](https://github.com/resir014)
* [Italian translation update](https://github.com/adobe/brackets/pull/10874) by [Denisov21](https://github.com/Denisov21)
* [Korean translation fix](https://github.com/adobe/brackets/pull/10477) by [Yong Woo Jeon](https://github.com/mixed)
* [Tweak ASCII-art Brackets logo in Getting Started content](https://github.com/adobe/brackets/pull/10797) by [electricduck](https://github.com/electricduck) and [Pavel Dvořák](https://github.com/dvorapa)
* [Fix legacy "X-UA-Compatible" in Getting Started content](https://github.com/adobe/brackets/pull/10694) by [PlanetVaster](https://github.com/PlanetVaster)
* [Update stale version number in README](https://github.com/adobe/brackets/pull/10690) by [Christian Coliff](https://github.com/coliff)

#### Pulling source code from Git
* Recommended: rebuild or reinstall an updated brackets-shell (no critical updates, but there are bugfixes).
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 1.3
-------------------------
For details on the bugs addressed, please refer to [closed Release 1.3 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.3%22). Not all fixed bugs will be caught by this search query, however.