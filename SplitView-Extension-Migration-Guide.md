As Kevin pointed out in this Week's ["Brackets Weekly"](http://blog.brackets.io/2014/08/18/brackets-weekly-episode-10/) SplitView is a pretty big change under the hood. So this guide is here to help extension authors migrate their extensions to the new paradigm and what extension authors should do to maintain compatibility with Brackets.

For the most-part, Brackets will operate as it does today with a "current" view -- even though there is more than 1 view visible. Most of the changes will be transparent to extension authors.  There are, however, a few things which extension authors should be aware of and a few things that extension authors should no longer do.

Here is a summary of the changes:

* Editors are not the only thing that live in the Main Viewing Area so `DocumentManger.getCurrentDocument()` and `EditorManager.getCurrentFullEditor()` may return `null`. This is not new but there may be more cases when this will happen so extension authors need to be prepared for this. 

* Brackets will no longer have a single Working Set.  Each `Pane` will manage a list of open files so extensions will need to identify which Working Set they wish to address when access the Working Set APIs.  For alternatives to using Working Sets, see the section below on #working set alternatives.

* Working Sets will contain `File` objects, as they did in previous versions, but the `File` objects may not map to `Document` objects as they did in the past. Extension authors will need to be able to handle non-document objects in Working Sets.

* `MainViewManager` manages all `Pane` objects and is the interface to accessing Views, Working Sets, etc... Most `MainViewManager` APIs require a `PaneId` to identify which `Pane` to operate on. There are 2 PaneIds that have special meaning: `ALL_PANES` and `ACTIVE_PANE`.  `ACTIVE_PANE` can be used on any API where a `PaneId` is required.  Some APIs, howerver, such as close, remove, find, etc... will take the `ALL_PANES` identifier to make traversal much easier.

* Custom View APIs are being deprecated in lieu of a View Factory Registry.  Custom View providers need to construct a factory object that can create views for files.

* `Editor` objects and ImageViewers are not direct descendants of the `#editor-holder` DOM node any more.  Extensions which manipulate the DOM at or under this node are probably going to stop working or cause stability and or usability issues for Brackets.


<h2>Changes to DocumentManager</h2>

These Events are Deprecated.  Extension authors should migrate to the recommended event.

<table>
<thead>
<tr>
  <td><b>Event</b></td><td><b>Reccomended event</b></td><td><b>Notes</b></td></tr>
</thead>
  <tr>
    <td>DocumentManager.workingSetAdd</td>
    <td>MainViewManager.paneViewAdd</td>
    <td>(1)</td>
  </tr>
  <tr>
    <td>DocumentManager.workingSetAddList </td>
    <td>MainViewManager.paneViewAddList</td>
    <td>(1)</td>
  </tr>
  <tr>
    <td>DocumentManager.workingSetRemove </td>
    <td>MainViewManager.paneViewRemove </td>
    <td>(1)</td>
  </tr>
  <tr>
    <td>DocumentManager.workingSetRemoveList </td>
    <td>MainViewManager.paneViewRemoveList </td>
    <td>(1)</td>
  </tr>
  <tr>
    <td>DocumentManager.workingSetSort </td>
    <td>MainViewManager.paneViewSort</td>
    <td>&nbsp;</td>
  </tr>
</table>
(1) Event will now be fired with File Object(s) that may not map to a Document  

_   
   

These APIs have been Deprecated and Extension Authors should migrate to the recommended API.

