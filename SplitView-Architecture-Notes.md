Refactoring Brackets to Support Split View with Multiple Documents requires quite a bit of hacking on the plumbing but there are only a few places that need to be heavily refactored to do so.

# Current Implementation
This is an object view of the system. There are ancillary objects involved which will be discussed at another time.

The `.content` area of the application contains many widgets (in addition to the editor) that add to Bracket's user experience. These all start with the `PanelManager` which, for all intents and purposes, only manages the placement of these widgets. It does other things but doesn't really manage panels.

Here's a 50,000ft view of how it's glued together:
 
`PanelManager` View which manages the placement of all panels (subviews) in the dom node `.content` it does not manage the status-bar.  
`   +->` Replace all Results Panel (subview of mustache-rendered dom node `#search-results`) managed by FindReplace.js   
`   +->` Find In Files Results Panel (subview of mustache-rendered dom node `#replace-all-results`) managed by FindInFiles.js  
`   +->` Problems (jsLint) Panel (subview of of mustache-rendered dom node `#problems-panel`)
Managed by CodeInspection.js  

Various services create, show and manipulate these panels by rendering some HTML using Mustache and adding to the dom by calling `PanelManager.createBottomPanel()` which wraps the HTML snippet and inserts it into the DOM. 

`EditorManager` Manages the creation and destruction of an editor for a document.   
`    +-> Editor` Wraps a code mirror instance, code mirror options and provides a high level api this is basically the view attached to `#editor-holder`  

Currently there is only 1 instance of the visible editor so its dom element is part of the base HTML and it's referenced whenever needed as `$(#editor-holder)` 

The layout of the editor is fluid for the most part, but the height is computed by `PanelManager` and the editor is resized by triggering an Event and passing data with the event to tell the editor how tall it should be. The `EditorManager` then handles this event and sets the `$(#editor-holder)` to the explicit height negotiated by the panel manager.  The event is passed to the `Editor` object which then tells codemirror to recompute its scroll state and other size related data.

# Proposed Implementation 

First we create a helper to manage the layout of editors.

`EditorPaneManager` is a helper for `EditorManager` to compute the placement and layout of all full sized editors (inline editors are not managed here) and has various APIs for changing the layout of the editor area and placement `Editor` objects.  

`EditorManager` View of `#editor-holder`    
`   +-> EditorPaneManager`
`            +-> Array[1][1]` of `Editor` instances  (`.editor-pane`)

There cannot be `0,0` (rows, columns) elements so there needs to be logic to enforce that.  Initially it is 1 row 1 column so there will always be at least 1 instance of `EditorManager` to work on.

## EditorPaneManager.setLayoutScheme(_rows_,_columns_)  

This API will change the layout to match _rows_ and _columns_.   

RULES:
* Only 1 row and 2 columns or 2 rows and 1 column are initially supported
* Any Pane created by this API initially will show the Brackets logo interstitial screen until the corresponding `EditorManager` for that pane has been loaded with a document or image.
* When a Pane is  destroyed, all documents in the corresponding working set for that pane are moved to another pane's working set.  Since there is only 2 panes in the initial implementation this is just a matter of collapsing them down to the remaining Pane's working set.

Creating a pane is rendered at runtime using Mustache to generate the HTML and insert it into the DOM.  The `Editor` Instance will generate the HTML when the `EditorManager` asks for it and `EditorPaneManager` will insert it into the DOM in the appropriate place to ensure proper keyboard navigation.  The generation can either be Mustache generated or simple jQuery insertion.

`EditorManager` will handle the `PanelManager`'s resize event and call a function on its `EditorPaneManager` to update its layout. 

The `PanelManager` computes the new size of `#editory-layout-holder` in its `window.resize` event handler and triggers an event to resize that `EditorManager` subscribes to.  Currently `PanelManager` only computes the Height and passes that as data but it needs to also compute Width and pass that as well so that the `EditorPaneManager` can verify that the width doesn't get too lean to handle 2 editor instances.  At the point that it is too lean then it will need to start resizing other columns to get a decent layout.  This algorithm becomes more complex with more columns and rows.  For now it can be a matter of going to something like 50%.  We may want to bump up the shell's minimum width as well to avoid getting too small.

