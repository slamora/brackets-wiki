Most of Brackets' inner workings will need some sort of change to make SplitView work.  For the most-part, Brackets will operate as it does today and the chages will be transparent to extension authors.  This is a guide of what extension authors should do to maintain compatibility with future versios of Brackets.

Here are the high-level changes that Extension Authors need to keep in mind:

* Brackets may have more than one full sized editor visible at any given time so Extension Authors must be aware that the API, `DocumentManager.getCurrentDocument()`, will only give you the document of the currently focused editor.  This is also a deprecated API so extension authors are encouraged to migrate to `EditorManager.getFocusedEditor().getDocument()`  

* Brackets will no longer have a single working set.  Each "Editor Pane" as we're calling it, will have its own working set.  This is probably the most disruptive change because the working set is used quite extensively to monitor various events and provide useful feedback to the user about open documents.  Extension authors can use `EditorManager.getWorkingSet(FOCUSED_PANE)` to get the focused editor's working set or use `editorManager.getAllWorkingSets()` to get all working sets.  The latter of which returns an array of arrays.

* Working sets will no longer contain only filenames that have `Document` objects so extension authors will need to be able to handle the case where `DocumentManager.getDocumentForPath(workingSet[0])` may return `null`.

* `EditorManager` may manage several visible editors so the legacy APIs on `EditorManager` will work only for the `FOCUSED_PANE`'s editor.

All `DocumentManager` Workingset APIs will continue to work for some time but these are deprecated and will eventually be removed. Below is a chart of used APIs and the reccommended upgrade path.

## Changes to Working Set APIs

```text
-------------------------------------------+----------------------------------------------------------------------------------
API                                        |  Recommended Substitution
-------------------------------------------+----------------------------------------------------------------------------------
DocumentManager.findInWorkingSet           |  EditorManager.findInWorkingSet(EditorManager.ALL_PANES) 
-------------------------------------------+----------------------------------------------------------------------------------
DocumentManager.getCurrentDocument()       |  EditorManager.getFocusedEditor().getDocument()              
-------------------------------------------+----------------------------------------------------------------------------------
DocumentManager.getWorkingSet()            |  EditorManager.getWorkingSet(EditorManager.FOCUSED_PANE)         
-------------------------------------------+----------------------------------------------------------------------------------
DocumentManager.addToWorkingSet()          |  EditorManager.addToWorkingSet(EditorManager.FOCUSED_PANE)         
-------------------------------------------+----------------------------------------------------------------------------------
DocumentManager.addListToWorkingSet()      |  EditorManager.addListToWorkingSet(EditorManager.FOCUSED_PANE)         
-------------------------------------------+----------------------------------------------------------------------------------
DocumentManager.removeFromWorkingSet()     |  EditorManager.removeFromWorkingSet(EditorManager.ALL_PANES)         
-------------------------------------------+----------------------------------------------------------------------------------
```
The existing single-workingset APIs (`DocumentManager.getWorkingSet`, `DocumentManager.addToWorkingSet`, etc...) are still available but deprecated. They are convenence functions that delegate the work to the EditorManager and operate on the currently focused editor only.

The new Working Set APIs take an identifier of the pane containing the working set to work on .  These APIs will have a `paneId` as the first argument which is gotten from `EditorManager.createPane()` or from the event `EditorManager.editorPaneCreated`. There are, however, 2 shortcut constants you can use to make life simpler:

```text
------------------------+-----------------------------------------------------------------------------------------------------
Constant                | Usage
------------------------+-----------------------------------------------------------------------------------------------------
ALL_PANES               | Perform the operation on all panes (e.g. search for fullpath in all working sets)
FOCUSED_PANE            | Perform the operation on the currently focused pane (can also use EditorManager.getFocusedPane())
------------------------+-----------------------------------------------------------------------------------------------------
```

`findInWorkingSet` will change to return an object {pane: paneId, index: index} or undefined if the file was not found.

## Changes to Working Set Events

Working Set Events are currently fired by `DocumentManager`.  `DocumentManager` will continue to fire the following events but they are being deprecated and will stop working in a future version of Brackets.  A deprecation warning will display once when these events are fired:

```text
-------------------------------------------+-----------------------------------------------------------------------------------
Event Name                                 |  Recommended Substitution
-------------------------------------------+-----------------------------------------------------------------------------------
DocumentManager.workingSetAdd              |  EditorManager.workingSetAdd
-------------------------------------------+-----------------------------------------------------------------------------------
DocumentManager.workingSetAddList          |  EditorManager.workingSetAddList         
-------------------------------------------+-----------------------------------------------------------------------------------
DocumentManager.workingSetRemove           |  EditorManager.workingSetRemove   
-------------------------------------------+-----------------------------------------------------------------------------------
  
```


## Changes to EditorManager APIs
-------------------------------------------+-----------------------------------------------------------------------------------
API                                        |  Recommended Substitution
-------------------------------------------+-----------------------------------------------------------------------------------
EditorManager.getCurrentlyViewedPath       |  EditorManager.getFocusedPane().getCurrentlyViewedPath()
-------------------------------------------+-----------------------------------------------------------------------------------
```