<table>
<thead>
<tr><td><b>API</b></td><td><b>Recommended API</b></td><td><b>Notes</b></td></tr>
</thead>
  <tr>
    <td>DocumentManager.findInWorkingSet()</td>
    <td>MainViewManager.findView(MainViewManager.ALL_PANES) </td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td>DocumentManager.getWorkingSet() </td>
    <td>MainViewManager.getViews(MainViewManager.ALL_PANES) or MainViewManager.getAllOpenFiles()</td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td>DocumentManager.addToWorkingSet() </td>
    <td>MainViewManager.addView(MainViewManager.FOCUSED_PANE) </td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td>DocumentManager.addListToWorkingSet() </td>
    <td>MainViewManager.addViews(MainViewManager.FOCUSED_PANE) </td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td>DocumentManager.removeFromWorkingSet() </td>
    <td>MainViewManager.removeView(MainViewManager.ALL_PANES) </td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td>DocumentManager.removeListFromWorkingSet() </td>
    <td>MainViewManager.removeViews(MainViewManager.ALL_PANES) </td>
    <td>&nbsp;</td>
  </tr>  
  <tr>
    <td>DocumentManager.setCurrentDocument(doc) </td>
    <td>MainViewManager.open(MainViewManager.ACTIVE_PANE, doc.file) </td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td>DocumentManager.closeAll() </td>
    <td>MainViewManager.closeAll(MainViewManager.ALL_PANES) </td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td>DocumentManager.closeFullEditor() </td>
    <td>MainViewManager.close(MainViewManager.ALL_PANES) </td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td>DocumentManager.beginDocumentNavigation() </td>
    <td>MainViewManager.beginTraversal() </td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td>DocumentManager.finalizeDocumentNavigation() </td>
    <td>MainViewManager.endTraversal() </td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td>DocumentManager.getNextPrevFile() </td>
    <td>MainViewManager.traverseViewsByMRU() </td>
    <td>&nbsp;</td>
  </tr>      
</table>

These Public APIs are no longer in use and were identified as not being used by extensions. They have been removed from Brackets.

<table>
<thead>
<tr><td><b>API</b></td><td><b>Notes</b></td></tr>
</thead>
  <tr>
    <td>sortWorkingSet()</td>
    <td>Not used by registered extensions</td>
  </tr>
  <tr>
    <td>swapWorkingSetIndexes()</td>
    <td>Not used by registered extensions</td>
  </tr>
  <tr>
    <td>findInWorkingSetAddedOrder()</td>
    <td>Not used by registered extensions</td>
  </tr>
</table>



<H3>Changes to EditorManager</H3>

These APIs have been Deprecated and Extension Authors should migrate to the recommended API.

<table>
<thead>
<tr><td><b>API</b></td><td><b>Recommended API</b></td><td><b>Notes</b></td></tr>
</thead>
  <tr>
    <td>DocumentManager.resizeEditor()</td>
    <td>WorkspaceManager.recomputeLayout()</td>
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td>EditorManager.getCurrentlyViewedPath()</td>
    <td>MainViewManager.getCurrentlyViewedPath()</td>
    <td>EditorManager.getCurrentlyViewedPath() will return NULL if the current file is an image or otherwise not a document.</td>
  </tr>
  <tr>
    <td>EditorManager.setEditorHolder()</td>
    <td>&nbsp;</td>
    <td>Used by 1 or 2 extensions for unit tests but now does nothing. Unit tests can construct a create a MainViewManager and initialize it to a DOM element. See MainViewManager._init()</td>
  </tr>  
  <tr>
    <td>EditorManager.registerCustomViewer()</td>
    <td>&nbsp;</td>
    <td>Used by 1 or 2 extensions but does nothing. The preverred method is to create a View Factory that can create views of a file.</td>
  </tr> 
  <tr>
    <td>EditorManager.focusEditor()</td>
    <td>MainViewManager.focusActivePane()</td>
    <td>Extensions should be prepared that ImageViews or other Views are focusable in the new model.</td>
  </tr>   
</table>

These Public APIs are no longer in use and were identified as not being used by extensions. They have been removed from Brackets.

<table>
<thead>
<tr><td><b>API</b></td><td><b>Recommended API</b></td><td><b>Notes</b></td></tr>
</thead>
  <tr>
    <td>EditorManager.notifyPathDeleted()</td>
    <td>&nbsp;</td>
    <td>Was makred for internal use only but not private. Not used by extensions.</td>
  </tr>
   <tr>
    <td>EditorManager.showingCustomViewerForPath()</td>
    <td>&nbsp;</td>
    <td>Not used by extensions. There is no equivelent.</td>
  </tr>
