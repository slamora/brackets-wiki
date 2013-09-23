 Motivation for this document is to enable a educated decision on trade offs that have to be made when introducing API changes to enable displaying an image in place of the  main editor.

The problem to be solved is that so all documents in Brackets so far have always been text documents that use a code mirror instance to manage state and view. Thus the Document and Editor classes make assumptions such that it is always possible to call getText() documents and that every editor is always a codemirror instance tied to a text document.

To complete the  [_Preview Images_ user story](https://trello.com/c/l9AcILkC/24-8-preview-images) Brackets needs to show a simple view ( a very simple HTML document ) in place of a code mirror instance.

The choice seems to be be between making non-disruptive or less disruptive API changes at the cost of conceptual clarity of Editor and Document classes.

This document explores the ramifications of four ideas, ranked by hackiness, where I'm assuming that the most hacky is the least disruptive:
* Modal dialog with image
* Non modal image viewer in place of the text editor backed by a standard document that has no text
* Non modal image viewer in place of the text editor backed by a custom document
* Clean generic model where Document and Editor have base classes. 


###  Show modal dialog with image
* add logic to DocumentCommandHandlers.doOpen() to detect file type to open modal dialog.

Advantages
* most simple in terms of implementation. Neither Document, nor Editor API need to change.

Disadvantages
* user experience
* no "Save as...".

Open question:
* when does the modal dialog open? Single click, double click, hover
