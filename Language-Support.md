## Refactoring LESS support into an extension

_See branch dk/less-refactoring_

- Idea: rename LESS to MORE and add an extension that adds support for MORE - this way we can leave current LESS support intact and freely experiment with API designs
- Added an extension that just requires the CodeMirror mode  
  Result: Called EditorUtils.js _getModeFromFileExtensions with an unhandled file extension: more (EditorUtils.js:163)
- **Task:** Add an API to define a language (Name, file extensions, CodeMirror mode name, possibly CodeMirror mode implementation) - [Trello](https://trello.com/card/api-for-extensions-to-add-new-language-syntax-coloring-mode/4f90a6d98f77505d7940ce88/639)
- **Smell:** CodeMirror.defineMode doesn't check if the mode is already defined, so extensions could override existing modes and cause problems for features that rely on specific tokens, like auto complete.
- Monkey-patched getModeFromFileExtensions to use file extension as mode name if such a mode exists
- **Task:** Load (language) extensions earlier to support their CodeMirror modes for the file automatically opened at startup (last open file)
- Added a function that changes the mode of current editors if (after adding the new mode) a different one would be used when re-opening the editor
- **Task:** Allow extensions to define how to add comments for a specific CodeMirror mode **(in progress)**  
- Monkey patching not easily possible due to references to internal functions.
- **Smell:** A difficulty when making comments more generic is that we rely on the defined comment symbols to be completely contained in one CodeMirror token. I.e. we cannot define "//~" as the prefix for line comments (like [SciTE](http://www.scintilla.org/SciTE.html) does) because it is not a prefix of the "//" token.

## Things to keep in mind

_From the [Extensibility Proposal](https://zerowing.corp.adobe.com/display/brackets/Extensibility+Proposal)_

Features we think should be able to be built as plug-ins:

- Access to code model
- Quick open / go to symbol
- Index files/code (background process)
- Better code hinting for "supported" languages (e.g. for a particular framework)
- JSLint tools
- Inline editor providers (e.g. CSS gradient editor)
- Language-specific search and replace (e.g. HTML tag search and replace)
- Code cleanup / refactoring tools