## Introduction
Support for languages like HTML, JavaScript and CSS is currently a core part of Brackets. Support for other languages on the same level cannot easily be added by extensions. This page documents our efforts to change that.

## TL;DR

In an extension, if a language has an existing CodeMirror mode, you can declare the new language in a simple JSON object:

```
var LanguageManager = brackets.getModule("language/LanguageManager");

LanguageManager.defineLanguage("shell", {
    name: "Shell",
    mode: "shell",
    fileExtensions: ["sh"],
    lineComment: "#"
});
```

If you need to provide a custom mode, it must be (registered to CodeMirror)[http://codemirror.net/doc/manual.html#modeapi] using ``CodeMirror.defineMode()`` first before calling ``LanguageManager.defineLanguage()``.

## New LanguageManager Module and Language API
- The Sprint 21 release introduced a new high-level "language" concept and refactored support for LESS based on that ([Pull Request](https://github.com/adobe/brackets/pull/2844))

## The Language Concept

* A language has an ID (i.e. "cpp", "cs") for computers (variables, object keys, file names, etc.)
* A language has a name (i.e. "C++", "C#") for humans (displayed in the status bar)
* A language can have a list of file extensions
* A language can have a prefix for line comments (i.e. "//")
* A language can have a prefix and a suffix for block comments (i.e. "/*" and "*/")
* A language can refer to a CodeMirror mode for parsing, tokenization, indentation and syntax highlighting

### Goals

Our goal is to extend languages as the primary mechanism to extend other core features of Brackets such as code hinting, text manipulation commands, quick edit, live preview, etc.

## LanguageManager Module

The ``LanguageManager`` modules can be accessed in an extension: ``brackets.getModule("language/LanguageManager")``.

It defines three methods for managing languages:

* ``defineLanguage(id, definition)``
* ``getLanguage(id)``
* ``getLanguageForFileExtension(path)``

## Language API

A ``Language`` ...

* ``getName()``
* ``getMode()``
* ``getFileExtensions()``
* ``hasLineCommentSyntax()/getLineCommentSyntax()/setLineCommentSyntax(prefix)``
* ``hasBlockCommentSyntax()/getBlockCommentPrefix()/getBlockCommentSuffix()/setLineCommentSyntax(prefix)``
* ``getLanguageForMode()``

## Places contributing to "language" support as of Sprint 21
Based on [LESS Refactoring](https://github.com/adobe/brackets/pull/2844)

### Module brackets

* Loads a few selected submodules that it doesn't use, including `CSSInlineEditor`, `JSUtils`, and `JSLintUtils`.
* Function `_initTest` loads a few more

### Module document/DocumentCommandHandlers

* Method `_handleNewItemInProject` hardcodes ".js"/"Untitled.js" as the default file extension/name for new files. **Should maybe be a per-project setting.** See also [Card #291](https://trello.com/c/WUdhIRlh)

### Module document/DocumentManager

* `Document.getLanguage` uses the language API to determine the language based on the file extension.

### Module editor/CodeHintManager

* Method `registerHintProvider` **registers hint providers by mode**.

### Module editor/CSSInlineEditor

* Inline editor provider `htmlToCSSProvider` **decides to open based on the editor mode**. In addition it **relies on HTMLUtils and CSSUtils**

### Module editor/Editor.js

* Method `_checkElectricChars` adjusts indentation when blocks are ended. **The characters to detect block boundaries are hard-coded - `]`, `{`, `}` and `)`**

### Module editor/EditorCommandHandlers

* Functions `_findCommentStart`, `_findCommentEnd` and `_findNextBlockComment` **use tokens provided by CodeMirror** to search for comment boundaries. While the strings they search are provided by a language definition, this prevents us from defining "//~" as the prefix for line comments (like [SciTE](http://www.scintilla.org/SciTE.html) does) because it is not a prefix of the "//" token.
* Methods `blockCommentPrefixSuffix` and `lineCommentPrefixSuffix` have similar constrains as they navigate by tokens instead of characters. In addition they check whether a token's `className` is different from `"comment"`. Therefore they **use tokens provided by CodeMirror**.

### Module file/FileUtils.js

* Methods `isStaticHtmlFileExt` and `isServerHtmlFileExt` **use hardcoded lists of file extensions**

### Module language/CSSUtils

* Method `findMatchingRules` to find CSS rules matching a selector. Searches an HTML document via language/HTMLUtils if it is the current full editor's document. Also searches CSS files as indexed by project/FileIndexManager in the css index, therefore currently indirectly **uses file extensions**
* Method `extractAllSelectors` extracts CSS selectors from a string. Internally uses CodeMirror's css mode as a parser, but that could be swapped out.
* Method `findSelectorAtDocumentPos` to find the selector(s) of a CSS block, directly **uses tokens provided by CodeMirror**
* Method `getInfoAtPos` to provide a context info object for the given cursor position, directly **uses tokens provided by CodeMirror**

### Module language/HTMLUtils

* Method `findStyleBlocks` to gather info about all `<style>` blocks in an HTML document, directly **uses tokens provided by CodeMirror** 

### Module language/JSLintUtils

* Method `run` to run JSLint on the current document. Checks if the extension is one of .js, .htm or .html, therefore **uses file extensions**. Otherwise uses JSLint internally which could be swapped out.

### Module language/JSUtils

* Method `findAllMatchingFunctionsInText` to find all instances of a function name in a string of code. Internally uses CodeMirror's javascript mode as a parser, but that could be swapped out.
* Method `findMatchingFunctions` to find all functions with a specified name within a set of files. Only uses files ending with ".js", i.e. **uses file extensions**

### Module language/Languages

* Defines the "language" concept
* Loads default languages from `language/languages.json`
* Method `defineLanguage` to add a new language (see JSDoc)
* Method `getLanguage` to get an object representing a language by its ID
* Method `getLanguageForFileExtension` to map file extensions to languages
* Used by extension "LESSSupport" to add basic support for LESS

### Module LiveDevelopment/LiveDevelopment

* **Requires hardcoded list of special documents**, namely {CSS,HTML,JS}Document
* Function `_setDocInfo` **determines a root URL biased towards HTML and HTML extensions**
* Function `_classForDocument` **uses a hardcoded mapping of file extensions to document types**
* Function `_openDocument` **only loads related CSS documents** (no JavaScript, not extensible)
* Function `_onLoad` **excludes CSSDocument from being marked as out of sync** (not extensible)
* Method `open` **reduces LiveDevelopment support to HTML files based on file extensions**
* Method `showHighlight` **only calls doc.updateHighlight() for CSSDocuments**
* Function `_onDocumentChange` **closes LiveDevelopment when switching to an HTML file** and **excludes CSSDocument from being marked as out of sync** (not extensible)
* Function `_onDocumentSaved` **only reloads the page if the document is not a CSSDocument** (not extensible)
* Function `_onDirtyFlagChange` **only updates the LiveDevelopment status if the dirty file is not a CSSDocument** (not extensible)

### Module LiveDevelopment/Agents/DOMHelpers

* Contains multiple methods that encapsulate knowledge about HTML, **should potentially be tied to the language object**

### Module LiveDevelopment/Agents/DOMNode

* DOMNode.prototype.toString contains basic knowledge about HTML, **should potentially be tied to the language object**

### Module LiveDevelopment/Agents/GotoAgent

* **Has hardcoded support for HTML, CSS and JavaScript**

### Module LiveDevelopment/Agents/HighlightAgent

* **Has hardcoded support for HTML and CSS**

### Script LiveDevelopment/Agents/RemoteFunctions

* Function `_typeColor` has **hardcoded distinctions between html, css, js and others**

### Module project/FileIndexManager

* Maintains an index called "css" using only files ending with ".css", i.e. **uses file extensions**

### Module search/QuickOpen

* Method `addQuickOpenPlugin` **uses file extensions** to register plugins

### Module utils/ExtensionUtils

* Method `loadStyleSheet` **uses file extensions** to support LESS files. Note that is only relevant for extension developers, but technically this could use a more general architecture that allows to auto-discover compilers to CSS.

### Module utils/StringUtils

* Method `htmlEscape` **escapes characters with special meaning in HTML**, should this at least be refactored to the language object for HTML?

### Module utils/TokenUtils

* Method `getModeAt` **has a hardcoded special case for XML**. Once the other places are no longer based on this mode, but on the language, this can be removed to just report "xml", which should be mapped to HTML for the language HTML.

## Notes

### Determining file extensions

We should potentially add a central point to extract file extensions for a given file name to support extensions with multiple parts (i.e. ".html.erb"). Then file names wouldn't have just one potential extension, since file names like "1. Introduction.html" (also multiple dots) would also need to be supported.

### Features we think should be able to be built as plug-ins
_From the [Extensibility Proposal](https://zerowing.corp.adobe.com/display/brackets/Extensibility+Proposal)_ (internal)
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