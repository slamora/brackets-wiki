## Introduction
Support for languages like HTML, JavaScript and CSS is currently a core part of Brackets. Support for other languages, before Sprint 21, could not easily be added by extensions. This page documents language support (added in Sprint 21) to allow extensions to define languages without modifying core Brackets modules.

## TL;DR

In an extension, if a language has an [existing CodeMirror mode](http://codemirror.net/doc/modes.html), you can declare the new language in a simple JSON object:

```
var LanguageManager = brackets.getModule("language/LanguageManager");

var language = LanguageManager.defineLanguage("haskell", {
    name: "Haskell",
    mode: "haskell",
    fileExtensions: ["hs"],
    blockComment: ["{-", "-}"],
    lineComment: "--"
});
```

If you need to provide a custom mode, it must be [registered to CodeMirror](http://codemirror.net/doc/manual.html#modeapi) using ``CodeMirror.defineMode()`` first before calling ``LanguageManager.defineLanguage()``.

To inspect the language of the current ``Document`` call ``document.getLanguage()``. To inspect the language of the current selection in an ``Editor``, use ``editor.getLanguageForSelection()``. This language in a selection may not be the same as the language for the document. For example, the ``html`` language supports ``css`` and ``javascript`` content.

## New LanguageManager Module and Language API
- The Sprint 21 release introduced a new high-level "language" concept and refactored support for LESS based on that ([Pull Request](https://github.com/adobe/brackets/pull/2844))

## The Language Concept

* A language has an ID (i.e. "cpp", "cs") for computers (variables, object keys, file names, etc.)
* A language has a name (i.e. "C++", "C#") for humans (displayed in the status bar)
* A language can have a list of file extensions (e.g. "svg", "cpp", "html.erb", "coffee.md")
* A language can have a list of file names (e.g. "Makefile", ".profile")
* A language can have one or more prefixes for line comments (i.e. "//")
* A language can have one or more sets of a prefix and a suffix for block comments (i.e. "/*" and "*/")
* A language can refer to a CodeMirror mode for parsing, tokenization, indentation and syntax highlighting

### Goals

Our goal is to extend languages as the primary mechanism to extend other core features of Brackets such as code hinting, text manipulation commands, quick edit, live preview, etc.

## LanguageManager Module

The ``LanguageManager`` modules can be accessed in an extension: ``brackets.getModule("language/LanguageManager")``.

It defines three methods for managing languages:

* ``defineLanguage(id, definition)`` Defines a language. returns A promise object that will be resolved with a Language object.
    * ``id`` Unique identifier for this language, use only letters a-z, numbers and _ inbetween (i.e. "cpp", "foo_bar")
    * ``definition`` An object describing the language
    * ``definition.name`` Human-readable name of the language, as it's commonly referred to (i.e. "C++")
    * ``definition.fileExtensions`` List of file extensions used by this language (e.g ["php", "php3"])
    * ``definition.lineComment`` Line comment prefix (i.e. "//")
    * ``definition.blockComment`` Array with two entries defining the block comment prefix and suffix (e.g ["<!--", "-->"])
    * ``definition.mode`` CodeMirror mode (i.e. "htmlmixed"), optionally with a MIME mode defined by that mode ["clike", "text/x-c++src"] unless the mode is located in thirdparty/CodeMirror2/mode/<name>/<name>.js, you need to first load it yourself.
* ``getLanguage(id)`` Resolves a language ID to a Language object.
* ``getLanguageForPath(path)`` Resolves a file path to a Language object.

## Language API

``Language`` contains model data for a language.

* ``getName()`` Returns the human-readable name of this language.
* ``getMode()`` Returns the CodeMirror mode for this language.
* ``getFileExtensions()`` Returns an array of file extensions for this language.
* ``hasLineCommentSyntax()/getLineCommentSyntax()/setLineCommentSyntax(prefix)`` Line comment info used for toggle line comment editor command.
* ``hasBlockCommentSyntax()/getBlockCommentPrefix()/getBlockCommentSuffix()/setLineCommentSyntax(prefix)`` Block comment info used for toggle block comment editor command.
* ``getLanguageForMode()`` Returns either a language associated with the mode or the fallback language. Used to disambiguate modes used by multiple languages.

## Places contributing to "language" support as of Sprint 21
Based on [LESS Refactoring](https://github.com/adobe/brackets/pull/2844). The remainder of this page documents future work required to support languages across various features in Brackets. The focus is on finding out what is hardcoded and needs to be refactored to use existing capabilities, or requires new capabilities before it can be refactored.

### No changes required

These are okay the way the are.

* document/DocumentManager.js
    * `Document.getLanguage` uses the LanguageManager to determine the language based on the file extension.
* language/CSSUtils.js
    * Method `extractAllSelectors` extracts CSS selectors from a string. Internally uses CodeMirror's css mode as a parser, but that could be swapped out.
* language/JSLintUtils.js (now extensions/default/JSLint/main.js)
    * Uses JSLint internally which could be swapped out.
* language/JSUtils.js
    * Method `findAllMatchingFunctionsInText` to find all instances of a function name in a string of code. Internally uses CodeMirror's javascript mode as a parser, but that could be swapped out.
* language/LanguageManager.js
    * Defines the "language" concept
    * Loads default languages from `language/languages.json`
    * Method `defineLanguage` to add a new language (see JSDoc)
    * Method `getLanguage` to get an object representing a language by its ID
    * Method `getLanguageForPath` to map file names or extensions to languages
    * Used by extension "LESSSupport" to add basic support for LESS
* utils/StringUtils.js
    * Method `htmlEscape` escapes characters with special meaning in HTML. However, this function is necessary since the Brackets UI is written in HTML, and has nothing to do with language support for the users.

### Straightforward refactoring

These need to be changed to use existing functionality.

* __Done:__ brackets.js __requires language/JSLintUtils.js__. This can be refactored into an extension without introducing new APIs. See [issue #3094](https://github.com/adobe/brackets/issues/3094) and [pull request #3143](https://github.com/adobe/brackets/pull/3143).
* brackets.js __requires editor/CSSInlineEditor.js__. It should first call __require("editor/MultiRangeInlineEditor")__ (loaded by CSSInlineEditor.js), since this defines shortcuts for inline editor navigation. Then the CSSInlineEditor could be moved to an extension.
* editor/CodeHintManager.js
    * _In progress:_ Method `registerHintProvider` **registers hint providers by mode**. This can simply be changed to check for language IDs since currently all modes this function is being called with (either by Brackets or the known extensions) belong to a language with an equal ID ("css", "html", "javascript"). See [issue #3085](https://github.com/adobe/brackets/issues/3085) and [pull request #3270](https://github.com/adobe/brackets/pull/3270).
* __Done:__ editor/CSSInlineEditor.js
    * Inline editor provider `htmlToCSSProvider` **decides to open based on the editor mode**. This can simply be changed to check for the language ID to be "html" (via `editor.getLanguageForSelection().getId()`).
* project/FileIndexManager.js. See [pull request #3301](https://github.com/adobe/brackets/pull/3301).
    * Maintains an index called "css" using only files ending with ".css", i.e. **uses file extensions**. The call to add this index should be moved to CSSUtils (the only place this index is used at the moment). In addition, the filter function can be changed to use the language API: `return LanguageManager.getLanguageForPath(entry.name).getId() === "css";`
* __Done:__ language/JSLintUtils.js
    * Method `run` to run JSLint on the current document. Checks if the extension is .js, therefore **uses file extensions**. See [issue #3094](https://github.com/adobe/brackets/issues/3094) and [pull request #3143](https://github.com/adobe/brackets/pull/3143).
* language/JSUtils.js
    * Method `findMatchingFunctions` finds all functions with a specified name within a set of files. Filters these files by checking that the file extension is ".js", i.e. **uses file extensions**. This should use the language API instead (determine the language for the file and check whether that language has the ID "javascript").
* __Done:__ search/QuickOpen
    * Method `addQuickOpenPlugin` **uses file extensions** to register plugins. It should use language IDs instead. It is currently only used with file extensions "css", "js" and "html". For "css" and "html", the calling code can remain unchanged, transparently changing the meaning of the string from file extension to language ID. For "js", the calling code needs to use "javascript" instead. Currently `extensions/default/QuickOpenJavaScript/main.js` is the only place in either Brackets core or the extensions that uses this file extension. See [pull request #3301](https://github.com/adobe/brackets/pull/3301).

### Issues that should be adressed as part of other planned work

These are places that affect areas we already have plans to work on, and where issues are best adressed as part of that work.

* document/DocumentCommandHandlers.js
    * Method `_handleNewItemInProject` hardcodes ".js"/"Untitled.js" as the default file extension/name for new files. Changing this to work as proposed in [card #291](https://trello.com/c/WUdhIRlh) would remove this issue.
* language/{CSSUtils|HTMLUtils|JSUtils}.js should be provided by default extensions. For this to work, all other parts that depend on them (ideally only other extensions) need to be able to access these extensions. Supporting this is part of the [ongoing extensions research](https://github.com/adobe/brackets/wiki/Extensions2).
    * brackets.js __loads JSUtils.js__. This is only necessary so extensions can load it synchronously via `JSUtils = brackets.getModule("language/JSUtils")` instead of asynchronously via `brackets.getModule(["language/JSUtils"], function (JSUtils) { ... })`. This can be removed once JSUtils can be loaded as an extension.
    * brackets.js __exports CSSUtils and JSUtils for tests__. Tests should instead load these modules from extensions, but this depends on the point above.
    * editor/CSSInlineEditor.js __relies on HTMLUtils and CSSUtils__.
    * LiveDevelopment/Agents/DOMHelpers.js contains multiple methods that encapsulate knowledge about HTML, **should potentially be moved to HTMLUtils**
    * LiveDevelopment/Agents/DOMNode.js contains DOMNode.prototype.toString contains basic knowledge about HTML, **should potentially be HTMLUtils**
* utils/ExtensionUtils
    * Method `loadStyleSheet` **uses file extensions** to support LESS files. Once we have a compiler infrastructure in place, any path could be mapped to a language, and if there's a compiler to CSS for that language, it should be used. Note that is only relevant for extension developers.
* utils/TokenUtils
    * Method `getModeAt` **has a hardcoded special case for XML**. Once the other places are no longer based on this mode, but on the language, this can be removed to just report "xml". For HTML documents, the language manager maps the "xml" mode to the HTML language. XML documents are not affected by this. See [issue #2965](https://github.com/adobe/brackets/issues/2965) for a related discussion.

### Code that relies on the current editor state

These places currently access CodeMirror's state directly and are therefore not usable without an active editor. They might benefit from doing their own parsing, possibly using CodeMirror modes as parsers. CodeMirror's editor state could still optionally be used for optimization, but nothing else.

* editor/EditorCommandHandlers.js
    * Functions `_findCommentStart`, `_findCommentEnd` and `_findNextBlockComment` **use tokens provided by CodeMirror** to search for comment boundaries. While the strings they search are provided by a language definition, this prevents us from defining arbitrary comment symbols. One example is "//~" as the prefix for line comments (as [SciTE](http://www.scintilla.org/SciTE.html) does). Adding a comment this way is possible, but removing it does not work because "//~" is not a prefix of the CodeMirror token for "//".
    * Methods `blockCommentPrefixSuffix` and `lineCommentPrefixSuffix` have similar constraints as they navigate by tokens instead of characters. In addition they check whether a token's `className` is different from `"comment"`. Therefore they **use tokens provided by CodeMirror**.
* language/CSSUtils.js
    * Method `findMatchingRules` to find CSS rules matching a selector. Searches an HTML document via language/HTMLUtils __if it is the current full editor's document__.
    * Method `findSelectorAtDocumentPos` to find the selector(s) of a CSS block, directly **uses tokens provided by CodeMirror**
    * Method `getInfoAtPos` to provide a context info object for the given cursor position, directly **uses tokens provided by CodeMirror**
* language/HTMLUtils.js
    * Method `findStyleBlocks` to gather info about all `<style>` blocks in an HTML document, directly **uses tokens provided by CodeMirror** 

### Providing code semantics

This relates to places that require information about the semantics of code beyond what is provided by CodeMirror.

* editor/Editor.js
    * Method `_checkElectricChars` adjusts the current line's indentation when blocks are ended. **The characters to detect block boundaries are hard-coded - `]`, `{`, `}` and `)`.** The function is supposed to replace CodeMirror's own implementation, citing [bugs](https://github.com/adobe/brackets/pull/250). In contrast to `_checkElectricChars`, CodeMirror's own implementation does not re-indent the line after typing "]" in a JavaScript file, so for now we cannot remove our own implementation without removing existing functionality.

__New API:__ Allow adding electric __strings__ to language definitions. Some languages have block boundaries that are not just one char long, for instance Ruby's `begin ... end`.

### Starting Live Preview

These areas are concerned with what files live preview can be started with. This also affects Brackets' behavior when switching between files while live preview is active.

* file/FileUtils.js
    * Methods `isStaticHtmlFileExt` and `isServerHtmlFileExt` __use hardcoded lists of file extensions__ to determine which file extensions are okay to open statically and which require a base URL to work. Switching to a file matching these criteria causes live preview to show that file instead unless it is included in the currently displayed file.
* LiveDevelopment/LiveDevelopment.js
    * Method `open` **reduces LiveDevelopment support to HTML files based on file extensions**
    * Function `_onDocumentChange` **closes LiveDevelopment when switching to a different, not included HTML file**

In general the possibility of live development for a given file depends on its format, the formats we can turn it into and the formats a client supports.

Consequently we need ways to define what formats are supported by which client, and what formats the project's files are available in.
Right now the only supported client is Chrome. We want to extend this to other browsers, and may eventually want to extend this to different types of clients, like Node.js, or a PDF viewer to preview LaTeX files.

__languages.json:__

    {
        "html": {
            "name": "HTML",
            "mimeTypes": ["text/html"],
            "fileExtensions": ["html"]
        }
    }

__clients.json:__

    {
        "chrome": {
            "mimeTypes": ["text/html", "application/xhtml+xml", "application/xml"]
        }
    }

If the user opens "index.html" and clicks the live preview button, Brackets would know that Chrome supports this file and allow the live preview.

If the user provides a base URL, opens "index.php" and clicks the live preview button, Brackets could send a HEAD request for index.php to the server. If the returned MIME type is "text/html", Brackets would know that Chrome supports this format and allow the live preview. Otherwise, the live preview will not be enabled.

This way, we would not need to maintain a list of file extensions that may or may not generate content in a format supported by Chrome.

__Extension:__

    var ClientManager = brackets.getModule("LiveDevelopment/ClientManager"),
        chrome = ClientManager->getClient("chrome");
    chrome.addMimeType("image/svg+xml");

    var LanguageManager = brackets.getModule("language/LanguageManager"),
        svg = LanguageManager.getLanguage("svg");
    svg.addMimeType("image/svg+xml");

If the user opens "index.svg" and clicks the live preview button, Brackets would know that Chrome supports this file and allow the live preview. This would also work if index.php generates SVG code, provided it sends the proper MIME type. SVG is an interesting use case since Brackets would right now provide live development with SVG if only its extension were listed in the `_staticHtmlFileExts` array. Brackets would show XML code, the browser would show the rendered image, Brackets would reload the browser when saving the file, and even refresh styles as they are changed if the SVG file links to external stylesheets (`<?xml-stylesheet type="text/css" href="style.css" ?>` before the opening `<svg>` tag).

__Extension:__

    var LanguageManager = brackets.getModule("language/LanguageManager"),
        md = LanguageManager.getLanguage("markdown");
    md.addCompiler("html", function (doc) {
        ...
        return htmlCode;
    });

If the user opens "index.md" and clicks the live preview button, Brackets would know that it could compile this code to HTML. It would look up HTML's MIME type and see that Chrome could open the compiled file. It would then use the first file extension defined for the HTML language and create a temporary file - say "file.md.html" - with the return value of the compiler function as its contents. Finally, Brackets would start a live preview on the compiled file. On a related note, [card 565](https://trello.com/c/4BHJSfzo) introduces the idea of HTML rewriting.

__Extension:__

    var ClientManager   = brackets.getModule("LiveDevelopment/ClientManager"),
        LanguageManager = brackets.getModule("language/LanguageManager"),
        $preview        = ...,
        sync;

    var client = ClientManager.addClient("preview", {
        open: function (doc) {
            sync = function () {
                var compile = doc.getLanguage().getDefaultCompilerToLanguage("html");
                $preview.html(compile(doc.getText()));
            };

            $(doc).on("change", sync);
            sync();
            
            // Add preview frame to Brackets
            $preview.appendTo(...);
        },
        close: function () {
            // Close preview frame
            $preview.remove();
            
            $(doc).off("change", sync);
            sync = null;
        }
    });

    // Mark this client as compatible with every language that has an HTML compiler
    function considerLanguage(language) {
        if (language.getDefaultCompilerToLanguage("html")) {
            // Indirectly define MIME types via a language to make synchronization unnecessary
            // when adding MIME types to the language 
            client.addLanguage(language);
        }
    }

    // Consider new or modified languages
    $(LanguageManager).on("languageAdded languageModified", function (e, language) {
        considerLanguage(language);
    });

    // Consider existing languages
    Array.forEach(LanguageManager.getLanguages(), considerLanguage);

### Updating Live Preview

These areas are concerned with making sure that the live preview is up to date. This is needed if the main document or included files are changed.

* LiveDevelopment/LiveDevelopment.js
    * **Requires hardcoded list of special documents**, namely {CSS,HTML,JS}Document. These function as updaters and highlighters.
    * Function `_classForDocument` **uses a hardcoded mapping of file extensions to document types**
    * Function `_openDocument` **only loads related CSS documents** (no JavaScript, not extensible)
    * Function `_onLoad` **excludes CSSDocument from being marked as out of sync** (not extensible)
    * Function `_onDocumentChange` **excludes CSSDocument from being marked as out of sync** (not extensible)
    * Function `_onDocumentSaved` **only reloads the page if the document is not a CSSDocument** (not extensible)
    * Function `_onDirtyFlagChange` **only updates the LiveDevelopment status if the dirty file is not a CSSDocument** (not extensible)

For live development on _save_, the client only needs to reveal that the saved file was requested by the document. This allows Brackets to reload the document if an included file is saved. For Chrome, the network agent handles this task by listening to the "Network.requestWillBeSent" event.

For live development on _change_, the client needs to provide ways to directly inject the new content into the document. In Brackets, we need to be aware of such methods and use them instead of reloading the whole document. Extensions therefore need a way to register an updater. We currently provide this in a hard-coded way for CSS, and for HTML and JavaScript files if `LiveDevelopment.config.experimental` is true.

The following scenarios showcase what needs to change, using LESS as an example.

#### External Precompilation

In this scenario, the LESS file is converted to a static CSS file by an external tool. This tool might monitor the LESS file for changes, or the user could run it manually. The resulting static CSS file is delivered to the browser in the usual way. Therefore, it can also be updated like normal CSS files.

To support this, we would need to detect external changes to included CSS files even if the corresponding CSS file is not open in Brackets. Such an external change would then need to be treated like the user saving the file.

Updating on change is not possible in this scenario since the external tool only has access to the contents of the file through the file system.

#### Internal Precompilation

In this scenario, the LESS file is converted to a static CSS file by Brackets. The resulting static CSS file is delivered to the browser in the usual way. Therefore, it can also be updated like normal CSS files.

To support this, the user would need to be able to turn this behavior on, possibly for individual files only since LESS files can include other LESS files, and typically only the master file should be converted.

However, the [BracketsLESS extension](https://github.com/olsgreen/BracketLESS) adds a menu entry to turn compilation on for all LESS files. Compilation occurs when the file is saved. Sadly, Brackets currently doesn't detect that the CSS file was changed, not even when switching to it from within Brackets after saving the LESS file. Switching to another application and back to Brackets will update the CSS file in the browser if at one point a Document object has been created for the CSS file (for instance by opening the CSS file within Brackets). There also is no straightforward way to create the file via the Document API in the first place. Even if the file already exists (and the Document object can therefore be created), there is no `doc.save()` method to call since all writing is apparently done via editors (presumed to be user visible, which may be incorrect). Clearly, programmatic changes to documents need to be made easier. For now, saving the file manually and calling `$(window).triggerHandler("focus");` is the most convenient way to make Brackets aware of the changed file so the preview is updated accordingly.

Ideally, this extension could simply open a Document object with the target path and replicate all changes of the LESS file to the CSS file. If the LESS file is changed, `cssDoc.setText(cssCode)` is called, and the live preview is updated as if the user had typed these changes into the CSS file manually. If the LESS file is saved, so is the CSS file, similarly with deleting the LESS file.

#### Client-side compilation

In this scenario, the browser includes less.js and references the LESS stylesheets in `<link>` tags with the MIME type "stylesheet/less". LESS finds these references and downloads the files with `XMLHttpRequest`s. Consequently, the network agent regards the LESS files as requested, causing the HTML document to reload when the LESS document is saved.

For better live development support, Brackets would need to compile the LESS code to CSS and update the corresponding CSS code in the browser.

There are three critical components to this:
* The compiler to convert LESS code into CSS code (could be requested via `doc.getLanguage().getDefaultCompilerToLanguage("css")`)
* The updater to find the `<link>` tag that referenced the LESS file, determine the ID of the corresponding `<style>` tag, generate the CSS code (using the compiler), and change the contents of the `<style>` tag
* The live development module to allow registering the updater, trigger it at the right time and avoid reloading the page if the updater was successful

#### Server-side compilation

In this scenario, the server compiles LESS code to CSS on the fly. There are various conceivable ways to implement this. The URLs for the file might look like this:

* `style.less` - same file name, but transparently returns the CSS version
* `style.less?compile=true` returns the CSS version while `style.less` will return the file as-is
* `style.less.css` returns the CSS version
* `style.css` returns the CSS version
* `compiled/style.less` returns the CSS version
* `compiled/style.css` returns the CSS version
* `compile.php?file=style.css` returns the CSS version
* `all_styles.css` - returns various styles combined into one file

If the file name is not modified (and even if a query string is appended to its URL), the network agent will already declare the file as requested. The network agent could detect usage of the two conventions that only modify the file extension by checking whether the file exists on the server. If not, it could conclude that the server generates derivatives of the current file for these URLs.

If the network agent also listens to the "responseReceived" event, it will be able to report the MIME type as well. We can then check whether there is a compiler for the current document's language (LESS) to a language that has the same MIME type as returned by the server. If so, we can update the file ourselves whenever it is changed in the same way normal CSS files are updated.

However, this might fail if the server compiler uses special settings, like a search path to use when including other LESS files in a LESS file. A safe compromise would be to only update the stylesheet after it has been saved: in this case, the compiled document can simply be requested by the server and we would only inject the result into the running page.

An alternative with other risks would be to transparently move the saved version to a backup location, silently save the changed version in the editor, request the compiled version from the server and restore the saved version from the backup. If either Brackets or the whole computer crash in the middle of that process, or a cloud storage service watches the directory, this approach could result in data loss.


# Showing the context in Live Preview

These areas are concerned with showing the context of the current cursor position by highlighting affected areas in the live preview and opening files related to elements in the page (GotoAgent).

* LiveDevelopment/LiveDevelopment.js
    * **Requires hardcoded list of special documents**, namely {CSS,HTML,JS}Document. These function as updaters and highlighters.
    * Function `_classForDocument` **uses a hardcoded mapping of file extensions to document types**
    * Function `_openDocument` **only loads related CSS documents** (no JavaScript, not extensible)
    * Method `showHighlight` **only calls doc.updateHighlight() for CSSDocuments**
* LiveDevelopment/Agents/GotoAgent.js
    * **Has hardcoded support for HTML, CSS and JavaScript**
* LiveDevelopment/Agents/HighlightAgent
    * **Has hardcoded support for HTML and CSS**
* LiveDevelopment/Agents/RemoteFunctions.js
    * Function `_typeColor` has **hardcoded distinctions between html, css, js and others**

Todo: describe what is necessary to make this extendable






## Notes

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