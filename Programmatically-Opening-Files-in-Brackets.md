Brackets has a few different APIs for opening and managing files.  This document describes best practices for opening files and examines the differences between the APIs used for opening and managing files in Brackets.

# Best Practices
The best way to open a document in Brackets is to execute the command `CMD_ADD_TO_WORKINGSET_AND_OPEN`.  This command will open the file in the specified pane, add it to the working files list and select it in the working file list.  This is the preferred way to open a file.

Here's an example:
```javascript
CommandManager.execute(Commands.CMD_ADD_TO_WORKINGSET_AND_OPEN, {fullPath: "./view/WorkingSetView.js", paneId: "first-pane"})
```


# API Details

| API | Function | Returns | Notes | 
| --- | -------- | ------- | ----- |
| CommandManager.execute(Commands.FILE_ADD_TO_WORKING_SET) | Opens a Document and adds it to the workingset | Promise(Document) | Deprecated. Use CommandManager.execute(CMD_ADD_TO_WORKINGSET_AND_OPEN) |
| CommandManager.execute(Commands.CMD_ADD_TO_WORKINGSET_AND_OPEN) | Opens a File and adds it to the workingset | Promise(File) | |
| CommandManager.execute(Commands.FILE_OPEN) | Opens a Document (not added to Working Files List) | Prompise(Document) | Deprecated. Use CommandManager.execute(CMD_OPEN) |
| CommandManager.execute(Commands.CMD_OPEN) | Opens a File (not added to Working Files List) | Promise(File) |  |
| MainViewManager.addToWorkingSet | Adds a file to the working files list | number | For Internal Use only |
| MainViewManager.addListToWorkingSet | Adds a list of files to the working files list | number | For Internal Use only |
| DocumentManager.setCurrentDocument | Sets the currently view document to the document specified  | void | Deprecated use CommandManager.execute(Commands.CMD_ADD_TO_WORKINGSET_AND_OPEN) |
| FileViewController.openAndSelectDocument | opens the document and selects it | Promise(*) | For Internal Use only. May resolve to a Document, File or void depending on the current state |