</table>

These Events have been removed.  

<table>
<thead>
<tr>
  <td><b>Event</b></td><td><b>Reccomended event</b></td><td><b>Notes</b></td></tr>
</thead>
  <tr>
    <td>EditorManager.getCurrentlyViewedFileChange</td>
    <td>MainViewManager.currentFileChange</td>
    <td>Not used by any registered extension</td>
  </tr>
</table>


These Private APIs have been removed but are being used by extensions.  Extensions using these APIs will break.

<table>
<thead>
<tr><td><b>API</b></td><td><b>Notes</b></td></tr>
</thead>
  <tr>
    <td>EditorManager._showCustomViewer()</td>
    <td>Register a View Factory then open the file that is to be used with the custom viewer.</td>
  </tr>
  <tr>
    <td>EditorManager._closeCustomViewer()</td>
    <td>No substition available. Extensions should register View Factories to create custom views and open a file assocatied with the factory object.  Closing that view would be a close command on the current view.</td>
  </tr>
</table>

<H3>Commands</H3>

The following commands have been added to support `File` objects.

<table>
<thead>
<tr><td><b>Legacy Command</b></td><td><b>New Command</b></td><td><b>Notes</b></td></tr>
</thead>
  <tr>
    <td>FILE_ADD_TO_WORKING_SET</td>
    <td>CMD_ADD_TO_PANE_AND_OPEN</td>
    <td>The new command will resolve to a File object while the legacy command resolves to a Document object.</td>
  </tr>
  <tr>
    <td>FILE_OPEN</td>
    <td>CMD_OPEN</td>
    <td>The new command will resolve to a File object while the legacy command resolves to a Document object.</td>
  </tr>
</table>

<H3>Pane Operations</H3>
Most of the `MainViewManager` APIs require a Pane ID to denote which pane the operation should target. Should it open the file in the left or right pane, for instance or locate a file in the top or bottom pane.  Extension authors can use the following special Pane Ids when addressing a pane:
<table>
<thead>
<tr>
  <td><b>Constant</b></td>
  <td><b>Functionality</b></td><td><b>Notes</b></td></tr>
</thead>
  <tr>
    <td>MainViewManager.ALL_PANES</td>
    <td>The operation traverses all panes </td>
    <td>Not all APIs will take this constant. See the API for details</td>
  </tr>
  <tr>
    <td>MainViewManager.ACTIVE_PANE</td>
    <td>The operation is performed only on the active pane</td>
    <td>&nbsp;</td>
  </tr>
</table>



# Workingset Alternatives
APIs extension authors can use to get the list of files in the project in various forms that aren't Working Set specific:

## MainViewManger.getAllOpenFiles()
Returns a list of all open files. This list will include all temporary views of files (those not in a working set), files opened that are not part of the project, images, files that are opened in an editor and files opened in custom viewers.  
@type `Array.<File>`

## [ProjectManager.getAllFiles()](http://brackets.io/docs/current/modules/project/ProjectManager.html#-getAllFiles)
Returns a list of all files in the project.  Supports Filtering and inclusion of files opened in the working set which are not part of the projec.
@type `Array.<File>`

## [DocumentManager.getAllOpenDocuments()](http://brackets.io/docs/current/modules/document/DocumentManager.html#-getAllOpenDocuments)
Returns a list of all open documents -- including documents which are in a workingset but not yet opened, documents which have been opened in inline editors but not part of a workingset and opened but not modified or added to any workingset.
@type `Array.<Document>`


# Custom Views

`EditorManager.registerCustomViewer()` has been deprecated and dead-ended which means calling it will no longer register a custom viewer.  

Custom Viewers must be implemented with a Factory Object that can be registered and respond to callbacks to create custom Views.  `registerCustomViewer` made assumptions about the lifespan of a view and would only allow 1 view to be created which didn't scale to the new model so it was deprecated.

