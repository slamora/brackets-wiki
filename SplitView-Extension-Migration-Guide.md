Most of Brackets' inner workings will need some sort of change to make SplitView work.  For the most-part, Brackets will operate as it does today and the changes will be transparent to extension authors.  This is a guide of what extension authors should do to maintain compatibility with future versions of Brackets.

Here are the high-level changes that Extension Authors need to keep in mind:

* DocumentManager.getCurrentDocument() is being deprecated. Brackets may have more than one full sized editor visible at any given time so Extension Authors must be aware that the API only provides access to the document of the currently focused editor.  Extension authors are encouraged to migrate to `EditorManager.getFocusedEditor().getDocument()`.  

* `EditorManager.getFocusedEditor().getDocument()` may return `null`. There are many cases when this may happen so extension authors need to be prepared for this. 

* Brackets will no longer have a single Workingset.  Each "Editor Pane", as we're calling it, will have its own Workingset.  This is probably the most disruptive change because the Workingset is used quite extensively to monitor various events and provide useful feedback to the user about open documents.  Extension authors can use `EditorManager.getWorkingSet(FOCUSED_PANE)` to get the focused editor's Workingset or use `editorManager.getAllWorkingSets()` to get all Workingsets.  The latter of which returns an array of arrays.

* Workingsets will no longer contain only filenames that have `Document` objects so extension authors will need to be able to handle the case where `DocumentManager.getDocumentForPath(workingSet[0])` may return `null`.  To that effect, extension authors are strongly discouraged from directly using the workingset APIs as these may change over time.  This may not be entirely possible as extensions that work integrate with the WorkingSetView will need to use the working set.  There are several alternatives to getting the list of open files, documents, and the like, however, and those APIs are preferred. Extension Authors using the new workingset APIs should also be prepared to handle the case where a file may show up in more than one working set.

All `DocumentManager` Workingset APIs will continue to work for some time but these are deprecated and will eventually be removed. Below is a chart of used APIs and the recommended upgrade path.

## Changes to Workingset APIs

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
The existing single-workingset APIs (`DocumentManager.getWorkingSet`, `DocumentManager.addToWorkingSet`, etc...) are still available but deprecated. They are convenience functions that delegate the work to  `EditorManager` and operate on the currently focused editor only.

The new Workingset APIs take an identifier of the pane containing the Workingset to work on .  These APIs will have a `paneId` as the first argument which is gotten from `EditorManager.createPane()` or from the event `EditorManager.editorPaneCreated`. There are, however, 2 shortcut constants you can use to make life simpler:

```text
------------------------+-----------------------------------------------------------------------------------------------------
Constant                | Usage
------------------------+-----------------------------------------------------------------------------------------------------
ALL_PANES               | Perform the operation on all panes (e.g. search for fullpath in all Workingsets)
FOCUSED_PANE            | Perform the operation on the currently focused pane (can also use EditorManager.getFocusedPane())
------------------------+-----------------------------------------------------------------------------------------------------
```

Extension Authors should also be aware that moving `DocumentManager.findInWorkingSet()` to `EditorManager` will change to return an object `{pane: paneId, index: index}` or `undefined` if the file was not found.  `DocumentManager.findInWorkingSet()` will continue to work as it does today but a deprecation warning will be written to the console and it will only search using the `FOCUSED_PANE` derivative.

## Changes to WorkingSet Events

Workingset Events are currently fired by `DocumentManager`.  `DocumentManager` will continue to fire the following events but they are being deprecated and will stop working in a future version of Brackets. 

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
```text
-------------------------------------------+-----------------------------------------------------------------------------------
API                                        |  Recommended Substitution
-------------------------------------------+-----------------------------------------------------------------------------------
EditorManager.getCurrentlyViewedPath()     |  EditorManager.getFocusedPane().getCurrentlyViewedPath()
-------------------------------------------+-----------------------------------------------------------------------------------
```


# Workingset Alternatives

Extension authors are encouraged to use these new APIs in lieu of using the Workingset APIs whenever possible.  

## ProjectManager.getAllOpenFiles()
Returns a list of all open files

## DocumentManager.getAllOpenDocuments()
Returns a list of all open documents -- including documents which are in a workingset but not yet opened, documents which have been opened in inline editors but not part of a workingset and opened but not modified or added to any workingset.