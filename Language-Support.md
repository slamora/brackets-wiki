## Refactoring LESS support into an extension

_See branch dk/less-refactoring_

- Idea: rename LESS to MORE and add an extension that adds support for MORE
- Added an extension that just requires the CodeMirror mode  
  Result: Called EditorUtils.js _getModeFromFileExtensions with an unhandled file extension: more (EditorUtils.js:163)
- **Task:** Add an easy way to map a file extension to a CodeMirror mode
- **Smell:** CodeMirror.defineMode doesn't check if the mode is already defined, so extensions could override existing modes and cause problems for features that rely on specific tokens, like auto complete.
- Monkey-patched getModeFromFileExtensions to use file extension as mode name if such a mode exists
- **Task:** Load (language) extensions earlier to support their CodeMirror modes for the file automatically opened at startup (last open file)
- Added a function that changes the mode of current editors if (after adding the new mode) a different one would be used when re-opening the editor
- **Task:** Allow extensions to define how to add comments for a specific CodeMirror mode **(in progress)**  
- Monkey patching not easily possible due to references to internal functions.
- **Smell:** A difficulty when making comments more generic is that we rely on the defined comment symbols to be completely contained in one CodeMirror token. I.e. we cannot define "//~" as the prefix for line comments (like [SciTE](http://www.scintilla.org/SciTE.html) does) because it is not a prefix of the "//" token.