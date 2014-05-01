Refactoring Brackets to Support Split View with Multiple Documents requires quite a bit of hacking on the plumbing but there are only a few places that need to be heavily refactored to do so.  To break this work up in to smaller pieces, an accompanying document [SplitView Architecture Tasking](SplitView-Architecture-Tasking) has been created.

# Proposed Implementation 
This proposal calls on `EditorManager` to manage all viewable files and their supporting data. This includes Editor or Read Only Viewer Placement, Working Sets and Editor or Read Only Viewer Instances.  `DocumentManager` is refactored somewhat to remove Working Set management -- although some legacy APIs, Events, Functions, Commands, etc..., will remain for some time to maintain backwards compatibility.  

Those items that are needed to maintain backwards compatibility have been identified and documented in the [SplitView Extension Migration Guide](SplitView-Extension-Migration-Guide).

This proposal is an overview of the system along with functional details and implementation changes needed to build the SplitView feature.

## Editor APIs

### EditorManager.getFocusedEditor
Reimplemented by moving current Implementation into `Editor` and invoking `Editor.getFocusedEditor()` for the `Editor` object of the focused pane.

### EditorManager.getCurrentlyViewedPath
Reimplemented by moving current impl into `Editor` and invoking `Editor.getCurrentlyViewedPath` for the `Editor` object of the focused pane.

### EditorManager.createCustomViewerForFile
Creates a Read Only viewer for any [image] file.  
Replaces `EditorManager._showCustomViewer`, current implementation of ._showCustomViewer is moved to `Editor` as `createCustomViewerForFile` and invoked from `EditorManager`.  
_this function may not be necessary as the command handler reimplementation for opening a file may change to support images in a different way.  This is an implementation detail that will be decided on when the feature is implemented_

### EditorManager.getCurrentFullEditor
Reimplemented as `EditorManager.getFocusedEditor().getCurrentFullEditor()`

## View Layout APIs

### EditorManager.setLayoutScheme(_rows_,_columns_)  
This API will change the layout to match _rows_ and _columns_.

### EditorManager.setColumnWidth(_column_, _width_)  
### EditorManager.setRowHeight(_row_, _height_)  
Update pane height/width. Not initially implemented.

# Working Sets
The Implementation of these functions will move from `DocumentManager` to `EditorManager`. See the section at the bottom of this document for a list of deprecated Working Set APIs that need to be kept for backwards compatibility.

## Working Set APIs

Working Set APIs have been migrated from DocumentManager. Some of these will APIs will continue to exist in DocumentManager to maintain backwards compatibility.  These have the same functionality as in previous versions of Brackets except they will take a paneId to identify which pane's working set to work on.

Most of the Working Set APIs will take one of these special constants for paneId:
```text
------------------------+-----------------------------------------------------------------------------------------------------
Constant                | Usage
------------------------+-----------------------------------------------------------------------------------------------------
ALL_PANES               | Perform the operation on all panes (e.g. search for fullpath in all working sets)
FOCUSED_PANE            | Perform the operation on the currently focused pane (can also use EditorManager.getFocusedPane())
------------------------+-----------------------------------------------------------------------------------------------------
```
### EditorManager.getWorkingSet(paneId)  
### EditorManager.addToWorkingSet(paneId, _file_, _open_)  
### EditorManager.addListToWorkingSet(paneId, _files_)
### EditorManager.removeFromWorkingSet(paneId, _file_) 
### EditorManager.removeListFromWorkingSet(paneId, _files_)
### EditorManager.swapWorkingSetIndexes(paneId, _files_)
### EditorManager.sortWorkingSet(paneId, _files_)

### EditorManager.findInWorkingSet(paneId, _file_)  
### EditorManager.findInWorkingSetAddedOrder(paneId, _file_)    
Returns {paneId: _paneId_, index: _index_) or undefined if not found

## Working Set Events  
_EditorManager Events will add paneId to the event data_

### EditorManager.workingSetSort
### EditorManager.workingSetAdd
### EditorManager.workingSetAddList
### EditorManager.workingSetRemove
### EditorManager.workingSetRemoveList
### EditorManager.workingSetDisableAutoSorting

### EditorManager.editorPaneCreated
### EditorManager.editorPaneDestroyed
### EditorManager.activePaneChanged

### EditorManager.activeEditorChanged
### EditorManager.fullEditorChanged

### EditorManager.currentlyViewedFileChanged 

## Commands

The following list of commands will move from `DocumentCommandHandlers` along with their corresponding implementation into a new module -- `FileCommandHandlers`.  It may be easier to leave some of the `Document` object specific code handling in `DocumentCommandHandlers` and wire it up to `FileCommandHandlers` when working with Files that have Document object but a cursory review indicated that this wasn't necessary.  This is an implementation detail that will be decided when the work is actually done.

