What's New in Sprint 17
-----------------------
* **Live Preview**
    * ['Live Highlight' from CSS code](https://trello.com/card/3-live-development-highlight-html-elements-in-browser-from-css/4f90a6d98f77505d7940ce88/285): While Live Preview is open, putting your cursor in a CSS rule in Brackets will highlight all matching HTML elements in the browser. Use "File > Live Highlight" to toggle this off.
    * JSP and ASP files are now recognized as HTML-like for Live Preview's local server support
* **Visual Editing**
    * [Inline color picker](https://trello.com/card/3-color-selector/4f90a6d98f77505d7940ce88/662): Use Quick Edit anywhere a hex color, rgb() or hsl() appears to edit it inline. Includes shortcut swatches for  other colors used in the file.
* **Code Editing**
    * [Block comment/uncomment](https://github.com/adobe/brackets/pull/2080): Use Ctrl+Shift+/ (Cmd on Mac) to toggle block comments around the current selection.
* **Search**
    * [Find in Files scoping](https://github.com/adobe/brackets/pull/2084): Search a specific subtree or even a single file: right-click in the folder tree and choose "Find in..."
* **Localization**
    * [Turkish translation](https://github.com/adobe/brackets/pull/2127)


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-16...sprint-17#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-16...sprint-17#commits_bucket)


UI Changes
----------
No major changes to existing features.

API Changes
-----------
**Resizable panels** - Panel resizing and expand/collapse now emit a simpler, clearer set of events. See docs on `Resizer` for details.

New/Improved Extensibility APIs
-------------------------------
**InlineWidget** - Inline widgets can provide a `refresh()` function (overriding the empty stub inherited from InlineWidget) to be notified when the code editor font size changes.

Known Issues
------------
* [#1551](https://github.com/adobe/brackets/issues/1551): Changes within an extension (or a unit test) are not reflected by a simple "Debug > Reload Brackets." Workarounds:
    * Quit and re-launch Brackets to pick up the changes.
    * Open Developer Tools, click the gear icon in the lower-right, and select "Disable cache." This setting is remembered, but is only in effect so long as the Developer Tools browser tab remains open.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* _Debug > Show Developer Tools_ opens in a new tab in Chrome, rather than a new window in Brackets.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.


Community contributions to Brackets
-----------------------------------
* [Block comment/uncomment](https://github.com/adobe/brackets/pull/2080) ([and](https://github.com/adobe/brackets/pull/2148)) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Visual cue if no quick open results](https://github.com/adobe/brackets/pull/2101) by ["J.M."](https://github.com/mynetx)
* [Turkish translation](https://github.com/adobe/brackets/pull/2127) by [Ilker Guller](https://github.com/Sly777)
* [Improve expand/collapse behavior and drag events for resizable panels](https://github.com/adobe/brackets/pull/2092) by [Chema Balsas](https://github.com/jbalsas)
* [Fix: Undoing certain text operations takes multiple steps](https://github.com/adobe/brackets/pull/2132) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #2182: HTML src/href autocomplete is stale after switching projects](https://github.com/adobe/brackets/pull/2197) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #2061: No gray JSLint star when Brackets launched with JSLint disabled](https://github.com/adobe/brackets/pull/2099) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Code cleanup of Live Preview's injected JS code](https://github.com/adobe/brackets/pull/1529) by [Jonathan Diehl](https://github.com/jdiehl)
* [In-browser: Shim FileError when running in browsers that lack it](https://github.com/adobe/brackets/pull/2094) by [Maksim Lin](https://github.com/maks)
* [Italian translation update](https://github.com/adobe/brackets/pull/2090) by [Antonello Pasella](https://github.com/antonellopasella)
* [Spanish translation update](https://github.com/adobe/brackets/pull/2111) by [Chema Balsas](https://github.com/jbalsas)
* [German translation updates](https://github.com/adobe/brackets/pull/2001) ([and](https://github.com/adobe/brackets/pull/2114)) by ["J.M."](https://github.com/mynetx)
* [Japanese translation update](https://github.com/adobe/brackets/pull/2135) by [Shumpei Shiraishi](https://github.com/shumpei)

Bugs fixed in Sprint 17
-----------------------
For details on the bugs addressed, please refer to [closed sprint 17 bugs](https://github.com/adobe/brackets/issues?labels=sprint+17&state=closed). A few of the fixed bugs might not be caught by this search query, however.