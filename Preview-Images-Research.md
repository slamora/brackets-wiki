 Motivation for this document is to enable a educated decision on trade offs that have to be made when introducing API changes to enable displaying an image in place of the  main editor.

The problem to be solved is that so all documents in Brackets so far have always been text documents that use a code mirror instance to manage state and view. Thus the Document and Editor classes make assumptions such that it is always possible to call getText() documents and that every editor is always a codemirror instance tied to a text document.

To complete the  [_Preview Images_ user story](https://trello.com/c/l9AcILkC/24-8-preview-images) Brackets needs to show a simple view ( a very simple HTML document ) in place of a code mirror instance.

The choice seems to be be between making non-disruptive or less disruptive API changes at the cost of conceptual clarity of Editor and Document classes.

This document explores the ramifications of four ideas, ranked by hackiness, where I'm assuming that the most hacky is the least disruptive:
* [Modal dialog with image](https://github.com/adobe/brackets/wiki/Preview-Images-Research#show-modal-dialog-with-image)
* [Non modal image viewer in place of the text editor backed by a standard document](https://github.com/adobe/brackets/wiki/Preview-Images-Research#non-modal-image-viewer-in-place-of-the-text-editor-backed-by-a-standard-document)
* [Non modal image viewer in place of the text editor backed by a custom document](https://github.com/adobe/brackets/wiki/Preview-Images-Research#non-modal-image-viewer-in-place-of-the-text-editor-backed-by-a-custom-document)
* Clean generic model where Document and Editor have base classes. 


###  Show modal dialog with image
* add logic to DocumentCommandHandlers.doOpen() to detect file type to open modal dialog.

Advantages
* smallest change, neither Document API nor Editor API need to change.
* least disruptive - non breaking

Disadvantages
* user experience - some say it's not the best.
* no "Save as...".

Open question:
* when does the modal dialog open? Single click, double click, hover

### Non modal image viewer in place of the text editor backed by a standard document
_Also known as Glenn's hack_
* Image documents are empty standard but immutable documents. All calls to text based APIs (get text, cursor, selection) should remain unchanged and respond as they would on an empty document.
* So far Brackets does not support immutable documents, hence all APIs that modify text would have to be tweaked to check for mutability.
* getFocusedEditor should return null - need to investigate what the implications are. Some code may assume that if getFocusedEditor returns null that the working set must be empty.

Needs more research to determine how many extensions break and if so how they break

###  Non modal image viewer in place of the text editor backed by a custom document
_Also known as the original proposal_ - based on the discussion and outline [here](https://github.com/adobe/brackets/pull/4492) 

 