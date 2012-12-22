**Note that this build has a new download location: _http://download.brackets.io/_**

This final sprint of 2012 focused largely on architectural work preparing for future sprints, so there are fewer new features than usual (and more API changes).

What's New in Sprint 18
-----------------------
* **Code Hinting**
    * [Code hint provider API improvements](https://trello.com/card/0-research-code-hint-provider-api/4f90a6d98f77505d7940ce88/723): APIs for all code hints have been streamlined to support multiple hint providers per language more cleanly, remove bug-prone pitfalls, and do less processing per keystroke. Details below and in the [API spec](New-Code-Hinting-API-Proposal) and [JSDocs](https://github.com/adobe/brackets/blob/master/src/editor/CodeHintManager.js); background in the [functional spec](Code-Hinting-Functional-Specifications) and [forum](https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/Luf7IN0-iAM).
    * [CSS cursor-context API](https://trello.com/card/2-css-utilities-api-to-support-code-completion/4f90a6d98f77505d7940ce88/713): New CSS-parsing functionality was added to support the [ongoing effort to add CSS code hinting](https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/sAdPhV3ffOY) in a future sprint. Details below and in the [API spec](CSS-Context-API-implementation-spec).
    * [Don't hint attributes already on an HTML tag](https://github.com/adobe/brackets/issues/2263)
* **Code Editing**
    * [Toggle Line Comment for CSS & HTML](https://github.com/adobe/brackets/pull/2133): Toggle Line Comment now inserts/removes one-line block comments instead of doing nothing.
    * [Enable Python syntax highlighting](https://github.com/adobe/brackets/issues/2355)
    * [Progress on CodeMirror 3 migration](https://trello.com/card/2-codemirror-3-inline-editor-open-edit/4f90a6d98f77505d7940ce88/650): Inline editors are now mostly working on the [cmv3 branch](https://github.com/adobe/brackets/compare/master...cmv3), with one major exception: they are not always the correct height.
* **Overall UI**
    * [Select most recently visited file after Close](https://github.com/adobe/brackets/pull/2122): Previously, the bottom-most file on the open files list was always selected.
    * "Search bars" (Find, Replace, Quick Open) no longer cover up search results that occur in the first 2 lines of code in the viewport.
* **Localization**
    * [Russian translation](https://github.com/adobe/brackets/pull/2268)
* **Extensions**
    * [Research language extensibility](https://trello.com/card/0-research-language-extensibility/4f90a6d98f77505d7940ce88/712): Planning for for a [future feature](https://trello.com/card/api-for-extensions-to-add-new-language-syntax-coloring-mode/4f90a6d98f77505d7940ce88/639) to allow extensions to add support for new file types. As a guinea pig, we're working on [moving LESS support from core out into an extension](https://trello.com/card/0-less-refactor-into-extension/4f90a6d98f77505d7940ce88/711).
* **Developer Workflow**
   * The About dialog now indicates whether Brackets is running in post-"setup_for_hacking" mode (pointing at git source repo) or normal mode (source in the installation directory). Look for the text "development build" (hacking) vs. "experimental build" (normal).

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-17...sprint-18#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-17...sprint-18#commits_bucket)


UI Changes
----------
No major changes to existing features.


API Changes
-----------

**Code hints**
- **registerHintProvider()** - When registering with CodeHintManager, hint providers must now specify a list of supported CodeMirror mode names (e.g., "html" or "css"), and an integer priority (the UI is currently "winner take all," so the first provider to offer hints for a given mode is the only one whose hints are shown). Higher numbers should be used for providers that are more specialized (higher specificity).
- **Hint provider interface** - Code hint providers must now implement a different, simpler interface. There are three required methods: 
 1. ``hasHints()`` - Indicates whether a provider is willing to provide hints for the current editor and cursor location, and begins a new hinting session if so.
 2. ``getHints()`` - Returns either an immediate, synchronous response or a Promise that will eventually resolve to the response; the response contains a list of hints along with several other parameters.
 3. ``insertHint()`` - Inserts a hint into the current editor context and indicates whether code hints should remain open afterward (updated for the new cursor position).
- Complete API details available at: [New Code Hinting API Proposal](New-Code-Hinting-API-Proposal)

**Inline widgets**
- InlineWidget now automatically adjusts its width to match the host editor (see `updateWidth()`).
- Widgets no longer have to provide an `_editorHasFocus()` method or override `close()` to avoid crashes when that function doesn't exist.
- `EditorManager.closeInlineWidget()` no longer takes a `moveFocus` parameter. Instead, it auto-detects whether the inline widget has focus.
- `EditorManager.getFocusedInlineWidget()` now returns _any_ focused InlineWidget, not just subclasses of InlineTextEditor. It returns the widget directly instead of a bag containing it.
- If you override `InlineWidget.onClosed()`, chaining to the original method is now _required_ since it's no longer an empty stub.
- New APIs: `InlineWidget.hasFocus()` and `InlineTextEditor.getFocusedEditor()`

**NativeFileSystem** - several changes to match the latest [Directories and System draft spec](http://www.w3.org/TR/file-system-api/):
- **requestNativeFileSystem()** - successCallback is now passed a FileSystem object with a `root` DirectoryEntry property (instead of being passed the DirectoryEntry directly).
- **FileError -> NativeFileError** (which implements DOMError) - For error codes, `FileError.code` -> `DOMError.name` and the constants for these values have moved from FileError to the NativeFileError enumeration (in its own separate module).
- [JSDocs in NativeFileSystem](https://github.com/adobe/brackets/blob/master/src/file/NativeFileSystem.js) are much improved

**Toolbar icons** - Any icons added to the `#main-toolbar > .buttons` div must be 16px tall; otherwise all icons in the toolbar will become misaligned.

**Native shell build** - The [brackets-shell](https://github.com/adobe/brackets-shell) build procedure has changed. Note: you _only_ need to build brackets-shell if you're editing the native shell code - editing Brackets' HTML/JS/LESS code does not require this. To set up for the new build environment, follow the [updated build setup instructions](https://github.com/adobe/brackets-shell/wiki/Building-Brackets-Shell). You can follow these steps either on a fresh clone of brackets-shell or on an existing environment that was previously using the old build setup.

Warning: do _NOT_ attempt to use the new build process on Windows XP. Building brackets-shell is not supported on XP, and the new scripts could delete your entire Brackets repo as a result (specifically, if you have a pre-existing XP build environment rigged up that you run these scripts on).

New/Improved Extensibility APIs
-------------------------------
**CSSUtils** - Use ``getInfoAtPos()`` to get info about the CSS code near a given cursor position, similar to the longstanding `HTMLUtils.getTagInfo()` API.

**Key bindings** - to decline to handle a key event, a command can return a Promise that is already (synchronously) rejected at the time it's being returned. Other code or the browser default action will then handle the event. This is useful when you don't want to a bind key globally - e.g. the [Emmett extension](https://github.com/emmetio/emmet/downloads) handles the Enter key when the focus is in an HTML editor, but not when the focus is in the Find bar.

It is now possible to specify a default platform-agnostic keybinding alongside one or more platform-specific bindings. This is a useful distinction as the [experimental Linux build](https://groups.google.com/d/topic/brackets-dev/29vOJ6tvl8A/discussion) continues progressing.

**ExtensionUtils** - new APIs:
- ``addEmbeddedStyleSheet()`` - Appends an inline `<style>` tag to the document's head.
- ``addLinkedStyleSheet()`` - Appends a `<link>` tag to the document's head and tracks its loading.
- ``parseLessCode()`` - Converts LESS code to CSS code.
- ``getModulePath()`` - Returns the local disk path of an extension module.
- ``getModuleUrl()`` - Returns the `file://` URL of an extension module.
- ``loadFile()`` - Loads a file from an extension-module-relative path.
- The existing ``loadStyleSheet()`` now supports .less files in addition to .css


Known Issues
------------
* [#1551](https://github.com/adobe/brackets/issues/1551): Changes within an extension (or a unit test) are not reflected by a simple "Debug > Reload Brackets." Workarounds:
    * Quit and re-launch Brackets to pick up the changes.
    * Open Developer Tools, click the gear icon in the lower-right, and select "Disable cache." This setting is remembered, but is only in effect so long as the Developer Tools browser tab remains open.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* _Debug > Show Developer Tools_ opens in a new tab in Chrome, rather than a new window in Brackets.
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight sometimes jiggles when horizontally resizing window.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.


Community contributions to Brackets
-----------------------------------
* [Select most recently visited file after Close](https://github.com/adobe/brackets/issues/2122) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Line Comment for CSS & HTML (1-line block comment)](https://github.com/adobe/brackets/issues/2133) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Enable Python syntax highlighting](https://github.com/adobe/brackets/issues/2355) by [Justin Abrahms](https://github.com/justinabrahms)
* [Filter out Python .pyc files](https://github.com/adobe/brackets/issues/2366) by [Justin Abrahms](https://github.com/justinabrahms)
* [Don't code-hint attributes already on an HTML tag](https://github.com/adobe/brackets/issues/2263) by [Chema Balsas](https://github.com/jbalsas)
* [Default to Windows keybindings on Linux](https://github.com/adobe/brackets/issues/2331) based on [earlier work](https://github.com/adobe/brackets/pull/2000) by [Pritam Baral](https://github.com/pritambaral) (note: Linux builds are experimental; see [forum thread](https://groups.google.com/d/topic/brackets-dev/29vOJ6tvl8A/discussion) for more info)
* [Russian translation](https://github.com/adobe/brackets/pull/2268) ([and](https://github.com/adobe/brackets/issues/2320)) by ["noway421"](https://github.com/noway421), reviewed by [Arty Bolembakh](https://github.com/TurboTurkey), and inspired by [Vladimir Aus](https://github.com/VladimirAus)'s [original](https://github.com/adobe/brackets/pull/1900)
* [Update Quick Open text field label based on mode](https://github.com/adobe/brackets/pull/2298) inspired by [Sagar](https://github.com/sagarsane)'s [original](https://github.com/adobe/brackets/pull/1706)
* [Fix several block comment issues](https://github.com/adobe/brackets/issues/2342) ([and](https://github.com/adobe/brackets/issues/2121)) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #2085: Can't use File > New File on collapsed folder](https://github.com/adobe/brackets/issues/2198) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #2270: JSLint star missing tooltip if disabled on startup](https://github.com/adobe/brackets/issues/2280) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Check for updates daily even if Brackets left running](https://github.com/adobe/brackets/pull/2368) by ["J.M."](https://github.com/mynetx)
* [Enable Haxe syntax highlighting on cmv3 branch](https://github.com/adobe/brackets/pull/2075) (not available in mainline Brackets yet) by [Andy Li](https://github.com/andyli)
* [Use ellipsis symbol instead of dots in menu items](https://github.com/adobe/brackets/issues/2240) by ["J.M."](https://github.com/mynetx)
* [requestNativeFileSystem() should return FileSystem to match spec](https://github.com/adobe/brackets/issues/2158) by [Chema Balsas](https://github.com/jbalsas)
* [NativeFileSystem should use NativeFileError to match spec](https://github.com/adobe/brackets/issues/2318) by [Chema Balsas](https://github.com/jbalsas)
* [NativeFileSystem documentation and examples](https://github.com/adobe/brackets/issues/2063) by [Chema Balsas](https://github.com/jbalsas)
* [Color editor CSS cleanups](https://github.com/adobe/brackets/issues/2225) by ["J.M."](https://github.com/mynetx)
* [Spanish translation update](https://github.com/adobe/brackets/issues/2390) ([and](https://github.com/adobe/brackets/issues/2273)) by [Chema Balsas](https://github.com/jbalsas)
* [German translation update](https://github.com/adobe/brackets/issues/2367) ([and](https://github.com/adobe/brackets/issues/2224)) by ["J.M."](https://github.com/mynetx)
* [Add CONTRIBUTING file](https://github.com/adobe/brackets/issues/2304) by ["J.M."](https://github.com/mynetx)


Contributions _from_ Brackets
-----------------------------
**Contributions to [CodeMirror](https://github.com/marijnh/CodeMirror):**
* [Unit tests for line focus restoration](https://github.com/marijnh/CodeMirror/commit/fee2bb7660f914a1a71e695ce0584ed91fe5d73a)
* [Ignore certain events when target is inside a line widget](https://github.com/marijnh/CodeMirror/commit/9a076b991545390a0d3034e66434724185f19119)
* [Track global variables alongside locals in the parser state](https://github.com/marijnh/CodeMirror/commit/1bcc97635b11db2030697e5bb121d547c8c456e4)
* [Use globals as well as locals in hinting](https://github.com/marijnh/CodeMirror/commit/426a38985a7bef76b190ad3717c9855911b86662)

Bugs fixed in Sprint 18
-----------------------
For details on the bugs addressed, please refer to [closed sprint 18 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=5&state=closed). A few of the fixed bugs might not be caught by this search query, however.