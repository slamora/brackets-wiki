Refactoring Brackets to Support Split View with Multiple Documents requires quite a bit of hacking on the plumbing but there are only a few places that need to be heavily refactored to do so.  To break this work up in to smaller pieces, an accompanying document [SplitView Architecture Tasking](SplitView-Architecture-Tasking) has been created.

# Proposed Implementation 
This proposal calls for anything that wants to show a view in the "Editor Workspace Area" must be a registered View Provider.  There are other ways to get views into this area but deserialization requires a method for views to be reconstructed at startup and this is done through a Registered View Provider. Registered View Providers expose methods to create a view for a given URI.  A ViewFactory will be responsible for maintaining the View Provider registry and invoke its `createView` when a View Provider indicates that it can create a viewer for a URI.

Initially there will be 2 view providers: an Image provider and a Document Editor provider.  `EditorManager` will be the registered view provider for creating all editable document views.

Workingset management moves from `DocumentManager` to `MainViewManager` -- although some legacy APIs, Events, Functions, Commands, etc..., will remain for some time to maintain backwards compatibility.  

Those items that are needed to maintain backwards compatibility have been identified and documented in the [SplitView Extension Migration Guide](SplitView-Extension-Migration-Guide) and are detailed in the section below on [Deprecating Legacy APIs](#deprecating-legacy-apis)

This proposal is an overview of the system along with functional details and implementation changes needed to build the SplitView feature.

## High Level Block Diagram
![SplitView Architecture ](images/splitviewarch.jpg)

## EditorManager APIs
### EditorManager.getFocusedEditor
Reimplemented by moving current Implementation into `Editor` and invoking `Editor.getFocusedEditor()` for the `Editor` object of the focused pane.  The API will continue to work as it does today.

### EditorManager.getCurrentlyViewedPath
Reimplemented by moving current impl into `Editor` and invoking `Editor.getCurrentlyViewedPath` for the `Editor` object of the focused pane. The API will continue to work as it does today.

### EditorManager.getCurrentFullEditor
Reimplemented as `EditorManager.getFocusedEditor().getCurrentFullEditor()`

## View Layout APIs
### MainViewManager.setLayoutScheme(_rows_,_columns_)  
This API will change the layout to match _rows_ and _columns_.

### MainViewManager.setColumnWidth(_column_, _width_)  
### MainViewManager.setRowHeight(_row_, _height_)  
Update pane height/width. Not initially implemented.

## View APIs
## MainViewManager.addView(_paneID_, _view_, _context_) 
This is how the viewFactory will add things but components can add views as necessary.

_view_ required interface

```javascript
/** @type {string} **/
var title;

/** @type {boolean} **/
var modified;

function getHTML();
@returns {string}
```

## MainViewManager.registerViewProvider(_provider_)

_provder_ is an object with the following interface:

```javascript
/** @type {string} **/
var displayname;

function canDecode(_uri_)
@returns {boolean}

function createViweFor(_uri_)
@return {$(object)}

```

# Workingsets
The Implementation of these functions will move from `DocumentManager` to `MainViewManager`. See the section at the bottom of this document for a list of deprecated Workingset APIs that need to be kept for backwards compatibility. This is done primarily because there will be multiple working sets (1 per pane)  and `EditorManager` manages the panes so it makes sense to move it there. But, architecturally, it makes sense the Working Set is a property of the pane because it's really the structure of the UI -- **not the document**.

Extension Authors and 3rd party developers are encouraged to use other methods whenever possible to work with the set of open documents.  See the [SplitView Extension Migration Guide](SplitView-Extension-Migration-Guide) for more information.

## Workingset APIs
Workingset APIs we be migrated from `DocumentManager`. Some of these will APIs will continue to exist in `DocumentManager` to maintain backwards compatibility.  These have the same functionality as in previous versions of Brackets except they will take a `paneId` to identify which pane's Workingset to work on.

Most of the Workingset APIs will take one of these special constants for `paneId` in addition to valid paneIds:
```text
------------------------+-----------------------------------------------------------------------------------------------------
Constant                | Usage
------------------------+-----------------------------------------------------------------------------------------------------
ALL_PANES               | Perform the operation on all panes (e.g. search for fullpath in all Workingsets)
FOCUSED_PANE            | Perform the operation on the currently focused pane (can also use MainViewManager.getFocusedPane())
------------------------+-----------------------------------------------------------------------------------------------------
```
### MainViewManager.getWorkingSet(_paneId_)  
### MainViewManager.addToWorkingSet(_paneId_, _file_, _open_)  
### MainViewManager.addListToWorkingSet(_paneId_, _files_)
### MainViewManager.removeFromWorkingSet(_paneId_, _file_) 
### MainViewManager.removeListFromWorkingSet(_paneId_, _files_)
### MainViewManager.swapWorkingSetIndexes(_paneId_, _files_)
### MainViewManager.sortWorkingSet(_paneId_, _files_)

### MainViewManager.findInWorkingSet(_paneId_, _file_)  
### MainViewManager.findInWorkingSetAddedOrder(_paneId_, _file_)    
Returns {paneId: _paneId_, index: _index_) or undefined if not found

## Workingset Events  
_MainViewManager Events will add `paneId` to event data_

### MainViewManager.workingSetCreated
### MainViewManager.workingSetDestroyed
### MainViewManager.workingSetSort
### MainViewManager.workingSetAdd
### MainViewManager.workingSetAddList
### MainViewManager.workingSetRemove
### MainViewManager.workingSetRemoveList
### MainViewManager.workingSetDisableAutoSorting

## Pane Events
### MainViewManager.paneCreated
### MainViewManager.paneDestroyed
### MainViewManager.activePaneChanged
### MainViewManager.currentlyViewedFileChanged 

## EditorManager Events (provided for backwards compatibility but adds PaneId as Event Data)
### EditorManager.activeEditorChanged
### EditorManager.fullEditorChanged

## ProjectManager Events
### ProjectManager.openFileListChanged

# Implementing the Layout Manager
The initial implementation will be mostly handled by `MainViewManager` but an `ViewLayoutManager` object may be created just to help handle the layout.  We shouldn't need to build for advanced layout mechanics since we only need, at most, 2 panes.  Support for arbitrary rows and columns can be built into the `ViewLayoutManager` flyweight at a later date.

Whether or not the initial implementation uses an `ViewLayoutManager` is an implementation detail that will be decided when the feature is implemented.

**Layout Rules:**
* Only 1 pane or 1 row and 2 columns or 2 rows and 1 column are initially supported
* Any Pane created by this API initially will show the Brackets logo interstitial screen until the corresponding `EditorManager` for that pane has been loaded with a document or image.
* When an editor pane is destroyed, all documents in the corresponding Workingset for that pane are moved to another pane's Workingset.  Since there is only 2 panes in the initial implementation this is just a matter of collapsing them down to the remaining Pane's Workingset.  This will generate a workingSetAddList event with the working set passed as event data.

Creating a pane is rendered at runtime and inserted it into the DOM.  The `Editor` Instance will generate the HTML when the `EditorManager` asks for it and insert it into the DOM in the appropriate place to ensure proper keyboard navigation.  The generated HTML can either come from a template rendered with Mustache or simple jQuery insertion.

PanelManager is being renamed to to WorkspaceManager. This will impact quite a few extensions. _Q: can require map "PanelManager" to "WorkspaceManager"? so the following code will continue to work from extensions:_

```javascript

   var panelManager = brackets.getModule("PanelManager");
```

Otherwise we would need to stub an exports to rewire `PanelManager` to `WorkspaceManager`.

`EditorManager` will handle the `WorkspaceManager`'s resize event and create an `EditorLayoutManager` object to assist with the layout. 

`WorkspaceManager` computes the new size of `#editor-holder` in its the `window.resize` event handler and triggers an event to resize that `EditorManager` subscribes to.  Currently `WorkspaceManager` only computes the Height and passes that as data but it needs to also compute Width and pass that as well so that the `EditorLayoutManager` object can verify that the editor holder doesn't get too narrow to handle 2 editor instances.  At the point that it is too narrow then it will need to start resizing other columns to get a decent layout.  This algorithm becomes more complex with more columns and rows.  For now it can be a matter of going to something like 50%.  We may want to bump up the shell's minimum width as well to avoid getting too small.

## Column Width/Row Height Rules
_height_ and _width_ are expressed in percentages when affixing the CSS to the columns (e.g. `width: 40%`).  Doing it in a percentage and only applying to all except the rightmost column and bottom most row will yield a fluid layout.  The API will reject setting the width on the rightmost column.  For the initial implementation we may just go with 50% splits all around without the ability to resize. 

#Implementing Pane Management
`ViewManager` will manage a Workingset for each of its editor panes. This may be abstracted and delegated into a `ViewPane` object if implementation starts to get to messy but the API to get the Workingset will be on `ViewManager` to make the interface easier to use. Management of the Workingset will move from the `DocumentManager` into `ViewManager` the and the Workingset will no longer be a collection of `Document` objects.  It will be a collection of file names.  

*NOTE:* To abstract the Workingset's pane location, each editor pane is addressed by _paneId_ rather than row,col.  Valid _paneId_ values cannot be `false, 0, null, undefined or ""` so that they can be used in `truthy` tests.

The shortcut _paneIds_ for Workingset APIs avoid having to maintain a reference to the pane in which a file belongs.  It also allows us to create 1 API rather than 2 for `All` and `Focused` derivatives.

# Implementing WorkingSetViews
 WorkingSetView objects are created when the event `editorPaneCreated` is handled.  `SideBarView` will handle this event and create a `WorkingSetView` object which is bound to the Workingset created for the pane and passed in as event data.

`#open-files-container` is a container which contains one or more `.working-set-container` divs in the DOM. Several 3rd Party Extensions rely or use the `#open-files-container` div. The extensions which style the elements will continue to work. 

# Supporting Images in Workingsets

To support images in split view, we will need to change the Workingset rules to allow for images in Workingsets. This means that callers of the new Workingset APIs will need to check to make sure they can operate on a file by getting its file type from the language manager or by checking its extension. 

The Workingset will basically just be a list of files that may or may not have a Document object owned by the Document Manager.  The deprecated API, `DocumentManager.getWorkingSet()`, will filter out any files that don't have an associated `Document` object.


# Implementing WorkingSet Context Menus
This currently works by listening to `contextmenu` events on the `#open_files_container`.  This will change to listen to `contextmenu` events on an `.open_files_container` and the `WorkingSetView` who attached to the `.open_files_container` will be maintain the paneId from the `EventData` it was passed during the create event so that callers (Extensions) will be able to determine which editor has focus when the menu is invoked.

This will also trigger a focus action on the DOM node causing the `Editor` to gain focus.  The Default extension, `CloseOthers`, will be retooled to work on the Workingset for the editor pane associated with the `WorkingSetView` that manages it and ask that `EditorManager` instance for the Workingset.

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

# Workingset Alternatives

## ProjectManager.getAllOpenFiles()
Returns a list of all open files

## DocumentManager.getAllOpenDocuments()
Returns a list of all open documents -- including documents which are in a workingset but not yet opened, documents which have been opened in inline editors but not part of a workingset and opened but not modified or added to any workingset.

# Deprecating Legacy APIs
```text
-------------------------------------+--------------------------------------------------------
API                                  | Calls...
-------------------------------------+--------------------------------------------------------
DocumentManager.getCurrentDocument() | EditorManager
                                     |    .getFocusedEditor()
                                     |    .getDocument();
-------------------------------------+--------------------------------------------------------
DocumentManager.getWorkingSet()      | $.map(EditorManager.getWorkingSet(ALL_PANES, …),
                                     |        function(fullPath) {
                                     |           if (DocumentManager.isDocument(fullPath)) {
                                     |               return fullPath; 
                                     |        }
                                     | });
-------------------------------------+--------------------------------------------------------
DocumentManager.findInWorkingSet()   | return EditorManager
*only used by pflynn                 |    .findInWorkingSet(ALL_PANES, ...).index || -1;
-------------------------------------+--------------------------------------------------------
DocumentManager.addToWorkingSet()    | return EditorManager
                                     |    .addToWorkingSet(FOCUSED_PANE, ...);
-------------------------------------+--------------------------------------------------------
DocumentManager.addListToWorkingSet  | return EditorManager
                                     |    .addListToWorkingSet(FOCUSED_PANE, ...);
-------------------------------------+--------------------------------------------------------
DocumentManager.removeFromWorkingSet | return EditorManager
                                     |    .removeFromWorkingSet(ALL_PANES, ...);
-------------------------------------+--------------------------------------------------------
```

# Deprecating Legacy Events
The following Events will be kept on the `DocumentManager` object to maintain backwards compatibility but will just be a repub of the EditorManager events. 


*Q:* _Is there a way to know if there are any listeners so that a deprecation warning can be written to the console only if there are listeners?_

## DocumentManager.workingSetAdd                
## DocumentManager.workingSetAddList            
## DocumentManager.workingSetRemove             
## DocumentManager.currentDocumentChange 
_Fired when handling `EditorManager.fullEditorChanged`_

# Opportunistic Cleanup

The following `EditorManager` functions will move to `Editor` and are opportunistic refactorings since we're working in that code.  These refactorings aid in the overall implementation but aren't necessary.

```text
------------------------+-----------------------------------+-------------------------------------
 Name                   | Usage                             | Disposition                  
------------------------+-----------------------------------+-------------------------------------
_openInlineWidget       | Internal Inline Widget Management | Moves to Editor without impunity
------------------------+-----------------------------------+-------------------------------------
_toggleInlineWidget     | Command Handler                   | Current Implementation moves to 
                        |                                   | Editor.
                        |                                   | Command handler will  call
					    |                                   |   getFocusedEditor()
						|                                   |    .toggleInlineWidget()
------------------------+-----------------------------------+-------------------------------------
_showCustomViewer       | API                               | Illegal usage from 
                        |                                   |   DocumentCommandHandlers
						|                                   | Command handler moves to 
						|                                   |  FileCommandHandlers
						|                                   | Function is renamed to 
						|                                   |  EditorManager
						|                                   |    createViewerForFile
------------------------+-----------------------------------+-------------------------------------
closeInlineWidget       |                                   | Moves to Editor
                        |                                   | InlineWidget.close will call
						|                                   | this.hostEditor.closeInlineWidget()
------------------------+-----------------------------------+-------------------------------------
getInlineEditors        |                                   | Moves to Editor
                        |                                   | InlineWidget
						|                                   |    ._syncGutterWidths(
						|                                   |                hostEditor
						|                                   | ) 
						|                                   | calls hostEditor.getInlineEditors()
------------------------+-----------------------------------+-------------------------------------
getFocusedInlineWidget  |                                   | Doesn't move but passes through to
                        |                                   |    getFocusedEditor()
						|                                   |      .getFocusedInlineWidget();
------------------------+-----------------------------------+-------------------------------------
                             
```

## Commands (opportunistic cleanup)

The following list of commands will move from `DocumentCommandHandlers` along with their corresponding implementation into a new module -- `ViewCommandHandlers`.  We will also remove "file." from the command name and rename the commands to "cmd.", deprecating the old command ids in the same fashion the find commands were deprecated using getters.

### cmd.addToWorkingSet
### cmd.open	
### cmd.rename	
### cmd.delete
### cmd.close
### cmd.closeAll
### cmd.closeList
### navigate.nextDoc
### navigate.prevDoc
### navigate.showInFileTree


Raw data can be found here:
[splitview architecture notes](splitview-architecture-notes)