These are proposed APIs that may not be implemented initially.  

## EditorPaneManager.setColumnWidth(_column_, _width_)  
## EditorPaneManager.setRowHeight(_row_, _height_)  

_height_ and _width_ are passed in in pixels and converted to a percentage when affixing the CSS to the columns (e.g. `width: 40%`).  Doing it in a percentage and only applying to all except the rightmost column and bottom most row will yield a fluid layout.  The API will reject setting the width on the rightmost column.  For the initial implementation we may just go with 50% splits all around without the ability to resize. 

`EditorManager` will manage a Working Set for each of its Editor Panes.  Management of the Working Set will move from the `DocumentManager` into `EditorManager` the and the working set will no longer be a collection of `Document` objects.  It will be an array of file names.  Theoretically we could register a view factory to create a view object for each file type (images, html, css, etc...) which would provide an easy way for custom viwers.  This could be extensible in some way but that work is outside the scope of this document.  For the time being, we only manifest CODE and Image viewers.

## EditorManager.getWorkingSet(_row_, _column_)  
## EditorManager.addToWorkingSet(_row_, _column_, _file_, _open_)  
## EditorManager.removeFromWorkingSet(_row_, _column_, _file_)  
## EditorManager.getAllWorkingSets()   

### WORKING SET CHANGES 

# Deprecate `DocumentManager.getWorkingSet()  ` 
A deprecation warning will be added and the function will return a unique list of all documents (not files) from all working sets by calling the various functions above, flattening and filtering the list as necessary to maintain backwards compatibility.

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

# Working Set Context Menus
This currently works by listening to `contextmenu` events on the `#open_files_container`.  This will change to listen to `contextmenu` events on a `.open_files_container` and the `EditorManager` who manages the `.open_files_container` will be passed with the `EventData` so that callers (Extensions) will be able to determine which `EditorManager` is in focus.  

This will also trigger a focus action on the DOM node causing the `Editor` to gain focus.  The Default extension, `CloseOthers`, will be retooled to work on the working set for the currently focused `EditorManager` instance.

# Working Set Management
The Implementation of these functions will move from `DocumentManager` to `EditorManager` and a deprecation warning will be displayed.

## EditorManager.findInWorkingSet
Used by (pflynn.brackets.editor.nav) which has a few other working set api calls 

## EditorManager.findInWorkingSetAddedOrder
** NOT USED BY EXTENSIONS **

## EditorManager.addToWorkingSet

```text
---------------------------+-----------------------------------------+-----------------------------------  
 Name                      | Usage                                   | Proposed Change                    
---------------------------+-----------------------------------------+-----------------------------------  
alexsalsas.brackets-jekyll | Create a git hub page...                | OK to use deprecated API
                           | Adds "GemFile" to the workingset & sets | 
                           | the current document to <curProjectDir>/| 
                           | "GemFile"                               | 
---------------------------+-----------------------------------------+-----------------------------------  
brackets-reopener          | Keeps track of recently opened files    | Problematic at best with many
                           | Reopens the last one closed             | working sets.  reopened files 
                           | (many working set apis and events used) | would reopen in the current pane
                           |                                         | Probably OK to use deprecated APIs
---------------------------+-----------------------------------------+-----------------------------------  

In addition to those two extensions, several other extensions make use of FileViewController.addToWorkingSetAndSelect.  
```

## EditorManager.addListToWorkingSet

```text
---------------------------+-----------------------------------------+-----------------------------------  
 Name                      | Usage                                   | Proposed Change                    
---------------------------+-----------------------------------------+-----------------------------------  
brackets-reopener          | Keeps track of recently opened files    | Problematic at best with many
                           | Reopens the last one closed             | working sets.  reopened files 
                           | (many working set apis and events used) | would reopen in the current pane
                           |                                         | Probably OK to use deprecated APIs
---------------------------+-----------------------------------------+-----------------------------------  
```

## EditorManager.removeFromWorkingSet

