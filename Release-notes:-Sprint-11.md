**Sprint 11 is in progress** until 7/20. These are _**draft**_ notes.

What's New in Sprint 11
----------------------
Our main goals for the sprint were enhancing the core feature set (i.e. Initial code hinting) and to pay attention to the community feedback. We also did some bug fixing in the CEF3 related code and to pass all Unit Tests for the new App Shell.
We built Code Completion infrastructure, continued App Shell improvements and appreciate working with contributors and the fast growing community.    
    
* **Code Hinting Epic**
    * [Code Completion for HTML Tags](https://trello.com/card/5-code-complete-html-tags/4f90a6d98f77505d7940ce88/283)
* **Native Shell Epic**
    * [CEF3 Shell Enhancements](https://trello.com/card/1-cef3-shell-enhancements/4f90a6d98f77505d7940ce88/560): Fix bugs and get most unit tests passing in the [experimental new shell](https://github.com/adobe/brackets-shell/), based on Chromium Embedding Framework 3 (Brackets currently uses CEF 1). We plan to switch over to the new shell sometime within the next couple of sprints.
* **Misc**
    * [Improved Esc key handling](https://trello.com/card/1-keyboard-controls-for-quick-editors/4f90a6d98f77505d7940ce88/252): Pressing Esc while a menu is open closes it. Pressing Esc while the cursor is inside an inline editor closes it. Pressing Esc while the cursor is in a regular editor closes _all_ inline editors embedded in it.
    * [Test Scrolling Performance Enhancements](https://trello.com/card/1-evaluate-scrolling-performance-enhancements/4f90a6d98f77505d7940ce88/555)

* **Architecture / Contributions**
    * _Improved performance:_ [Switch project performance improvements] (https://github.com/adobe/brackets/pull/1197)
    * _To CodeMirror:_ [Bug fix](https://github.com/marijnh/CodeMirror2/commit/590a1619b7713fd1530c7f2c80e6c2b264514ea0) to prevent selection from being lost when using Ctrl+click to right-click on a Mac.

UI Changes
----------

API Changes
-----------
* ProjectManager - the openProject() method now accepts an optional project path. If no path is specified, then the Folder Chooser dialog is invoked (same behavior as before). If a project path is specified, then the current project is closed, and the new project is opened. The loadProject() method is now private.

New Extensibility APIs
----------------------

Known Issues
------------
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight jitters when horizontally resizing window.
* [#1267](https://github.com/adobe/brackets/issues/1267): Text selection highlight doesn't extend further to the right if you scroll the editor horizontally.   

External contributions
----------------------

Core code updates (pull requests)
[1138](https://github.com/adobe/brackets/pull/1138), by [Pete Freitag] (https://github.com/pfreitag)
[1166](https://github.com/adobe/brackets/pull/1166),
[1230](https://github.com/adobe/brackets/pull/1230),
[1232](https://github.com/adobe/brackets/pull/1232),
[1249](https://github.com/adobe/brackets/pull/1249)
 
Extensions
https://github.com/jdiehl/brackets-color-picker
https://github.com/DennisKehrig/brackets-show-indentations

Bugs fixed in Sprint 11
-----------------------
We appreciate an increasing number of bug fixes coming from the community, this as well as the feedback, a fact of the increased usage, is a great success.    
For details on the bugs addressed, please refer to [closed sprint 11 bugs](https://github.com/adobe/brackets/issues?labels=sprint+11&page=1&state=closed). A few other bugs might have been fixed that weren't tagged.