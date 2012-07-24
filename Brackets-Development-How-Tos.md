## Asynchronous APIs ##

Many operations in Brackets return their results asynchronously -- for example, because they involve file I/O. These APIs return a [jQuery Promise](http://api.jquery.com/Types/#Promise) object that you can use to listen for success/failure and retrieve the result.

For working with a sequence of asynchronous operations (in parallel or in serial), the Async utils module may be helpful.

## Core Classes ##

When dealing with files the user is editing, there are three important classes to understand:

* `Editor` represents the view (it wraps a CodeMirror widget) -- either a full-size editor _or_ an inline editor. An Editor can have focus. Use an Editor object to get/set the cursor position, selection, or scroll position. Every Editor is attached to a Document.
* `Document` represents the model (the text content of the file). Use the Document object to get or modify the text, or to listen for changes to the text. There may be multiple Editors attached to a single Document (for example, a full-size editor plus an inline editor). Every Document is associated with a file on disk.
* `FileEntry` represents a file on disk. It's very lightweight, like a URI -- it doesn't actually store the file's contents. FileEntry is based on the W3C ["Directories and System" draft spec](http://www.w3.org/TR/file-system-api/#the-fileentry-interface) (not to be confused with the much more limited ["HTML5 file API" spec](http://www.w3.org/TR/FileAPI/)).

## <a name="doc"></a>Working with Documents ##

```Document``` is an object that represents a file on disk. Documents perform several important functions: they are the backing model for Editors; they provide APIs for reading and modifying the text content; and they emit events whenever the text is edited.

### How to get a Document ###

* If you're operating on an Editor, use ```Editor.document```.
    * To get the currently focused Editor, use `EditorManager.getFocusedEditor()`. This may return an inline editor.
    * If you always want the current _full-size_ editor (even if focus actually belongs to an inline editor within it), then use `DocumentManager.getCurrentDocument()` or `EditorManager.getCurrentFullEditor().document`.
* To get a Document for any file, use ```DocumentManager.getDocumentForPath()```. This returns asynchronously because it may need to read the file's content from disk.
* If you're sure a file is already open, you can use the synchronous ```DocumentManager.getOpenDocumentForPath()``` instead -- but it will return null if you're wrong.

### Proper Document usage ###

Documents are globally tracked, and thus _**must be ref counted**_ under certain circumstances.

> _Aside: Why is ref counting needed?_ We want all parts of Brackets to have a consistent view of the (potentially unsaved) contents of a file being edited.  Yet we also want to throw out that state when all parts of Brackets stop caring about a file, instead of keeping it in memory forever.  If JavaScript supported weak references they would be an ideal solution to these twin constraints, but alas -- we're stuck with ref counting instead.


**If you're only holding onto a Document for the duration of a function call** then you're in the clear and don't have to worry about this.  The text editing commands in EditorCommandHandlers are good examples of this.

If, on the other hand, you're...
* Storing a Document object in a property that you'll access later
* Getting a Document, doing something asynchronous, and then accessing the same Document pointer again when it's done
* Attaching event listeners to a Document

...then you'll need to be more careful. In all these cases, you must call ```addRef()``` on the Document after you fetch it, and then later call ```releaseRef()``` when you're done using it (e.g. when you null out / overwrite the property pointing to it, or detach your listeners).

The Document and its full text content will be kept in memory until you call ```releaseRef()```, so it's better to avoid holding onto Documents if possible. In many cases, you can just store the file's path and re-fetch the Document each time you need it.

**If you're attaching event listeners to Document**:
* The Document "change" event fires on virtually every keystroke. For performance reasons, consider deferring your processing until later (for example, using the DocumentManager "documentSaved" event instead).
* If you're listening for Document changes, you probably also care about Document deletion -- so be sure to listen for the Document "deleted" event as well.

### Making edits ###

To modify a Document's text content, use ```Document.replaceRange()```. If you're going to call it multiple times as the result of a single user action, wrap all your calls in ```Document.batchOperation()``` to ensure they're all batched into a single Undo/Redo entry.


## Menus and Keyboard Shortcuts ##

See [How to write extensions](How to write extensions#wiki-uihooks).

## Writing a New Inline Editor (Quick Edit) ##

See [How to write extensions](How to write extensions#wiki-featurehooks).