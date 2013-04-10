For the current state of language support, see page [Language Support](Language Support). This page documents what additional capabilities extensions would need in order to add language support on par with the current level of support for HTML, JavaScript and CSS. 

Our goal is to base all language-related features like code hinting, text manipulation commands, quick edit, live preview, etc. on these capabilities. Support for non-web programming languages will be moved moved to dedicated extensions (see [issue #2969](https://github.com/adobe/brackets/issues/2969)).


### Providing code semantics

Allow adding electric __strings__ to language definitions. Some languages have block boundaries that are not just one char long, for instance Ruby's `begin ... end`.

### Starting Live Preview

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