### Overview
Mac does not provide a set of simple APIs to handle dropping files from Finder. So we need to find a solution either by updating CEF and/or implement our own handling of drag-and-drop events. 
We need to handle at least two main events.
 - OnDragEnter event -- need this event to get the file paths of all the dragged files and folders.
 - OnDrop -- need this event to open all the dropped files.

### Findings
We need Draghandler introduced in CEF version 3.1453.1279 so that we can get the file paths of the dragged files and folders.
We 