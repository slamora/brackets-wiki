What's New in Sprint 19
-----------------------
* **Overall UI**
    * [Native menu bar](https://trello.com/board/brackets/4f90a6d98f77505d7940ce88) on Mac and Windows. Menu-related APIs remain unchanged.
* **Code Editing**
    * [CSS code hints](https://github.com/adobe/brackets/pull/2492) (thanks to [André Zoufahl](https://github.com/zoufahl)) for properties & enum-style values
    * Final prep for CodeMirror3 migration: [inline editors](https://trello.com/card/2-codemirror-3-inline-editor-size-vertically/4f90a6d98f77505d7940ce88/651) and [scrolling](https://trello.com/card/3-codemirror-3-scrolling/4f90a6d98f77505d7940ce88/652) now work well on the [cmv3 branch](https://github.com/adobe/brackets/compare/master...cmv3) (thanks to [Marijn Haverbeke](https://github.com/marijnh) for fast turnaround on [CodeMirror](https://github.com/marijnh/CodeMirror) bug fixes)
* **Search**
    * Quick Open / Go to Definition ranks results much better
    * Go to Definition is more responsive, especially on longer files
* **Localization**
    * [Swedish translation](https://github.com/adobe/brackets/pull/2477) (thanks to [Jack Billström](https://github.com/jackbillstrom))
    * [Portuguese translation](https://github.com/adobe/brackets/pull/2582) (thanks to [Tiago Oliveira](https://github.com/Tiagoliveira))
* **JSLint**
    * ["Go to First JSLint Error" shortcut](https://github.com/adobe/brackets/pull/2525): F8 on Windows, Cmd+' on Mac


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-18...sprint-19#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-18...sprint-19#commits_bucket)


UI Changes
----------
* **Toggle Block Comment** on Mac now uses Cmd+Opt+/ (instead of Cmd+Shift+/)
* **Cmd+Left arrow** on mac now behaves the same as Fn+Left arrow &ndash; the cursor moves to the first _non-whitespace_ character on the line unless it's already on that char, in which case the cursor moves to the start of the line. (Previously, Cmd+Left always moved to the start of the line regardless of whitespace).


API Changes
-----------
**Startup order of events** - Extensions are now loaded prior to project and working set initialization. Modules should continue to use ``AppInit.htmlReady`` for DOM initialization and ``AppInit.appReady()`` for additional initialization such as menus, key bindings, etc.

**Editor `offsetTopChanged` event deprecated** - `Editor` currently dispatches an `offsetTopChanged` event on inline (Quick Edit) widgets when something happens that changes their vertical position relative to the page. This used to be necessary for CSS and JS inline editors, which were doing hacky things to position the right-hand-side list. As part of the CodeMirror v3 merge, we will be eliminating those hacks, and plan to remove this event eventually, so as of now the event is deprecated. Please let us know if you rely on this event.

**ContextMenu `contextMenuClose` event removed** - This event was not very useful/reliable; in many cases, it was never actually dispatched after a menu closed. We will likely reinstate it once we've improved our handling of popups (#1381).

New/Improved Extensibility APIs
-------------------------------
None in this sprint.

Known Issues
------------
* [#1551](https://github.com/adobe/brackets/issues/1551): Changes within an extension (or a unit test) are not reflected by a simple "Debug > Reload Brackets." Workarounds:
    * Quit and re-launch Brackets to pick up the changes.
    * Open Developer Tools, click the gear icon in the lower-right, and select "Disable cache." This setting is remembered, but is only in effect so long as the Developer Tools browser tab remains open.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* _Debug > Show Developer Tools_ opens in a new tab in Chrome, rather than a new window in Brackets.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.


Community contributions to Brackets
-----------------------------------
* [CSS Code Hinting](https://github.com/adobe/brackets/pull/2498) by [André Zoufahl](https://github.com/zoufahl)
* [Portuguese translation](https://github.com/adobe/brackets/pull/2582) by [Tiago Oliveira](https://github.com/Tiagoliveira)
* [Swedish translation](https://github.com/adobe/brackets/pull/2338) by [Jack Billström](https://github.com/jackbillstrom)
* [Fix #2464: If tag not closed, HTML hints missing attributes found on next tag](https://github.com/adobe/brackets/pull/2465) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #2227: No color picker if rgba value starts with "."](https://github.com/adobe/brackets/pull/2446) by [chtennek](https://github.com/chtennek)
* [Fix #1390: No 'Go to Definition' results in files containing length()](https://github.com/adobe/brackets/pull/2317) by [Chema Balsas](https://github.com/jbalsas)
* [Inform user about missing Brackets libraries](https://github.com/adobe/brackets/pull/2440) by [Jonathan Diehl](https://github.com/jdiehl)
* [Use "smart home" for Cmd+Left, not just Fn+Left](https://github.com/adobe/brackets/pull/2483) by [Aleksandr Motsjonov](https://github.com/soswow)
* [Fix #2434: restore_installed_build fails if spaces in path](https://github.com/adobe/brackets/pull/2484) by [Aleksandr Motsjonov](https://github.com/soswow)
* [Use default cursor more, pointing hand less](https://github.com/adobe/brackets/pull/2144) by [J.M.](https://github.com/mynetx)
* [Enable syntax highlighting for CakePHP .ctp files](https://github.com/adobe/brackets/pull/2430) by [drewhjava](https://github.com/drewhjava)
* [Enable syntax highlighting for .dhtml, .xht, Python .pyw files](https://github.com/adobe/brackets/pull/2466) by [C1D](https://github.com/C1D)
* [Refactor jsTree to externalize CSS styles](https://github.com/adobe/brackets/pull/2553) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Russian translation update](https://github.com/adobe/brackets/pull/2437) by [noway421](https://github.com/noway421)
* [Spanish translation update](https://github.com/adobe/brackets/pull/2589) by [Chema Balsas](https://github.com/jbalsas)
* [German translation update](https://github.com/adobe/brackets/pull/2580) (and [for brackets.io](https://github.com/adobe/brackets/pull/2438)) by [J.M.](https://github.com/mynetx)
* [Fix Turkish translation typo](https://github.com/adobe/brackets/pull/2460) by [Mahmut Bulut](https://github.com/vertexclique)


Contributions _from_ Brackets
-----------------------------
**Contributions to [CodeMirror](https://github.com/marijnh/CodeMirror):**
* [widget.remove() should be widget.clear()](https://github.com/marijnh/CodeMirror/commit/db1b28207d5b8b799d7202cf47bb9ece1c0afb3c)
* [Make temporary wheel measurement vars per-instance](https://github.com/marijnh/CodeMirror/commit/ece10c7208da8f36001f3ff02a86d0bd6612c0bb)

**Contributions to [grunt-contrib-watch](https://github.com/gruntjs/grunt-contrib-watch):**
* [Use grunt.file.expand to support exclusions](https://github.com/gruntjs/grunt-contrib-watch/pull/30)

**New jQuery plugin:**
* [Balanced text plugin/polyfill](https://github.com/adobe-webplatform/balance-text)

**Contribution to reveal.js:**
* [Fade transition](https://github.com/hakimel/reveal.js/pull/301)

Bugs fixed in Sprint 19
-----------------------
For details on the bugs addressed, please refer to [closed sprint 19 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=6&state=closed). A few of the fixed bugs might not be caught by this search query, however.