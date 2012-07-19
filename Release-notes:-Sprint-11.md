**Sprint 11 is in progress** until 7/20. These are _**draft**_ notes.

What's New in Sprint 11
----------------------
Our main goals for the sprint was enhancing the core feature set (i.e. Initial code hinting) and to pay attention to the community feedback, we also did some bug fixing in the CEF3 related code and to pass all Unit Tests for the new App Shell:

* **Code Hinting Epic**
    * [Code Completion for HTML Tags](https://trello.com/card/5-code-complete-html-tags/4f90a6d98f77505d7940ce88/283)
* **Native Shell Epic**
    * [CEF3 Shell Enhancements](https://trello.com/card/1-cef3-shell-enhancements/4f90a6d98f77505d7940ce88/560): Fix bugs and get most unit tests passing in the [experimental new shell](https://github.com/adobe/brackets-shell/), based on Chromium Embedding Framework 3 (Brackets currently uses CEF 1). We plan to switch over to the new shell sometime within the next couple of sprints.
* **Misc**
    * [Improved Esc key handling](https://trello.com/card/1-keyboard-controls-for-quick-editors/4f90a6d98f77505d7940ce88/252): Pressing Esc while a menu is open closes it. Pressing Esc while the cursor is inside an inline editor closes it. Pressing Esc while the cursor is in a regular editor closes _all_ inline editors embedded in it.
    * [Test Scrolling Performance Enhancements](https://trello.com/card/1-evaluate-scrolling-performance-enhancements/4f90a6d98f77505d7940ce88/555)

* **Architecture / Contributions**

UI Changes
----------

API Changes
-----------

New Extensibility APIs
----------------------

Bugs fixed in Sprint 11
-----------------------
See [closed sprint 11 bugs](https://github.com/adobe/brackets/issues?labels=sprint+11&page=1&state=closed). A few other bugs might have been fixed that weren't tagged.