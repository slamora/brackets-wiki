_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 38 -- approximately April 14._

What's New in Sprint 38
-----------------------
* **Multiple Cursors**
    * [Multiple cursor / multiple selection support](https://trello.com/c/g58aNzCz/1187-finish-multiple-selection-multiple-cursor-support), including...
    * Ctrl/Cmd-click or drag to create multiple cursors/selections
    * Alt-drag to create a rectangular selection (add Ctrl/Cmd to select multiple rectangles)
    * Ctrl/Cmd-U to undo selection/cursor changes (add Shift to navigate forward in cursor history)
    * Ctrl/Cmd-B to add a new selection at the next occurrence of a word or selected text. Shift-Ctrl/Cmd-B to skip an occurrence and add a selection on the next one. Alt-F3 or Cmd-Ctrl-G to add _all_ occurrences as selections at once.
    * [Read the full documentation for details](https://github.com/adobe/brackets/wiki/Working-with-Multiple-Selections).
* **Linting**
    * [Support asynchronous linting](https://github.com/adobe/brackets/pull/6530): Linters that require reading extra files from disk or talking to network services can now integrate with the standard Brackets linting UI.
* **Search**
    * [Improved usability of file/folder exclusions](https://github.com/adobe/brackets/pull/7400): Filter editor displays how many files are left after filtering. Search shows warning if filter excludes all files in subtree. Filters disabled when searching only a single file.
    * [Improved performance on Windows](https://github.com/adobe/brackets/pull/7290) if large binary files exist in the project
* **Files and Folders**
    * [File tree expand/collapse shortcuts](https://github.com/adobe/brackets/pull/7026/files): Ctrl/Cmd-click to expand/collapse all siblings; Ctrl/Cmd-Alt-click to collapse subtree.
* **Image Preview**
    * [View .ico files](https://github.com/adobe/brackets/pull/7201), e.g. often used as website "favicon"
* **Overall UI**
    * [Visual polish, subtle animations added](https://github.com/adobe/brackets/pull/5921)
* **Localization**
    * [UK English locale added](https://github.com/adobe/brackets/pull/7333)
    * Process change: translation pull requests should include which SHA (commit) the update is based on.  [See localization README for details](https://github.com/adobe/brackets/blob/master/src/nls/README.md).
* **Ongoing Research** (not implemented yet)
    * [Research: JS Code Hints Cleanup](https://trello.com/c/heHZlATB/1158-research-js-code-hints-cleanup): Investigate how to simplify the code, improve performance & maintainability, and unify with new preferences system.
    * [Research: Improved LESS support](https://trello.com/c/qv5gTqXp/1163-s-research-early-less-support): Investigate support for Live Preview, Quick Edit, and Quick Find Definition with LESS code.

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-37...sprint-38#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-37...sprint-38#commits_bucket)


UI Changes
----------
**Find in Files** - If you've set a [file/folder exclusion filter](https://github.com/adobe/brackets/wiki/Using-File-Filters) and then you use 'Find In...' on a single file, filtering is temporarily disabled to ensure the file is always searched.


API Changes
-----------
**CodeMirror v4 & multiple cursors/selections** - See the **[[Brackets CodeMirror v4 Migration Guide]]**. Notably, referencing `CodeMirror` as a global identifier is now deprecated; use Require instead, as with other modules.

**PHP files** - `Editor.getLanguageForSelection()` will now return the correct `"php"` language (instead of the generic `"clike"`).

New/Improved Extensibility APIs
-------------------------------
**Inline Editors** - When a provider has nothing to show for the current cursor position, it can return an explanation string instead of just `null`. If no providers respond, the explanation is shown in a popup. This can help make inline editing functionality more discoverable to users.

**LanguageManager** - If a file extension already has a default Language mapping in Brackets, an extension can unregister the mapping in order to use it with a new Language. (For example, changing ".twig" from generic HTML highlighting to a new Twig-specific highlighting mode). Use `Language.removeFileExtension()` and `removeFileName()`.

**Editor** - All `get*` and `set*` methods for Editor settings now take an optional path parameter to get setting for a particular file. If omitted, then the current document is used. The `set*` methods also now return a boolean value which indicates whether value is valid.

In addition, all the add/remove file name/extension APIs can now optionally be passed an array of items instead of a single item.

**Jump to Definition** - use `EditorManager.registerJumpToDefProvider()` to register a provider for the _Navigate > Jump to Definition_ command.


Known Issues
------------
* With Chrome 34, using Live Preview will show a console error: "'CSS.getAllStyleSheets' wasn't found". This is harmless and shouldn't affect Live Preview. See [#7127](https://github.com/adobe/brackets/issues/7127).
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* On Windows XP, Brackets will not detect external file changes instantly. It behaves similarly to Sprint 35 and earlier releases - changes are detected upon window activation, and the folder tree must be manually refreshed.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. [See workaround](https://support.google.com/chrome/answer/106010?hl=en).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
TODO

#### Pulling source code from Git
TODO

Bugs fixed in Sprint 38
-----------------------
For details on the bugs addressed, please refer to [closed sprint 38 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=25&state=closed). Not all fixed bugs will be caught by this search query, however.