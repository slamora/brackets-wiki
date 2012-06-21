**Sprint 10 is still in progress.** These are _**draft**_ notes.

What's new in Sprint 10
----------------------
JavaScript Quick Edit is now an official extension. Within HTML or JavaScript files, CMD+E/CTRL+E on a function name will open all function definitions with that name in an inline editor.

Our main goals for the sprint were similar to the previous sprint - Continue to make progress on the extensibility architecture of Brackets: *research, basic architecture and extensibility API*:
* **Architecture / Contributions**
     * CodeMirror contributions (NJ)     
This [Story](https://trello.com/c/XxKi7w8x) is about submitting scrolling enhancements to CodeMirror, there were a number of pull requests related to fix flickering during vertical scrolling in CodeMirror, further details can be found here:
         * [#534](https://github.com/marijnh/CodeMirror2/pull/534)
         * [#551](https://github.com/marijnh/CodeMirror2/pull/551)
     
     * Build experimental CEF3 Shell (Glenn, Raymond)    
The [Story](https://trello.com/c/8Vuom2dA) indicates work in progress, for example there hasn't been a lot of scenario testing yet. The changes are manifesting in the performance improvements and better responsiveness running in the multi-process modes.     

* **Extensibility Epic**     
      * The [JavaScript Quick Edit] (https://trello.com/c/8d7sdB54) story (NJ, Jason, Randy, Peter, Joel) allows you to quickly access and edit JavaScript function definitions from a function reference within HTML and JavaScript files. Performance instrumentation was added to help the team optimize the JavaScript file parsing. Performance test reporting can now display metrics as a hierarchy to show how individual metrics roll-up. [Unit Tests](https://github.com/adobe/brackets/wiki/Extension-Experiments#wiki-unittests).    
      * The second extensibility [Story - Extension support for require.js text plugin](https://trello.com/c/BKQnEDRa) (Jason) in Sprint 10 enables extension developer to use the [require.js text plugin] (https://github.com/requirejs/text) without having to include it for each extension.    
      * We now support [HTML Context Menus](https://trello.com/c/Um2Nlhh9) (Ty, Randy, Peter) in Brackets - please also refer to the Menu API changes mentioned below.
      * We started working on [basic guidelines](https://github.com/adobe/brackets/wiki/Extension-UI-Guidelines) for Extensions UI ([User Story])(https://trello.com/c/tDdqua2R) (Garth)
      * [Extension Stylesheets](https://trello.com/c/ltSP2dcY) (Jason) Extensions may optionally specify CSS stylesheets to load ``ExtensionUtils.loadStyleSheet()``. See [Extension UI Guidelines](https://github.com/adobe/brackets/wiki/Extension-UI-Guidelines) for best practices.


Major external contributions
----------------------------
* [cantrell](http://github.com/cantrell) added Restore Default Font Size (CMD+0/CTRL+0)

UI Changes
----------
* "Debug > Enable JSLint" has moved to "View > Enable JSLint"
* "Debug > Use Tab Characters" has moved to "Edit > Use Tab Characters"

Menu API Changes
----------------
The following functions no longer take an ID parameter:

addMenuItem()
addMenuDivider()

Also, the relativeID parameter has changed. Previously this parameter specified the id of another MenuItem. Now it references the id of a Command that exists in the parent menu to specify a location. The net result is that MenuItems no longer have externally exposed ID’s.

These changes simplify the API, but will break existing extensions that add menus, so please update your Brackets extensions with this change.

Document/Editor API Changes
---------------------------
Mutator APIs have been added to Document. To modify text, use these Document APIs instead of going through Editor or CodeMirror. For details on Document usage, see "Working with Documents" in the [[Brackets Development How Tos]].

* Added to Document: replaceRange(), getRange(), getLine(), batchOperation().
* Changed Document.getText() so it always returns \n line endings (like the above new APIs) unless a special flag is passed.
* Removed Editor.getLineText() -- replaced by Document.getLine()

Project API Changes
-------------------
The events dispatched by ProjectManager have changed. Instead of dispatching `initializeComplete` and `projectRootChanged`, we now dispatch `projectOpen` and `beforeProjectClose` events. `beforeProjectClose` is dispatched before the project root is about to change, and `projectOpen` is dispatched afterwards. Also, `projectOpen` is dispatched after the initial project is opened when Brackets first launches.

Extensibility API Changes
-------------------------
* Extension style sheet loading: ``ExtensionUtils.loadStyleSheet(module, path)`` to load a style sheet relative to the extension's module
* RequireJS text plugin: Load content from a text file ``require("text!path/to/file")``

New Utility APIs
----------------
* Unit testing: waitsForDone() function for using Promise/Deferred ([see forum post](https://groups.google.com/forum/?fromgroups#!topic/brackets-dev/Y2RrDLv5DPI))
* Unit testing: SpecRunnerUtils.deleteFile()
* JSUtils: ``JSUtils.findMatchingFunctions(functionName, fileInfos)`` For a set of files, returns function offsets for function declarations with a matching name.

Bugs fixed in Sprint 10
----------------------
See [closed sprint 10 bugs](https://github.com/adobe/brackets/issues?labels=sprint+10&page=1&state=closed). A few other bugs might have been fixed that weren't tagged.