### file.addToWorkingSet
### file.open	
### file.rename	
### file.delete
### file.close
### file.closeAll
### file.closeList
### navigate.nextDoc
### navigate.prevDoc
### navigate.showInFileTree

# Implementing the Layout Manager

The initial implementation will be 2 columns x 1 row or 2 rows x 1 column.  However, implementing an arbitrary number of rows and columns could be trivial using this code:
https://github.com/FriendCode/codebox/blob/master/client/views/grid.js which is Apache-licensed.
However, this is a fairly integral piece to codebox and depends on other codebox libraries in order to work.
The initial implementation will be mostly handled by `EditorManager` but an `EditorLayoutManager` object will be created just to help handle the layout.  Since it's 1x1, 1x2 or 2x1 initially, it should be fairly easy to implement without the need for advanced layout mechanics.  
**Layout Rules:**
* Only 1 row and 2 columns or 2 rows and 1 column are initially supported
* Any Pane created by this API initially will show the Brackets logo interstitial screen until the corresponding `EditorManager` for that pane has been loaded with a document or image.
* When an editor pane is destroyed, all documents in the corresponding working set for that pane are moved to another pane's working set.  Since there is only 2 panes in the initial implementation this is just a matter of collapsing them down to the remaining Pane's working set.

Creating a pane is rendered at runtime and inserted it into the DOM.  The `Editor` Instance will generate the HTML when the `EditorManager` asks for it and insert it into the DOM in the appropriate place to ensure proper keyboard navigation.  The generated HTML can either come from a template rendered with Mustache or simple jQuery insertion.

`EditorManager` will handle the `PanelManager`'s resize event and create an `EditorLayoutManager` object to assist with the layout. 

The `PanelManager` computes the new size of `#editory-layout-holder` in its `window.resize` event handler and triggers an event to resize that `EditorManager` subscribes to.  Currently `PanelManager` only computes the Height and passes that as data but it needs to also compute Width and pass that as well so that the `EditorLayoutManager` object can verify that the editor holder doesn't get too narrow to handle 2 editor instances.  At the point that it is too narrow then it will need to start resizing other columns to get a decent layout.  This algorithm becomes more complex with more columns and rows.  For now it can be a matter of going to something like 50%.  We may want to bump up the shell's minimum width as well to avoid getting too small.

## Column Width/Row Height Rules
_height_ and _width_ are expressed in percentages when affixing the CSS to the columns (e.g. `width: 40%`).  Doing it in a percentage and only applying to all except the rightmost column and bottom most row will yield a fluid layout.  The API will reject setting the width on the rightmost column.  For the initial implementation we may just go with 50% splits all around without the ability to resize. 

#Implementing Pane Management
`EditorManager` will manage a Working Set for each of its editor panes. This may be abstracted and delegated into a `EditorPane` object if implementation starts to get to messy but the API to get the working set will be on `EditorManager` to make the interface easier to use. Management of the Working Set will move from the `DocumentManager` into `EditorManager` the and the Working Set will no longer be a collection of `Document` objects.  It will be a collection of file names.  

Note: To abstract the working set's pane location, each editor pane is addressed by paneId rather than row,col.  This is a change from the previous draft which had row, col addressable panes.  Valid paneId values cannot be `false, 0, null, undefined or ""` so that they can be used in `truthy` tests.
 This allows for:   

1) Panes to be found in the DOM by ID  
2) Easier to move panes around when changing layouts in the future and not break API  
3) Less data that callers need to understand about the implementation details  
The shortcut paneIds for Working Set APIs avoid having to maintain a reference to the pane in which a file belongs.  It also allows us to create 1 API rather than 2 for `All` and `Focused` derivatives.

# Implementing WorkingSetViews
 WorkingSetView objects are created when the event `editorPaneCreated` is handled.  `SideBarView` will handle this event and create a `WorkingSetView` object which is bound to the working set created for the pane and passed in as event data.
`#open-files-container` is a container which contains one or more `.working-set-container` divs in the DOM. Several 3rd Party Extensions rely or use the `#open-files-container` div. The extensions which style the elements will continue to work. The following extensions may no longer work:

# Supporting Images in Working Sets

To support images in split view, we will need to change the Working Set rules to allow for images in Working Sets. This means that callers of the new Working Set APIs will need to check to make sure they can operate on a file by getting its file type from the language manager or by checking its extension. 

The working set will basically just be a list of files that may or may not have a Document object owned by the Document Manager.  The deprecated API, `DocumentManager.getWorkingSet()`, will filter out any files that don't have an associated `Document` object.


