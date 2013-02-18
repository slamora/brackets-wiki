## Introduction
Support for languages like HTML, JavaScript and CSS is currently a core part of Brackets. Support on the same level cannot easily be added by extensions. This page documents our efforts to change that.

## Ongoing work
- Introduced a new high-level concept "language" and refactored support for LESS based on that ([Pull Request](https://github.com/adobe/brackets/pull/2844))

## Caveats
* A difficulty when making comments more generic is that we rely on the defined comment symbols to be completely contained in one CodeMirror token. I.e. we cannot define "//~" as the prefix for line comments (like [SciTE](http://www.scintilla.org/SciTE.html) does) because it is not a prefix of the "//" token.
* We should potentially add a central point to extract file extensions for a given file name to support extensions with multiple parts (i.e. ".html.erb"). Then file names wouldn't have just one potential extension, since file names like "1. Introduction.html" (also multiple dots) would also need to be supported.

## The language concept

* A language has an ID (i.e. "cpp", "cs") for computers (variables, object keys, file names, etc.)
* A language has a name (i.e. "C++", "C#") for humans (displayed in the status bar)
* A language can have a list of file extensions
* A language can have a prefix for line comments (i.e. "//")
* A language can have a prefix and a suffix for block comments (i.e. "/*" and "*/")
* A language can refer to one main CodeMirror mode and multiple mode aliases

## Places contributing to current language support
Based on [LESS Refactoring](https://github.com/adobe/brackets/pull/2844)

### Module document/DocumentCommandHandlers

* Method `_handleNewItemInProject` hardcodes ".js"/"Untitled.js" as the default file extension/name for new files. **Should maybe be a per-project setting.**

### Module document/DocumentManager

* `Document.getLanguage` uses the language API to determine the language based on the file extension.

### Module editor/CodeHintManager

* Method `registerHintProvider` **registers hint providers by mode**.

### Module editor/CSSInlineEditor

* Inline editor provider `htmlToCSSProvider` **decides to open based on the editor mode**. In addition it **relies on HTMLUtils and CSSUtils**

### Module editor/Editor.js

* Method `_checkElectricChars` adjusts indentation when blocks are ended. **The characters to detect block boundaries are hard-coded - `]`, `{`, `}` and `)`**

### Module language/CSSUtils

* Method `findMatchingRules` to find CSS rules matching a selector. Searches an HTML document via language/HTMLUtils if it is the current full editor's document. Also searches CSS files as indexed by project/FileIndexManager in the css index, therefore currently indirectly **uses file extensions**
* Method `extractAllSelectors` extracts CSS selectors from a string. Internally uses CodeMirror's css mode as a parser, but that could be swapped out.
* Method `findSelectorAtDocumentPos` to find the selector(s) of a CSS block, directly **uses tokens provided by CodeMirror**
* Method `getInfoAtPos` to provide a context info object for the given cursor position, directly **uses tokens provided by CodeMirror**

### Module language/HTMLUtils

* Method `findStyleBlocks` to gather info about all `<style>` blocks in an HTML document, directly **uses tokens provided by CodeMirror** 

### Module language/Languages

* Defines the "language" concept
* Loads default languages from `language/languages.json`
* Method `defineLanguage` to add a new language (see JSDoc)
* Method `getLanguage` to get an object representing a language by its ID
* Method `getLanguageForFileExtension` to map file extensions to languages
* Method `getLanguageForMode` to map CodeMirror modes to languages
* Used by extension "LESSSupport" to add basic support for LESS

### Module language/JSLintUtils

* Method `run` to run JSLint on the current document. Checks if the extension is one of .js, .htm or .html, therefore **uses file extensions**. Otherwise uses JSLint internally which could be swapped out.

### Module language/JSUtils

* Method `findAllMatchingFunctionsInText` to find all instances of a function name in a string of code. Internally uses CodeMirror's javascript mode as a parser, but that could be swapped out.
* Method `findMatchingFunctions` to find all functions with a specified name within a set of files. Only uses files ending with ".js", i.e. **uses file extensions**

### Module project/FileIndexManager

* Maintains an index called "css" using only files ending with ".css", i.e. **uses file extensions**

### Module search/QuickOpen

* Method `addQuickOpenPlugin`
* **Uses file extensions**
* Used by extensions "QuickOpenCSS", "QuickOpenHTML" and "QuickOpenJavaScript"
* Extension "QuickOpenCSS" uses module language/CSSUtils


## Notes

### Features we think should be able to be built as plug-ins
_From the [Extensibility Proposal](https://zerowing.corp.adobe.com/display/brackets/Extensibility+Proposal)_
- Access to code model
- Quick open / go to symbol
- Index files/code (background process)
- Better code hinting for "supported" languages (e.g. for a particular framework)
- JSLint tools
- Inline editor providers (e.g. CSS gradient editor)
- Language-specific search and replace (e.g. HTML tag search and replace)
- Code cleanup / refactoring tools

### LiveDevelopment considerations
* What hooks would need to be added where to allow extension-based language support?  
  _LiveDevelopment.registerDocumentClass(...)_
* What behavior would need to be configurable by extensions?  
  _turning off auto-reload for LESS files, configuring CodeMirror_
* What deployment scenarios do we want to support?  
  _Dynamic client-side, dynamic server-side, static server-side?_
* Do we need to integrate with 3rd-party build tools?  
  _I.e. run ant/cake/... when changing a LESS file to initiate compliation to CSS_
* Do we need a specification to delegate compilation to an existing server?  
  _The server might use a compiler written in a language other than JS, or might use a JS compiler with specific options that can't be easily extracted_
* What can we support without impacting performance too much?  
  _Running a build tool for every character entered will not work_
* How should the user configure Brackets to support server-side compilation?  
  _What file needs to be compiled how when changed - setting for the entire project or a subdirectory? Exceptions for single files?_
* How do we deal with distributed files?  
  _LESS supports including other LESS files, so changes to these should trigger compilation of the master file_