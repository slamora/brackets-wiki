_DRAFT_

Motivation for this document is to enable a educated decision on trade offs that have to be made when introducing API changes to enable displaying an image in place of the  main editor.

The problem is that documents in Brackets have always been text documents with a code mirror instance to manage state and view. Thus the Document and Editor classes make assumptions every document is a text document.

To complete the  [_Preview Images_ user story](https://trello.com/c/l9AcILkC/24-8-preview-images) Brackets needs to show a simple view ( a very simple HTML document ) in place of a code mirror instance.

The choice seems to be be between making non-disruptive or less disruptive API changes at the cost of conceptual clarity of Editor and Document classes.

This document explores the ramifications of four ideas, ranked by hackiness, where I'm assuming that the most hacky is the least disruptive API change:
* [Modal dialog with image](https://github.com/adobe/brackets/wiki/Preview-Images-Research#show-modal-dialog-with-image)
* [Non modal image viewer in place of the text editor backed by a standard document](https://github.com/adobe/brackets/wiki/Preview-Images-Research#non-modal-image-viewer-in-place-of-the-text-editor-backed-by-a-standard-document)
* [Non modal image viewer in place of the text editor backed by a custom document](https://github.com/adobe/brackets/wiki/Preview-Images-Research#non-modal-image-viewer-in-place-of-the-text-editor-backed-by-a-custom-document)
* [Generic model where Document and Editor have base classes](https://github.com/adobe/brackets/wiki/Preview-Images-Research#generic-model-where-document-and-editor-have-base-classes)


###  Show modal dialog with image
* add logic to DocumentCommandHandlers.doOpen() to detect file type to open modal dialog.

Notes
* smallest change, neither Document API nor Editor API need to change.
* least disruptive - non breaking
* user experience - some say it's not the best.
* no "Save as...".

Open question:
* when does the modal dialog open? Single click, double click, hover

### Non modal image viewer in place of the text editor backed by a standard document
_Also known as Glenn's proposal_
* A HTML doc with the image is placed in place of the editor. A document for an image is a standard but immutable doc w/o text. All calls to text based APIs (get text, cursor, selection) should remain unchanged and respond as they would on an empty document.
* So far Brackets does not support immutable documents, hence all APIs that modify text would have to be tweaked to check for mutability.
* getFocusedEditor should return null - need to investigate what the implications are. Some code may assume that if getFocusedEditor returns null that the working set must be empty.
* EditorManager.focusEditor() returns focus to last element shown in main editor space, i.e. image if that had focus or last editor otherwise

###  Non modal image viewer in place of the text editor backed by a custom document
_Also known as the original proposal_ - based on the discussion and outline [here](https://github.com/adobe/brackets/pull/4492) 
* A HTML doc with the image is placed in place of the editor. A document for an image is a new type of immutable document. 
* ImageDocument asserts / throws errors when any text-related APIs are called.
* DocumentManager.getDocumentForPath(fullPath) on path to image returns ImageDocument.
* DocumentCommandHandlers.doOpen() generates a special image document
* ImageDocument is immutable
* ImageDocument has no editor, but a view
* DocumentManager.getCurrentDocument()  - returns Document, standard document or image document
* Image document does not have any of the methods related to text.
* EditorManager.getCurrentFullEditor - returns NULL when ImageDocument is displayed
* EditorManager.getFocusedEditor - returns NULL when ImageDocument is displayed
* EditorManager.getActiveEditor - returns NULL when ImageDocument is displayed
* Listeners to DocumentManager event: currentDocumentChange or EditorManager event: activeEditorChange may break. Event activeEditorChange will have NULL as argument in place of new Editor.
* EditorManager.focusEditor() gives focus to ImageViewer
* Double click on image adds to working set.
* FileOpen: adds to working set.
* add new mode / language: API clients like extensions should check the language / mode
* Save as creates copy of file, updates working set

TBD: how many extensions break and if so how? How involved would be the fix

###  extensions may break
code folding, delete-line-start-end, superclipboard.js, case-converter, brackets-special-html-chars, brackets-minifier, brackets-indent-guides, spell-check, :


###  Generic model where Document and Editor have base classes
seems to be the least likely choice, being the most disruptive.
* DocumentManager.getCurrentDocument()  -  returns BaseDocument. Any consumer of this API needs to check wether it can call text or cursor related APIs

* EditorManager.getCurrentFullEditor - returns BaseEditor. Any consumer of this API needs to check wether it can call code mirror /text editor related APIs
* EditorManager.getFocusedEditor - returns BaseEditor. Any consumer of this API needs to check wether it can call code mirror /text editor related APIs
* EditorManager.getActiveEditor - returns BaseEditor. Any consumer of this API needs to check wether it can call code mirror /text editor related APIs
* ... lots of changes everywhere
This will break a lot of extensions and will require many core changes.


