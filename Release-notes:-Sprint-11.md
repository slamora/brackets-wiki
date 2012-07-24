What's New in Sprint 11
-----------------------
Our main goals this sprint were to grow the core feature set (starting on code hinting); continue preparing for the migration to a CEF3-based native app shell; and stay responsive to the fast-growing Brackets community. We're grateful for receiving both feedback as well as an increasing number of bug fixes and other patches from the community. This sprint we merged a number of external pull requests and evaluated many bug reports & feature requests.
    
* **Code Hinting**
    * [Code Completion for HTML Tags](https://trello.com/card/5-code-complete-html-tags/4f90a6d98f77505d7940ce88/283): Basic code hinting for HTML tag names. Appears when you type "<" or Ctrl+Space. This is just an early first step; see the [feature backlog](https://trello.com/board/brackets/4f90a6d98f77505d7940ce88) for future plans.
* **Native Shell**
    * [CEF3 Shell Enhancements](https://trello.com/card/1-cef3-shell-enhancements/4f90a6d98f77505d7940ce88/560): Fix bugs and get most unit tests passing in the [experimental new shell](https://github.com/adobe/brackets-shell/), based on Chromium Embedding Framework 3 (Brackets currently uses CEF 1). We plan to switch over to the new shell sometime within the next couple of sprints.
* **Performance**
    * [Switch project performance improvements] (https://github.com/adobe/brackets/pull/1197)
    * [Evaluate editor performance enhancements](https://trello.com/card/1-evaluate-scrolling-performance-enhancements/4f90a6d98f77505d7940ce88/555): Although not merged in yet, we're evaluating [some](https://github.com/adobe/brackets/pull/1007) [changes](https://github.com/adobe/CodeMirror2/pull/60) that may significantly improve scrolling and typing responsiveness.
* **Other Enhancements**
    * [Esc key shortcut](https://trello.com/card/1-keyboard-controls-for-quick-editors/4f90a6d98f77505d7940ce88/252): Pressing Esc while a menu of code hint popup is open closes it. While the cursor is inside an inline editor, Esc closes it. While the cursor is in a regular full-size editor, Esc closes _all_ inline editors that are open in it.

UI Changes
----------
no major changes to existing UI

API Changes
-----------
* FileViewController - now dispatches "fileViewFocusChange" when the selection focus changes between the project tree and the working set, but the current document didn't change
* ProjectManager - loadProject() is now private. Use openProject() instead: it now accepts an optional project path. If no path is specified, the Folder Chooser dialog is still invoked as before. If a project path is specified, then the current project is closed and the new project is opened.

New Extensibility APIs / API Enhancements
-----------------------------------------
* Menus - addMenuItem() now supports inserting new menu items relative to a general "menu group" rather than next to a specific existing menu item. See the Menus.MenuSection constants.

Known Issues
------------
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight jitters when horizontally resizing window -- will be fixed in the next sprint's CodeMirror upstream merge.
* [#1267](https://github.com/adobe/brackets/issues/1267): Text selection highlight doesn't extend further to the right if you scroll the editor horizontally -- will be fixed in the next sprint's CodeMirror upstream merge.

Contributions from Brackets
---------------------------
* _To CodeMirror:_ [Bug fix](https://github.com/marijnh/CodeMirror2/commit/590a1619b7713fd1530c7f2c80e6c2b264514ea0) to prevent selection from being lost when using Ctrl+click to right-click on a Mac.

External contributions
----------------------
**Core code updates** (pull requests)
* [CFML Syntax Highlighting (as HTML)](https://github.com/adobe/brackets/pull/1138) by [Pete Freitag] (https://github.com/pfreitag)
* [Update Submodule Path To Use HTTPS](https://github.com/adobe/brackets/pull/1230) by [Moritz Heuser] (https://github.com/mheuser)
* [Improve Console Output For Live Development Errors](https://github.com/adobe/brackets/pull/1232) by [Dennis Kehrig] (https://github.com/DennisKehrig)
* [FIX 1028: File should be in working set after File > New](https://github.com/adobe/brackets/pull/1249) by [Sebastian Rodriguez] (https://github.com/srodrigu85)
* [FIX 926: On last line of file, Duplicate clones text within line instead of adding new line](https://github.com/adobe/brackets/pull/1179) by [Junk](https://github.com/jedverity)
* [FIX 1100: Update Source To Adhere to Quote Style Guide](https://github.com/adobe/brackets/pull/1211) by [Jon Wolfe](https://github.com/JonathanWolfe)
* [FIX 1133: Update _shouldShowInTree To Only Hide Specified Hidden Files.](https://github.com/adobe/brackets/pull/1165) by [Tucker Whitehouse](https://github.com/TuckerWhitehouse)
 
**New Extensions**
* [Color Picker](https://github.com/jdiehl/brackets-color-picker) by [Jonathan Diehl] (https://github.com/jdiehl)
* [Show Indentations](https://github.com/DennisKehrig/brackets-show-indentations) by [Dennis Kehrig] (https://github.com/DennisKehrig)
* [W3C Validator](https://github.com/cfjedimaster/brackets-w3cvalidation) by [Ray Camden](https://github.com/cfjedimaster)

Bugs fixed in Sprint 11
-----------------------
For details on the bugs addressed, please refer to [closed sprint 11 bugs](https://github.com/adobe/brackets/issues?labels=sprint+11&page=1&state=closed). A few other bugs might have been fixed that weren't tagged.

Thanks especially to the bug fixes contributed from the community this sprint (see list of contributions above).