_DRAFT_

Motivation for this document is to spec out API changes to enable displaying an image from the project tree. Also see this Trello card for the user story  [_Preview Images_ user story](https://trello.com/c/l9AcILkC/24-8-preview-images).

Documents in Brackets have always been text documents with a code mirror instance to manage state and view. Thus the Document and Editor classes make assumptions every document is a text document. That needs to change to enable this user story

Hence the question how to extend and modify the existing code while keeping the following goals in mind:
* avoid breaking extensions, if we do, we better have a good reason
* build core code that is maintainable
* build core code that is extensible
* keep API intuitively understandable

---
## Have extensions?
If you have written extensions, this is what you need to know:

### Using EditorManager?

### Using DocumentManager?
`DocumentManager.getCurrentDocument()`: returns NULL while an image is displayed

Need the current document? Do this:
~~~~
var activeEditor = EditorManager.getActiveEditor(),
    activeDoc = activeEditor && activeEditor.document;
~~~~
### Consuming Events?
**DocumentManager: currentDocumentChange** - This event will be sent if an image is displayed.

Example, also see example for `DocumentManager.getCurrentDocument` above: 
~~~~
function _onCurrentDocumentChange() {
   var doc = DocumentManager.getCurrentDocument();
   if(doc){ // make sure to check for null
     ...
   }
   // even better use example above 
}
$(DocumentManager).on("currentDocumentChange", _onCurrentDocumentChange);
~~~~

**EditorManager: activeEditorChange** -  2nd argument is NULL when image is displayed
Example: 
When listening for activeEditorChange expect NULL for current: 

~~~~~
    function _onActiveEditorChange(event, current, previous) {
        if (current) {
            ...
        }
        if (previous) { // previous can be null too!
            ...
        } 
    }
    $(EditorManager).on("activeEditorChange", _onActiveEditorChange)
~~~~~        



Test your extensions with this branch:
TBA

---


The following design has been picked after investigating a number of architectures comparing trade offs between making non-disruptive or less disruptive API changes at the cost of conceptual clarity of Editor and Document classes.

This design does seems to strike a good balance between the goals stated above

Behavior details
* Single click on image in project display image
* Images are not added to working set
* FileOpen opens an image if a single image file is selected in the open dialog
* Drag & Drop of single image displays image
* Drag & Drop of multiple images does not display any image
* No _Save As_ for images
* File Rename, Delete, Show in File Tree, Show in OS work on image files the same way as for other files.
* Find, Replace, QuickOpen UI and the likes do not show UI when an Image is displayed
* Copy does not modify the clipboard contents
* Cut, Paste do nothing on an image file.

Implementation details
* DocumentManager.getCurrentDocument returns null while an image is displayed, so that code / extensions that modify documents do not have to be updated.
* EditorManager.getFocusedEditor always returns null when image is displayed
* EditorManager.getActiveEditor always returns null when image is displayed
* EditorManager.getCurrentFullEditor always returns null when image is displayed
* A div-tag containing an img-tag is appended to the code mirror wrapper element to display the image. The image itself is not loaded at all. It is loaded by the browser / CEF  to render the view.
* A new mode / language will be added such that LanguageManager.getLanguageForPath() on an image can be called to identify wether fileEntry points to an image file, by checking mode.getName(), i.e.
`var mode = LanguageManager.getLanguageForPath(fullPath);
if(mode.getName() === "image"){...}`
* Extensions that are prepared for getCurrentDocument to return NULL will be fine.
* DocumentManager.getDocumentForPath is modified so that no document will be created for paths to image files.
DocumentCommandHandlers.doOpen will own the responsibility to determine wether an image file or a text file or something else is attempted to be opened.

[More info on different designs discussed](https://github.com/adobe/brackets/wiki/Preview-Images-Research----old-drafts)