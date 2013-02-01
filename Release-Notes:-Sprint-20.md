_**This is a draft!** Release notes for Sprint 20 will be finalized at the end of the sprint, around 2/12/13._

What's New in Sprint 20
-----------------------
* **Code Editing**
    * [Migrated to CodeMirror 3](https://trello.com/card/5-codemirror-3-final-merge/4f90a6d98f77505d7940ce88/735): This enables many of the benefits listed below
    * Undo restores old selection / cursor position much more reliably
    * Granularity of Undo steps is more predictable
    * Gutter line numbers stay in view when scrolling horizontally
    * Newlines are no longer added when certain closing tags are auto-inserted (we may bring this back as a preference in a future build)
    * Double-click-drag and triple-click-drag to select multiple words/lines quickly
    * CSS code coloring is more colorful
    * Haxe syntax highlighting
* **Search**
    * All Find results are highlighted while search bar is open
    * Current result is centered in the viewport instead of often appearing at its edges (Find, Find in Files, Quick Open, etc.)
    * Quick Open / Go to Definition results update even faster as you type incrementally (building on last sprint's improvements)
* **Code Hinting**
    * CSS code hinting is less aggressive - it will pop up once you start typing a property name, but not before then (unless you explicitly press Ctrl+Space)
    * Bug fix: CSS code hinting now works in inline editors


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-19...sprint-20#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-19...sprint-20#commits_bucket)

UI Changes
----------

API Changes
-----------
**Editor `offsetTopChanged` event removed** - This was deprecated in [sprint 19](https://github.com/adobe/brackets/wiki/Release-Notes:-Sprint-19) and has been removed in sprint 20.

New/Improved Extensibility APIs
-------------------------------


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



Contributions _from_ Brackets
-----------------------------




Bugs fixed in Sprint 20
-----------------------
For details on the bugs addressed, please refer to [closed sprint 20 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=7&state=closed). A few of the fixed bugs might not be caught by this search query, however.