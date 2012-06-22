What's New in Sprint 10
----------------------
Our main goals for the sprint were similar to the previous sprint - Continue to make progress on the extensibility architecture of Brackets: *research, basic architecture and extensibility API*:

* **Architecture / Contributions**
     * [CodeMirror contributions](https://trello.com/c/XxKi7w8x) (NJ): After several rounds of iteration, our fixes to eliminate flickering during vertical scrolling were merged into the CodeMirror codebase. The primary pull request was [here](https://github.com/marijnh/CodeMirror2/pull/551), with a few bug fixes following.
     * [Build experimental CEF3 Shell](https://trello.com/c/8Vuom2dA) (Glenn, Raymond): The current Brackets shell is based on the original version of CEF (Chromium Embedded Framework). In this sprint, we started building a new shell based on CEF3; you can see the work so far in the [brackets-shell repo](https://github.com/adobe/brackets-shell). This is still a work in progress; there hasn't been a lot of scenario testing yet. The changes are manifested in the performance improvements and better responsiveness running in the multi-process mode. We plan to switch over to the new shell sometime within the next couple of sprints.

* **Extensibility Epic**     
      * [JavaScript Quick Edit](https://trello.com/c/8d7sdB54) (NJ, Jason, Randy, Peter, Joel): This story, which was prototyped in an earlier sprint, is now an official feature. Hitting Cmd+E/Ctrl+E on a function name will open all function definitions with that name in an inline editor. Performance instrumentation was added to help the team optimize the JavaScript file parsing. Performance test reporting can now display metrics as a hierarchy to show how individual metrics roll-up. Also, as part of this feature, we added the ability to create unit tests for extensions; see [Extension Unit Tests](https://github.com/adobe/brackets/wiki/Extension-Experiments#wiki-unittests).    
      * [HTML Context Menus](https://trello.com/c/Um2Nlhh9) (Ty, Randy, Peter): This extends the previous menu API to support context menus. As part of this work, we also added a couple of entries to the context menus in the editor and project file tree; we'll add more items over time. See also notes on menu API changes below. The main pull request for this feature is found [here](https://github.com/adobe/brackets/pull/1012).
      * [Extension support for require.js text plugin](https://trello.com/c/BKQnEDRa) (Jason): This enables extension developers to use the [require.js text plugin](https://github.com/requirejs/text) without having to include it for each extension.    
      * [Extension Stylesheets](https://trello.com/c/ltSP2dcY) (Jason): Extensions may now use the `ExtensionUtils.loadStyleSheet()` API to load a custom stylesheet for extension UI. See [Extension UI Guidelines](https://github.com/adobe/brackets/wiki/Extension-UI-Guidelines) for best practices.
      * [Extension UI Guidelines](https://trello.com/c/tDdqua2R) (Garth): We've updated the [Extension UI Guidelines](https://github.com/adobe/brackets/wiki/Extension-UI-Guidelines) to include a lot more information about how we intend extensions to add to the Brackets UI. Not all of these guidelines are applicable today, since not all the underlying extensibility functionality is in Brackets yet, but this gives a roadmap for where we think extension UI should go in the future. Please give us feedback on the brackets-dev group.

Major External Contributions
----------------------------
* [cantrell](http://github.com/cantrell) added Restore Default Font Size (Cmd+0/Ctrl+0) and made it so the scroll position is retained when the font size is changed.

UI Changes
----------
* "Debug > Enable JSLint" has moved to "View > Enable JSLint"
* "Debug > Use Tab Characters" has moved to "Edit > Use Tab Characters"

Menu API Changes
----------------
The `Menu.addMenuItem()` function no longer takes an ID parameter.

Also, the relativeID parameter has changed. Previously this parameter specified the id of another MenuItem. Now it references the id of a Command that exists in the parent menu to specify a location. The net result is that MenuItems no longer have externally exposed ID’s.

These changes simplify the API, but will break existing extensions that add menus, so please update your Brackets extensions with this change.

Document/Editor API Changes
---------------------------
Mutator APIs have been added to Document. To modify text, use these Document APIs instead of going through Editor or CodeMirror. For details on Document usage, see "Working with Documents" in the [[Brackets Development How Tos]].

* Added to Document: `replaceRange()`, `getRange()`, `getLine()`, `batchOperation()`.
* Changed `Document.getText()` so it always returns \n line endings (like the above new APIs) unless a special flag is passed.
* Removed `Editor.getLineText()`&mdash;replaced by `Document.getLine()`

Project API Changes
-------------------
The events dispatched by ProjectManager have changed. Instead of dispatching `initializeComplete` and `projectRootChanged`, we now dispatch `projectOpen` and `beforeProjectClose` events. `beforeProjectClose` is dispatched before the project root is about to change, and `projectOpen` is dispatched afterwards. Also, `projectOpen` is dispatched after the initial project is opened when Brackets first launches.

New Extensibility APIs
----------------------
* Extension style sheet loading: Use `ExtensionUtils.loadStyleSheet(module, path)` to load a style sheet relative to the extension's module.
* RequireJS text plugin: Load content from a text file using `require("text!path/to/file")`. This is useful for loading templates (we don't have a template library built in yet, but you can include one in your extension).

New Utility APIs
----------------
* Unit testing: `waitsForDone()` function for using Promise/Deferred ([see forum post](https://groups.google.com/forum/?fromgroups#!topic/brackets-dev/Y2RrDLv5DPI))
* Unit testing: `SpecRunnerUtils.deleteFile()`
* JSUtils: `JSUtils.findMatchingFunctions(functionName, fileInfos)` For a set of files, returns function offsets for function declarations with a matching name.

Bugs fixed in Sprint 10
----------------------
See [closed sprint 10 bugs](https://github.com/adobe/brackets/issues?labels=sprint+10&page=1&state=closed). A few other bugs might have been fixed that weren't tagged.