# Implementing WorkingSet Context Menus
This currently works by listening to `contextmenu` events on the `#open_files_container`.  This will change to listen to `contextmenu` events on an `.open_files_container` and the `WorkingSetView` who attached to the `.open_files_container` will be maintain the paneId from the `EventData` it was passed during the create event so that callers (Extensions) will be able to determine which editor has focus when the menu is invoked.

This will also trigger a focus action on the DOM node causing the `Editor` to gain focus.  The Default extension, `CloseOthers`, will be retooled to work on the working set for the editor pane associated with the `WorkingSetView` that manages it and ask that `EditorManager` instance for the working set.

```javascript

DefaultMenus:
        $("#open-files-container").on("contextmenu", function (e) {
            working_set_cmenu.open(e);
        });

Becomes:
        $(".working-set-container").on("contextmenu", function (e) {
            e.workingSetPaneId = this._paneId;
            working_set_cmenu.open(e);
        });

```


# Deprecating Legacy APIs
```text
---------------------------------------------------------------------+-------------------------------------------------------------------
API                                                                  | Calls...
---------------------------------------------------------------------+-------------------------------------------------------------------
DocumentManager.getCurrentDocument()                                 | EditorManager
                                                                     |    .getFocusedEditor()
                                                                     |    .getDocument();
---------------------------------------------------------------------+-------------------------------------------------------------------
DocumentManager.getWorkingSet()                                      | $.map(EditorManager.getWorkingSet(ALL_PANES, …),
                                                                     |        function(fullPath) {
                                                                     |           if (DocumentManager.isDocument(fullPath)) {
                                                                     |               return fullPath; 
                                                                     |        }
                                                                     | });
---------------------------------------------------------------------+-------------------------------------------------------------------
DocumentManager.findInWorkingSet()                                   | return EditorManager
*only used by pflynn                                                 |    .findInWorkingSet(ALL_PANES, ...).index || -1;
---------------------------------------------------------------------+-------------------------------------------------------------------
DocumentManager.addToWorkingSet()                                    | return EditorManager
                                                                     |    .addToWorkingSet(FOCUSED_PANE, ...);
---------------------------------------------------------------------+-------------------------------------------------------------------
DocumentManager.addListToWorkingSet                                  | return EditorManager
                                                                     |    .addListToWorkingSet(FOCUSED_PANE, ...);
---------------------------------------------------------------------+-------------------------------------------------------------------
DocumentManager.removeFromWorkingSet                                 | return EditorManager
                                                                     |    .removeFromWorkingSet(ALL_PANES, ...);
---------------------------------------------------------------------+-------------------------------------------------------------------

```

# Deprecating Legacy Events
The following Events will be kept on the `DocumentManager` object to maintain backwards compatibility but will just be a repub of the EditorManager events. 
__Is there a way to know if there are any listeners so that a deprecation warning can be written to the console only if there are listeners?__

## workingSetAdd                
## workingSetAddList            
## workingSetRemove             

# Opportunistic Cleanup

The following `EditorManager` functions will move to `Editor` 

```text
-----------------------------------+-----------------------------------------+-----------------------------------
 Name                              | Usage                                   | Disposition                  
-----------------------------------+-----------------------------------------+-----------------------------------
_openInlineWidget                  | Internal Inline Widget Management       | Moves to Editor without inpunity
_openInlineWidget                  |                                         | 
-----------------------------------+-----------------------------------------+-----------------------------------
_toggleInlineWidget                | Command Handler                         | Current Implementation moves to 
                                   |                                         | Editor.
                                   |                                         | Command handler will  call
					        	   |                                         |   _focusedEditor
								   |                                         |    .toggleInlineWidget()
-----------------------------------+-----------------------------------------+-----------------------------------
_showCustomViewer                  | API                                     | Illegal usage from 
                                   |                                         |   DocumentCommandHandlers
								   |                                         | Command handler moves to 
								   |                                         |  FileCommandHandlers
								   |                                         | Function is renamed to 
								   |                                         |  EditorManager
								   |                                         |    createViewerForFile
-----------------------------------+-----------------------------------------+-----------------------------------
closeInlineWidget                  |                                         | Moves to Editor
                                   |                                         | InlineWidget.close will call
								   |                                         | this.hostEditor.closeInlineWidget()
-----------------------------------+-----------------------------------------+-----------------------------------
getInlineEditors             	   |                                         | Moves to Editor
                             	   |                                         | InlineWidget
								   |                                         |    ._syncGutterWidths(
								   |                                         |                hostEditor
								   |                                         | ) 
								   |                                         | calls hostEditor.getInlineEditors()
-----------------------------------+-----------------------------------------+-----------------------------------
getFocusedInlineWidget             |                                         | Doesn't move but passes through to
                                   |                                         |    getFocusedEditor()
								   |                                         |      .getFocusedInlineWidget();
-----------------------------------+-----------------------------------------+-----------------------------------
                             


