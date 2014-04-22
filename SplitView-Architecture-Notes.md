Refactoring Brackets to Support Split View with Multiple Documents requires quite a bit of hacking on the plumbing but there are only a few places that need to be heavily refactored to do so.

# Current Implementation
This is kind of an object view of the system involved. There are ancillary object involved which will be discussed at another time.

The `.content` area of the application contains many widgets (in addition to the editor) that add to Bracket's user experience. These all start with the `PanelManager` which, for all intents and purposes, only manages the placement of these widgets. It does other things but doesn't really manage panels.

Here's a 50,000ft view of how it's glued together:
 
`PanelManager` View which manages the placement of all panels (subviews) in the dom node `.content` it does not manage the status-bar.  
   +-> Replace all Results Panel (subview of mustache-rendered dom node `#search-results`) managed by FindInFiles.js   
   +-> Find In Files Results Panel (subview of mustache-rendered dom node `#replace-all-results`) managed by FindReplace.js  
   +-> Problems (jsLint) Panel (subview of of mustache-rendered dom node `#problems-panel`)
Managed by CodeInspection.js  

Various services create, show and manipulate these panels by rendering some HTML using Mustache and adding to the dom by calling `PanelManager.createBottomPanel()` which wraps the HTML snippet and inserts it into the DOM. 

`EditorManager` Manages the creation and destruction of an editor for a document.   
    +-> `Editor` Wraps the code mirror instance, code mirror options and provides a high level api this is basically the view attached to `#editor-holder`  

Currently there is only 1 instance of the visible editor so its dom element is part of the base HTML and it's referenced whenever needed as `$(#editor-holder)` 

The layout of the editor is fluid for the most part, but the height is computed by `PanelManager` and the editor is resized by triggering an Event and passing data with the event to tell the editor how tall it should be. The `EditorManager` then handles this event and sets the `$(#editor-holder)` to the explicit height negotiated by the panel manager.  The event is passed to the `Editor` object which then tells codemirror to recompute its scroll state and other size related data.

# Proposed Implementation 

The first part is Basically a simple refactoring pattern to push down `EditorManager` and put a new object to manage the construction and handle the layout of `EditorManagers`. .  

`EditorLayoutManager` is a global singleton which manages all full sized editors (inline editors are not managed by this object) and has various APIs for creating, accessing and destroying instances of an `Editor` through a corresponding`EditManager`.  It manages all `EditorManagers` instances and negotiates the space needed for the editor when handling resize events.  It's attached to the DOM node `#editor-layout-holder` which is `#editor-holder` renamed.

Renaming `#editor-holder` may break some things -- which is good because those looking to traverse the DOM starting at `#editor-holder` looking for an editor are going to need to fail.  Most of the instances are simple search and replace.  They things like knowing where to put the `menu-bar` or where to put the Find/Replace dialog.

`EditorLayoutManager` View of `#editor-layout-holder`    
    +-> Array[1][1] of `EditorManager` instances  

There cannot be `0,0` (rows, columns) elements so there needs to be logic to enforce that.  Initially it is 1 row 1 column so there will always be at least 1 instance of `EditorManager` to work on.

## EditorLayoutManager.setLayoutScheme(_rows_,_columns_)  

This API will change the layout to match _rows_ and _columns_.   

RULES:
* Only 1 row and 2 columns are initially supported
* Any Pane created by this API initially will show the Brackets logo interstitial screen until the corresponding `EditorManager` for that pane has been loaded with a document or image.
* When a Pane is  destroyed, all documents in the corresponding working set for that pane are moved to another pane's working set.  Since there is only 2 panes in the initial implementation this is just a matter of collapsing them down to the remaining Pane's working set.

Creating a new editor instance is  rendered at runtime using Mustache to generate the HTML and insert it into the DOM.  The `Editor` Instance will generate the HTML when the `EditorManager` asks for it and `EditorLayoutManager` will insert it into the DOM in the appropriate place to ensure proper keyboard navigation.  

NOTE: We could take a shortcut and just generate 2 editor DOM nodes and set the one that isn't used to `display: none` but that might make the rendering engine do more work when it doesn't need to and it means that `EditorLayoutManager` would need to keep track of or interrogate the node to see if it's visible or not when it handles requests to relayout the editors in response to resize events.  We will evaluate performance and see which way is better.

`EditorLayoutManager` will respond to the `PanelManager`'s resize event.  This is just going to resize the `#editor-layout-holder` DOM node.  But there is a little extra processing that takes place

The `PanelManager` computes the new size of `#editory-layout-holder` in its `window.resize` event handler and triggers an event to resize that `EditorLayoutManager` listens for. Currently `PanelManager` only computes the Height and passes that as data but it needs to also compute Width and pass that along with the Height so that the `EditorLayoutManager` can verify that the width doesn't get too lean to handle 2 editor instances.  At the point that it is too lean then it will need to start resizing other columns to get a decent layout.  This algorithm becomes more complex with more columns and rows.  For now it can be a matter of going to something like 50%.  We may want to bump up the shell's minimum width as well to avoid getting too small.

