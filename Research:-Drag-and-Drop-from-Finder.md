### Overview
Mac does not provide a set of simple APIs to handle dropping files from Finder. So we need to find a solution either by updating CEF and/or implementing our own handling of drag-and-drop events. 
We need to handle at least two main events.
 - OnDragEnter event -- need this event to get the file paths of all the dragged files and folders.
 - OnDrop -- need this event to open all the dropped files.

### Findings
- We need CefDragHandler introduced in CEF version 3.1453.1279 so that we can get the file paths of the dragged files and folders.
- We can use onDrop event in JavaScript to trigger the opening of the dropped files. 
- Although we already have native shell implementation of drag-and-drop from Windows Explorer, it is also preventing us from enabling drag-and-drop in CodeMirror (ie. dragging text within the document). By switching to onDrop event in JavaScript we can also enable drag-and-drop of text inside a document.