Extension writers must now create a View Factory with this interface:
```
{
    canOpenFile:function(path:string):boolean
    openFile:function(path:string, pane:Pane):jQuery.Deferred() 
}
```

Then register the factory by calling `MainViewFactory.registerViewFactory()` 

View Factories will be able to crack a file URL to determine if they support creating a view for that particular file.  ImageViewer, for example, responds by testing the file type for a known image type:

```
    canOpenFile: function (fullPath) {
        var lang = LanguageManager.getLanguageForPath(fullPath);
        return (lang.getId() === "image");
    }
```

URL cracking provides a fairly robust way to provide custom views for just about every file type, path or filename.  View Factories are asked to crack URLs in a first-registered basis so there is no garaunteed order at this time.


```
{
     getFile: function () @return {?File} File object that belongs to the view (may return null)
     setVisible: function(visible:boolean) - shows or hides the view 
     updateLayout: function(forceRefresh:boolean) - tells the view to do ignore any cached layout data and do a complete layout of its cont
     destroy: function() - called when the view is no longer needed. 
     hasFocus:  function() - called to determine if the view has focus.  
     childHasFocus: function() - called to determine if a child component of the view has focus.  
     focus: function() - called to tell the view to take focus
     getScrollPos: function() - called to get the current view scroll state. @return {Object=}
     adjustScrollPos: function(state:Object=, heightDelta:number) - called to restore the scroll state and adjust the height by heightDelta
     switchContainers: function($newContainer:jQuery} - called to reparent the view to a new container
     getContainer: function() - called to get the current container @return {!jQuery} - the view's parent container
     getViewState: function() @return {?*} - Called when the pane wants the view to save its view state.  Return any data you needed to res
     restoreViewState: function(!viewState:*) - Called to restore the view state. The viewState argument is whatever data was returned from
}
```