These are proposed APIs that may not be implemented initially.  We definitely won't need `setRowHeight` but we should have a way to drag the column border to set the width of the column.

## EditorLayoutManager.setColumnWidth(_column_, _width_)  
## EditorLayoutManager.setRowHeight(_row_, _height_)  

Generally this should be done with percentages.  _width_ can be passed in in pixels and converted to a percentage when affixing the CSS to the columns `width: 40%`.  Doing it in a percentage and only applying to any except the rightmost column will yield a fluid layout.  The API will reject setting the width on the rightmost column.

EditorLayoutManager also manages the Working Set for each pane.

## EditorLayoutManager.getWorkingSet(_row_, _column_)  
## EditorLayoutManager.addToWorkingSet(_row_, _column_, _file_, _open_)  
## EditorLayoutManager.removeFromWorkingSet(_row_, _column_, _file_)  
## EditorLayoutManager.getAllWorkingSets()   

# Deprecate `DocumentManager.getWorkingSet()  ` 
A deprecation warning will be added and the function will return a unique list of all documents (not files) from all working sets.

All core extensions and functions that use `DocumentManager.getWorkingSet()` will call one of the 2 `get*` functions above.

3rd Party extensions:

```text
---------------------------+-----------------------------------------+-----------------------------------  
 Name                      | Usage                                   | Proposed Change                    
---------------------------+-----------------------------------------+-----------------------------------  
Close-Others               | Adds Close commands the Working Set Menu| No change Needed. The extension is  
                           |                                         | packaged to run only on brackets  
                           |                                         | <= 0.32.0   
---------------------------+-----------------------------------------+-----------------------------------  
Brackets-Typescript        | Subclasses Working The Set and exposes  | This is a very complicated case.  
                           | back to the rest of the extension.  It  | We should probably contact the   
                           | also handles various DocumentManager    | extension author propose he   
                           | events that are triggered when the      | use the new api.  This is tricky  
                           | working set is changed.                 | because it's also responding to                                                              
                           |                                         | editorManager.activeEditorChange  
                           |                                         | should probably be ok but we   
                           |                                         | can use this to validate the old API  
---------------------------+-----------------------------------------+-----------------------------------  
Brackets-autosave-files-on-| Saves all files in working set on       | OK using deprecated API  
window-blur-1.0.4          | window.blur()                           | 
---------------------------+-----------------------------------------+-----------------------------------  
Brackets-autosave-files-on-| Saves all files in working set on       | OK using deprecated API  
window-blur-1.0.4          | window.blur()                           |    
---------------------------+-----------------------------------------+-----------------------------------  
Brackets-editor-nav-master | Adds a command for QuickOpen to search  | OK using deprecated API but   
                           | the open document list                  | Peter Flynn wrote it and can fix it  
---------------------------+-----------------------------------------+-----------------------------------  
Brackets-editor-nav-master | Adds a command for QuickOpen to search  | OK using deprecated API but   
                           | the open document list                  | Peter Flynn wrote it and can fix it  
---------------------------+-----------------------------------------+-----------------------------------  
zaggino.brackets.git       | Iterates over the working set and closes| OK using deprecated API   
                           | any document that has been removed      | Doesn't file watchers do this?  
---------------------------+-----------------------------------------+-----------------------------------  
brackets-wdminmap-master   | Hides a widget when the working set is  | OK using deprecated API  
                           | EMPTY                                   |
---------------------------+-----------------------------------------+-----------------------------------  
zaggino.brackets.git       | Iterates over the working set and closes| OK using deprecated API   
                           | any document that has been removed      | **Doesn't file watchers do this?**  
---------------------------+-----------------------------------------+-----------------------------------  
zaggino.brackets.git       | Adds a command to close unmodified files| OK using deprecated API   
                           | Iterates over the working set and closes|   
                           | any document that hasn't been modified  |   
---------------------------+-----------------------------------------+-----------------------------------  

```


# Supporting Images

Working set so far has be devoid of any image files.  To support images in split view, we will need to change the working set rules to allow for images to be in the working set. This means that callers of the new working set APIs will need to check to make sure they can operate on a file by getting its file type from the language manager or checking the extension. 

The working set will basically just be a list of files that may or may not have a Document object owned by the Document Manager.  The deprecated `DocumentManager.getWorkingSet()` will filter out any image files without a Document object.

NOTE: Since images are now in the working set it is opportunistic for us to fix the images don't refresh issue since image instances in the working set can become stale.  

# Implementing the Layout Manager

The initial implementation will be 2 columns x 1 row or 2 rows x 1 column.  However, implementing an arbitrary number of rows and columns could be trivial to do using this code:
https://github.com/FriendCode/codebox/blob/master/client/views/grid.js which is Apache-licensed.
However, this is a fairly integral piece to codebox and depends on other codebox libraries in order to work.

# Additional Changes

`DocumentManager.getCurrentDocument()` will be mapped to `EditorLayoutManager.getFocusedPane().getEditor().getDocument()`  since `DocumentManager` will no longer maintain the "current document".

# Performance Testing

Do early research on typing and scrolling performance.





