This is a draft!
----------------
_This document will not be finalized until the end of Sprint 12 -- August 10._

What's New in Sprint 12
-----------------------
* **Code Hinting**
    * [Code Completion for HTML Attributes](https://trello.com/card/3-code-complete-html-attributes/4f90a6d98f77505d7940ce88/557): Basic code hinting for HTML attribute names. Appears when you type Space within a tag (or press Ctrl+Space). This is just an early step; see the [feature backlog](https://trello.com/board/brackets/4f90a6d98f77505d7940ce88) for future plans.
    * Code hinting now supports basic extensibility -- see below.
* **Native Shell**
    * [Live Development in CEF 3 Shell](https://trello.com/card/2-enable-live-development-in-cef3-shell/4f90a6d98f77505d7940ce88/539): Get existing HTML live editing functionality working in the [future new app shell](https://github.com/adobe/brackets-shell/).
    * [CEF 3 Shell Bug Fixes](https://trello.com/card/2-test-cef3-shell/4f90a6d98f77505d7940ce88/540): Fix [more bugs](https://github.com/adobe/brackets-shell/issues?labels=sprint+12&page=1&state=closed) in the new app shell. Developer Tools (for debugging Brackets or extension code) are launched in Chrome instead of in the Brackets process, to avoid CEF deadlock problems.
    * [Research Native Installer/Packaging](https://trello.com/card/2-research-brackets-native-installer/4f90a6d98f77505d7940ce88/582): Finalize plans for creating a nicely packaged DMG/.app on Mac and a native installer on Windows.
    * [Research HiDPI (Retina) Support](https://trello.com/card/0-research-hidpi-support/4f90a6d98f77505d7940ce88/585): Change to vector icons to support HiDPI, and audit UI for other issues (one layout bug found). Note: Brackets only renders in HiDPI when using the new CEF 3 app shell.
* **Other Enhancements**
    * [Move Line(s) Up/Down command](https://github.com/adobe/brackets/pull/1282)
    * [Save All command](https://github.com/adobe/brackets/pull/1208)

UI Changes
----------
no major changes to existing UI

API Changes
-----------
no major changes to existing APIs

New/Improved Extensibility APIs
-------------------------------
**Code Hinting** - Extensions can add new code hint providers via `CodeHintManager.registerHintProvider()`. However, only one extension's hints are shown at a time (first provider wins) -- similar to current limitations with [inline editor providers](https://groups.google.com/forum/?fromgroups#!topic/brackets-dev/MtxQCIOLMzk%5B1-25%5D).

**LiveDevelopment** - New API functions: ```enableAgent(name)``` and ```disableAgent(name)```.

Live Development has a number of more "experimental" agents (feature modules) that are disabled by default. This API allows extensions (like [jdiehl/brackets-debugger](https://github.com/jdiehl/brackets-debugger)) to enable these features. The new agents take effect the next time a live development session starts. Current live development sessions are not affected. 

**Async utilities** - New `doSequentiallyInBackground()` API for slicing up a synchronous task into chunks that run in the background, automatically ensuring some idle time occurs periodically.

**Menus** - `addMenuItem()` gives better feedback on console if you pass it bad arguments.

**Dialogs** -- Dialog boxes can now contain editable text fields (input or textarea).

Known Issues
------------
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight jitters when horizontally resizing window -- will be fixed in the next sprint's CodeMirror upstream merge.
* [#1267](https://github.com/adobe/brackets/issues/1267): Text selection highlight doesn't extend further to the right if you scroll the editor horizontally -- will be fixed in the next sprint's CodeMirror upstream merge.

Community contributions to Brackets
-----------------------------------
* [Add doSequentiallyInBackground() utility API](https://github.com/adobe/brackets/pull/1009) by [Josh Hatwich](https://github.com/jhatwich)
* [Move Line(s) Up/Down command](https://github.com/adobe/brackets/pull/1282) by [Bruno Filippone](https://github.com/ShadowCloud)
* [Save All command](https://github.com/adobe/brackets/pull/1208) by [srinarasi](https://github.com/srinarasi)
* [Fix bug #143: Input fields don't work in dialogs](https://github.com/adobe/brackets/pull/1325) by [Sebastian Rodriguez](https://github.com/srodrigu85)

Bugs fixed in Sprint 12
-----------------------
For details on the bugs addressed, please refer to [closed sprint 12 bugs](https://github.com/adobe/brackets/issues?labels=sprint+12&page=1&state=closed). A few other bugs might have been fixed that weren't tagged.