Here is a [sample extension](https://github.com/adobe/brackets/tree/jeff/splitview-factory/src/extensions/samples/BracketsConfigCentral)
# Extensions that will Break
For the most part, we expect that most extensions will continue to work as they do today. We have, however, identified a handful of extensions which do not work wit Bracket's new architecture:
<table>
<thead>
<tr><td><b>Extension</b></td><td><b>Why it breaks</b></td><td><b>Suggested remediation</b></td><td><b>URL</b></td></tr>
</thead>
  <tr>
    <td>Code Overview</td>
    <td>Appnds nodes directly to `#editor-holder`</td>
    <td>Could listen for `EditorManager.activeEditorChange` and append nodes to the `$(Editor.getRootElement())` but would need to keep track of previous instances and remove them.</td>
    <td><a href="https://github.com/thomasvalera/Brackets-CodeOverview">CodeOverview</a></td>
  </tr>
  <tr>
    <td>MainWindow</td>
    <td>Resizes nodes inside of `#editor-holder`</td>
    <td>Just needs to resize `#editor-holder` and it should just work.</td>
    <td><a href="https://github.com/thomasvalera/Brackets-MainWindow">MainWindow</a></td>
  </tr>
  <tr>
    <td>Workspaces</td>
    <td>Resizes nodes inside of `#editor-holder`</td>
    <td>Just needs to resize `#editor-holder` and it should just work.</td>
    <td><a href="https://github.com/thomasvalera/Brackets-Workspaces">Workdspaces</a></td>
  </tr>
  <tr>
    <td>CharLimit</td>
    <td>Appnds DOM nodes directly to `#editor-holder`</td>
    <td>Could listen for `EditorManager.activeEditorChange` and append nodes to the `$(Editor.getRootElement())` but would need to keep track of previous instances and remove them.</td>
    <td><a href="https://github.com/cezarywojcik/cezarywojcik.charlimit">CharLImit</a></td>
  </tr>  
  <tr>
    <td>Epic Linter</td>
    <td>Assumes `#editor-holder` is the width of the editor</td>
    <td>Should use `$(Editor.getRootElement()).width()` instead.</td>
    <td><a href="https://github.com/fdecampredon/brackets-epic-linter">EpicLinter</a></td>
  </tr>
  <tr>
    <td>Shreder</td>
    <td>Sets the width of `#editor-holder` to zero</td>
    <td>Unable to test with Release 0.42 or Splitview</td>
    <td><a href="https://github.com/freestrings/shreder-brackets">Shreder</a></td>
  </tr>  
  <tr>
    <td>Brackets Tabs</td>
    <td>Removes the working set and replaces it with tabs</td>
    <td>The DOM node `#working-set-header` no longer exists so this extension fails. There are numerous other issues that would need to be fixed for this extension to work.</td>
    <td><a href="https://github.com/Sean-Davies">Repo not found|Sean Davies</a></td>
  </tr>  
  <tr>
    <td>Code Outline</td>
    <td>Bases size based on `#working-set-header` which no longer exists</td>
    <td>Probably can just use `$(#sidebar).width()`</td>
    <td><a href="https://github.com/crabcode/brackets-outline">Code Outline</a></td>
  </tr>
<tr>
    <td>Brackets Font Viewer</td>
    <td>Registers a Custom View for Font Files</td>
    <td>Register a View Provider Factory for font files</td>
    <td><a href="https://github.com/TomMalbran/brackets-fonts-viewer">Font Viewer</a></td>
  </tr>  
  <tr>
    <td>Brackets GIT</td>
    <td><ol>
            <li>Adds DOM nodes to `#working-set-header`</li>
            <li>Uses `EditorManager._closeCustomViewer()`</li>
        </ol>
    </td>
    <td>
        <ol>
            <li>Could append to `.pane-view-header` and use `ACTIVE_PANE` or `ALL_PANES` when closing unchanged documents.</li>
            <li>Shouldn't use a private method.  Will need to Register a View Provider Factory to create views or use a dialog.  NOTE: This extension implements a 1-off history viewer for each file when the `history` button is pressed.  This could use the `MainViewManager.showView()` 1-off viewer that was demonstrated late last week but pulled from the build. @dangoor, @pflynn should we consider bringing it back for this type of 1-off view?</li>
        </ol>
    </td>
    <td><a href="https://github.com/crabcode/brackets-outline">Code Outline</a></td>
  </tr>  
  
</table>

# Unit Tests that will Break
The following Extension's Unit Tests will no longer work. Fixing these isn't critical, however, but extension authors should maintain their reliance on running unit tests regularly to ensure their extension continues to work normally.  This list identifies those which will break.
<table>
<thead>
<tr><td><b>Extension</b></td><td><b>Why it breaks</b></td><td><b>Suggested remediation</b></td><td><b>URL</b></td></tr>
</thead>
  <tr>
    <td>QuickRequire</td>
    <td>Appends nodes directly to `#editor-holder`</td>
    <td>Could listen for `EditorManager.activeEditorChange` and append nodes to the `$(Editor.getRootElement())` but would need to keep track of previous instances and remove them.</td>
    <td><a href="https://github.com/thomasvalera/Brackets-CodeOverview">CodeOverview</a></td>
  </tr>
  <tr>
    <td>Simple JS Code Hints</td>
    <td>Uses `EditorManager.setEditorHolder()`</td>
    <td>Just use `MainViewManager._init()`.</td>
    <td><a href="https://github.com/iwehrman/brackets-simple-js-code-hints">Simple JS Code Hints</a></td>
  </tr>
  <tr>
    <td>Brackets Type Script Code Intel</td>
    <td>Uses `EditorManager.setEditorHolder()`</td>
    <td>Just use `MainViewManager._init()`.</td>
    <td><a href="https://github.com/tomsdev/brackets-typescript-code-intel">Type Script Code Intel</a></td>
  </tr>
</table>

# Courtesy Notices

* [Brackets Vim](https://github.com/megalord/brackets-vim) - The VIM splitview doesn't work and you referenced the work in progress.  Please let us know how it works for you.  We tried your extension but the command `:vsp` throws an rte in both master without SplitView and this branch. Otherwise it appeared that VIM worked normally.