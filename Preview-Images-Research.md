_DRAFT_

Motivation for this document is to enable a educated decision on trade offs that have to be made when introducing API changes to enable displaying an image from the project tree to complete the  [_Preview Images_ user story](https://trello.com/c/l9AcILkC/24-8-preview-images).

Documents in Brackets have always been text documents with a code mirror instance to manage state and view. Thus the Document and Editor classes make assumptions every document is a text document. Hence the question how to extend and modify the existing code while keeping the following goals in mind:
* avoid breaking extensions, if we do, we better have a good reason
* build core code that is maintainable
* build core code that is extensible
* keep API intuitively understandable

The seems to be a trade off between making non-disruptive or less disruptive API changes at the cost of conceptual clarity of Editor and Document classes.

This document explores the ramifications of four ideas, ranked by hackiness, where I'm assuming that the most hacky is the least disruptive API change:
* [Modal dialog with image](https://github.com/adobe/brackets/wiki/Preview-Images-Research#show-modal-dialog-with-image)
* Non modal image viewer in place of the text editor
  * backed by a [standard document](https://github.com/adobe/brackets/wiki/Preview-Images-Research#non-modal-image-viewer-in-place-of-the-text-editor-backed-by-a-standard-document)
  * backed by a [custom document](https://github.com/adobe/brackets/wiki/Preview-Images-Research#non-modal-image-viewer-in-place-of-the-text-editor-backed-by-a-custom-document)
* [backed by a generic image document, here Document and Editor have base classes](https://github.com/adobe/brackets/wiki/Preview-Images-Research#generic-model-where-document-and-editor-have-base-classes)

##  Show modal dialog with image
* add logic to DocumentCommandHandlers.doOpen() to detect file type to open modal dialog.

Advantage: 
* smallest change, neither Document API nor Editor API need to change.
* least disruptive, No extensions will break

Disadvantage
* user experience - some say it's not the best.
* no "Save as...".

Open question:
* when would the modal dialog open? Single click, double click, hover


## Non modal image viewer in place of the text editor
The following 2 options share the following details:

Behavior:
* Double click on image adds to working set.
* FileOpen adds to working set.
* Save as creates copy of file, updates working set

Implementation:
* add new mode / language: API clients like extensions can check the language / mode
* getFocusedEditor returns null

Common disadvantage: getFocusedEditor returns null - this will break some extensions

### Non modal image viewer in place of the text editor backed by a standard document
_Also known as Glenn's proposal_
* A HTML doc with the image is placed injected into the code mirror editor. 
* A document for an image is a standard but immutable doc w/o text. 
* All calls to text based APIs (get text, cursor, selection) should remain unchanged and respond as they would on an empty document.
* Provide support immutable documents, APIs that modify text would have to be tweaked to check for mutability.
* EditorManager.focusEditor() returns focus to last element shown in main editor space, i.e. image if that had focus or last editor otherwise

__Question what about getActiveEditor,  getCurrentFullEditor should return null

Advantage: 
* since EditorManager and DocumentManager APIs are unchanged, fewer extensions will break.
Disadvantage: 
* extra work to enable immutable documents
* extra work to maintain EditorManager.focusEditor
* Documents class gains in complexity, no separation of concerns for text and image documents, seems harder to maintain if new editors are added, i.e. image editors, hex editor, ...

###  Non modal image viewer in place of the text editor backed by a custom document
_Also known as the original proposal_ - based on the discussion and outline [here](https://github.com/adobe/brackets/pull/4492) 
* A HTML doc with the image is placed in place of the editor. A document for an image is a new type of immutable document. 
* ImageDocument asserts / throws errors when any text-related APIs are called.
* DocumentManager.getDocumentForPath(fullPath) on path to image returns ImageDocument.
* DocumentCommandHandlers.doOpen() generates a special image document
* ImageDocument is immutable
* ImageDocument has no editor, but a view
* DocumentManager.getCurrentDocument()  - returns either standard document or image document
* Image document does not have any of the methods related to text.
* EditorManager.getCurrentFullEditor - returns NULL when ImageDocument is displayed
* EditorManager.getFocusedEditor - returns NULL when ImageDocument is displayed
* EditorManager.getActiveEditor - returns NULL when ImageDocument is displayed
* Listeners to DocumentManager event currentDocumentChange or EditorManager event activeEditorChange may break. Event activeEditorChange will have NULL as argument in place of new Editor.
* EditorManager.focusEditor() gives focus to ImageViewer

Advantage: 
* conceptual clarity through separation of concerns. 
* Opens a path to add more types of documents

Disadvantage: 
* more extensions break. 

###  example extensions may break
code folding, delete-line-start-end, superclipboard.js, case-converter, brackets-special-html-chars, brackets-minifier, brackets-indent-guides, spell-check, brackets-xunit, brackets-beautify, camden.w3cvalidation, cezarwojcik.cleaner, dehats.annotate, dehats.prefixr, dehats.togist, fontParser, enturn.quick-search, fontface.brackets-vimderbar, mikaeljorhult.brackets-autoprefixer, ...:


###  Generic model where Document and Editor have base classes
seems to be the least likely choice, being the most disruptive.
* DocumentManager.getCurrentDocument()  -  returns BaseDocument. Any consumer of this API needs to check wether it can call text or cursor related APIs
* EditorManager.getCurrentFullEditor - returns BaseEditor. Any consumer of this API needs to check wether it can call code mirror /text editor related APIs
* EditorManager.getFocusedEditor - returns BaseEditor. Any consumer of this API needs to check wether it can call code mirror /text editor related APIs
* EditorManager.getActiveEditor - returns BaseEditor. Any consumer of this API needs to check wether it can call code mirror /text editor related APIs

This is not fully fleshed out as it doesn't seem to be the obvious choice.

Disadvantage: 
* Some new code to write for Base classes, plenty of core code to change - which seems particularly risky
*  extensions break - expected about the same amount as in previous option _original proposal_

## Summary
I suggest to implement _Glenn's proposal_ for the following reasons
* better user experience than Modal dialog with image
* few extensions break, fixes should be straightforward
* requires a reasonably small change to core code, which introduces minimizes the risk regarding unexpected challenges that may have been missed writing up this document.
* despite lack conceptual clarity the change seems small enough to revert, i.e in case of the need for more flavors of viewers and editors.

As a 2nd option I suggest to go with _Modal dialog with image_ as this is a very small and low risk change without side effects. The simplicity of the change outweighs the disadvantage of the user experience.