What's New in Sprint 20
-----------------------
* **Code Editing**
    * [Migrated to CodeMirror 3](https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/0Jybk-PQJWQ): This enables many of the benefits listed below
    * Undo restores old selection / cursor position much more reliably
    * Granularity of Undo steps is more predictable
    * Gutter line numbers stay in view when scrolling horizontally
    * When closing HTML tags are auto-inserted, newlines are never auto-inserted along with them (we may bring back auto newlines as a preference in a future build)
    * Double-click-drag and triple-click-drag to select multiple words/lines quickly
    * CSS code coloring is more colorful
    * Haxe syntax highlighting
* **Search**
    * All Find results are highlighted while search bar is open
    * Current result is centered in the viewport instead of often appearing at its edges (Find, Find in Files, Quick Open, etc.)
    * Quick Open / Go to Definition results update even faster as you type incrementally (building on last sprint's improvements)
* **Code Hinting**
    * CSS code hinting is less aggressive - it will pop up once you start typing a property name, but not before then (unless you explicitly press Ctrl+Space)
    * Fixed many code hinting issues: CSS code hints now work in inline editors, [plus](https://github.com/adobe/brackets/issues/2775) [a](https://github.com/adobe/brackets/issues/2601) [whole](https://github.com/adobe/brackets/issues/2625) [bunch](https://github.com/adobe/brackets/issues/2682) [of](https://github.com/adobe/brackets/issues/2628) [other](https://github.com/adobe/brackets/issues/2626) [fixes](https://github.com/adobe/brackets/issues/2586).


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-19...sprint-20#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-19...sprint-20#commits_bucket)

API Changes
-----------
**CodeMirror API changes** - Public Brackets editor APIs have not changed. However, if you access raw CodeMirror APIs directly (by using the `_codeMirror` property on Editor or setting CSS styles on CodeMirror instances), you'll need to make sure your code is updated to reflect the [changes in the CodeMirror v3 API](http://codemirror.net/doc/upgrade_v3.html).

If you want your extension to work with both Brackets and Edge Code (please do!), you will need your code to handle _both_ the v2 and v3 APIs for now, because Edge Code hasn't been updated to CodeMirror v3 yet. (For example, in cases where you're passing x/y in an object today, you might need to make your code pass both x/y and left/top.) Again, this should only be an issue if you're using raw CodeMirror APIs; Brackets APIs like `Editor.setScrollPos()` haven't changed.

**Editor `offsetTopChanged` event removed** - This was deprecated in [sprint 19](https://github.com/adobe/brackets/wiki/Release-Notes:-Sprint-19) and has been removed in sprint 20.

New/Improved Extensibility APIs
-------------------------------
**Selection centering** - Editor's `setSelection()` and `setCursorPos()` now offer the option to center the viewport on the targeted text when scrolling. Most "go to"-type actions should use this option -- pass true as an additional argument.

**Quick Open providers** - The provider's `search()` method is now passed an additional argument, `stringMatcher`. Using `stringMatcher.match()` instead of `QuickOpen.match()` will yield improved performance through caching. You can also cache your own data by adding properties to this object; see _extensions/default/QuickOpenHTML/main.js_ for sample usage. Use of this argument is optional.


Known Issues
------------
* [#1551](https://github.com/adobe/brackets/issues/1551): Changes within an extension (or a unit test) are not reflected by a simple "Debug > Reload Brackets." Workarounds:
    * Quit and re-launch Brackets to pick up the changes.
    * Open Developer Tools, click the gear icon in the lower-right, and select "Disable cache." This setting is remembered, but is only in effect so long as the Developer Tools browser tab remains open.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.


Community contributions to Brackets
-----------------------------------
* [Highlight results while Find bar open](https://github.com/adobe/brackets/pull/2662) largely by [Aleksandr Motsjonov](https://github.com/soswow)
* [Fix #2085: Cannot create new file in collapsed, empty folder](https://github.com/adobe/brackets/pull/2592) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #2509: Double-clicking in rename input acts like double-clicking file normally](https://github.com/adobe/brackets/pull/2802) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix #2688: Status bar JSLint tooltip is incorrect when JSLint gives up mid-file](https://github.com/adobe/brackets/pull/2690) by [Kieran Gorman](https://github.com/kjgorman)
* [Fix #850: Give a more useful NativeFileError code for encoding problems](https://github.com/adobe/brackets/pull/2639) by [connork09](https://github.com/connork09)
* [Code cleanups: move away from Promise.then(), and remove unneeded additionalKeys arguments](https://github.com/adobe/brackets/pull/2685/files) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [German translation update](https://github.com/adobe/brackets/pull/2789) ([and](https://github.com/adobe/brackets/pull/2800)) by [J.M.](https://github.com/mynetx)
* [Contributor guidelines: add info on starter bugs/features](https://github.com/adobe/brackets/pull/2841) by [Julian Viereck](https://github.com/jviereck)


Contributions _from_ Brackets
-----------------------------
**Contributions to [CodeMirror](https://github.com/marijnh/CodeMirror):**
* [Fix recently introduced typo](https://github.com/marijnh/CodeMirror/commit/96c4963bd7d5a45bbe034601c9bff33cace8a256)
* [Wrap undo/redo in an operation](https://github.com/marijnh/CodeMirror/commit/ae179ddd1fe0562ae9f46ddf212d7fad67cc317e)
* [Need to examine a longer substring for text/x-handlebars-template](https://github.com/marijnh/CodeMirror/commit/c6ad81e67271b5d44e8167591b71be19dcb5d14b)
* [widgetsSeen is no longer an array](https://github.com/marijnh/CodeMirror/commit/7ae4ba63f272cbc989ad10fe31029ff4239a0d22)
* [Port htmlmixed code](https://github.com/marijnh/CodeMirror/commit/a8b5188296f4254a5216758e41d200db1e6fcdb2)



Bugs fixed in Sprint 20
-----------------------
For details on the bugs addressed, please refer to [closed sprint 20 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=7&state=closed). A few of the fixed bugs might not be caught by this search query, however.