```text
---------------------------+-----------------------------------------+-----------------------------------  
 Name                      | Usage                                   | Proposed Change                    
---------------------------+-----------------------------------------+-----------------------------------  
Close-Others               | Adds Close commands the Working Set Menu| No change Needed. The extension is  
                           |                                         | packaged to run only on brackets  
                           |                                         | <= 0.32.0   
---------------------------+-----------------------------------------+----------------------------------- 
```

## EditorManager.removeListFromWorkingSet
** NOT USED BY EXTENSIONS **

## EditorManager.swapWorkingSetIndexes
** NOT USED BY EXTENSIONS **

## EditorManager.sortWorkingSet
** NOT USED BY EXTENSIONS **

# Supporting Images

Working set so far has be devoid of any image files.  To support images in split view, we will need to change the working set rules to allow for images to be in the working set. This means that callers of the new working set APIs will need to check to make sure they can operate on a file by getting its file type from the language manager or checking the extension. 

The working set will basically just be a list of files that may or may not have a Document object owned by the Document Manager.  The deprecated `DocumentManager.getWorkingSet()` will filter out any image files without a Document object.

# Events


```text
-----------------------------+-----------------------------------------+-----------------------------------  
 Name                        | Usage                                   | Proposed Change                    
-----------------------------+-----------------------------------------+-----------------------------------  
workingSetSort               | Fires when the order of the working set | Moves from DocumentManager to 
                             | changes                                 | EditorManager.  Data about which                  
                             | ** NOT USED BY EXTENSIONS **            | working set is added to event data
-----------------------------+-----------------------------------------+-----------------------------------  
workingSetAdd                | Fires when an item was added to the     | Moves from DocumentManager to 
                             | working set                             | EditorManager Data about which                  
                             | * TypeScript Extension to maintain      | working set is added to event data
                             |   integrity between its working set list|
                             |   and Brackets working set list         | The related files extension may
                             | * Reopener Extension (see above)        | be a problem but it appears to be
                             | * Related Files Extension (josh hatwich)| broken. There is at least 1 
                             |   appears he's using this event to know | deprecation warning and it looks 
                             |   when a file is opened in the editor   | like some callback data has 
                             |                                         | changed in the 8 months since it 
                             |                                         | was last touched. issue added.
                             |                                         | Related files works quite hevily
                             |                                         | with the working set apis, views
                             |                                         | and events which may stop working
                             |                                         | after changes are made to handle
                             |                                         | multiple working sets and views
                             |                                         | 
                             |                                         | The other two should be ok with
                             |                                         | changes to the event object
-----------------------------+-----------------------------------------+-----------------------------------  
workingSetAddList            | List added to working set               | (same as above)
                             | Oddly, the reopener extension does not  | 
                             | subscribe to this event.                | 
                             | Related Files and Typescript extensions | 
                             | subscribe to this event                 | 
-----------------------------+-----------------------------------------+-----------------------------------  
workingSetRemove             | Item removed from working set           | (same as above)
                             | Repopener, Typescript subscribe         | 
                             |  wdminimap - hides the minimap on event | 
-----------------------------+-----------------------------------------+-----------------------------------  
workingSetRemoveList         | List removed from working set           | (same as above)
-----------------------------+-----------------------------------------+-----------------------------------  
workingSetSort               | List sorted                             | (same as above)
-----------------------------+-----------------------------------------+-----------------------------------  
workingSetDisableAutoSorting | Sorting disabled due to swapping item   | (same as above)
                             | indices (manual reorder during drag)    |
-----------------------------+-----------------------------------------+-----------------------------------  
workingSetDisableAutoSorting | List removed from working set           | (same as above)
-----------------------------+-----------------------------------------+-----------------------------------  
```


# Additional Changes

`DocumentManager.getCurrentDocument()` is deprecated but maps to `EditorManager.getFocusedPane().getEditor().getDocument()`  since `DocumentManager` will no longer maintain the "current document".


# Implementing the Layout Manager

The initial implementation will be 2 columns x 1 row or 2 rows x 1 column.  However, implementing an arbitrary number of rows and columns could be trivial to do using this code:
https://github.com/FriendCode/codebox/blob/master/client/views/grid.js which is Apache-licensed.
However, this is a fairly integral piece to codebox and depends on other codebox libraries in order to work.


# Performance Testing

Do early research on typing and scrolling performance.




