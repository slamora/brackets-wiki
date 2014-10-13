Brackets has a few different APIs for opening and managing files.  This document describes best practices for opening files and examines the differences between the APIs used for opening and managing files in Brackets.

# Best Practices
The best way to open a document in Brackets is to execute the command `CMD_ADD_TO_WORKINGSET_AND_OPEN`.  This command will open the file in the specified pane, add it to the working files list and select it in the working file list.  This is the preferred way to open a file.

Here's an example:
```javascript
CommandManager.execute(Commands.CMD_ADD_TO_WORKINGSET_AND_OPEN, {fullPath: "./view/WorkingSetView.js", paneId: "first-pane"})
```


# API Details

| COMMAND | Function | Returns | Notes | 
| ------- | -------- | ------- | ----- |
| FILE_ADD_TO_WORKING_SET) | Opens a Document and adds it to the workingset | Promise(Document) | Deprecated. Use CMD_ADD_TO_WORKINGSET_AND_OPEN |
| CMD_ADD_TO_WORKINGSET_AND_OPEN | Opens a File and adds it to the workingset | Promise(File) | |
| FILE_OPEN | Opens a Document (not added to Working Files List) | Prompise(Document) | Deprecated. Use CMD_OPEN |
| CMD_OPEN) | Opens a File (not added to Working Files List) | Promise(File) |  |


| API     | Function | Returns | Notes | 
| ------- | -------- | ------- | ----- |

| MainViewManager .addToWorkingSet | Adds a file to the working files list | number | For Internal Use only |
| MainViewManager .addListToWorkingSet | Adds a list of files to the working files list | number | For Internal Use only |
| DocumentManager .setCurrentDocument | Sets the currently view document to the document specified  | void | Deprecated use CMD_ADD_TO_WORKINGSET_AND_OPEN |
| FileViewController .openAndSelectDocument | opens the specified file and selects it | Promise(*) | For Internal Use only. May resolve to a Document, File or void depending on the current state |
| FileViewController .openFileAndAddToWorkingSet | opens the specified document and selects it | Promise(*) | Deprecated. For Internal Use only. May resolve to a Document or void. Callers should migrate to MD_ADD_TO_WORKINGSET_AND_OPEN)



