Work in Progress, Sprint 34

### Editors to investigate

* Sublime
* TextMate
* WebStorm
* Coda
* Notepad++
* Dreamweaver

## Find in Current File

## Find/Replace in Multiple Files

### Scope

How the user selects the scope of files to search.

* Sublime - In Selected Folder, In Project, In explicit white-list/black-list, Open Folder Dialog
* TextMate - Project Folder, Other Folder
* WebStorm
* Coda
* Notepad++
* Dreamweaver

### Performance

* Sublime - Async results display. Cancelable.
* TextMate - Async results display. Cancelable.
* WebStorm
* Coda
* Notepad++
* Dreamweaver

### Results

How the results are displayed and accessed

* Sublime - New "editor" opened with results grouped by file. Double click a match to open an editor.
* TextMate - Find dialog groups results by file. Select a match to display the file in the editor. Double click a match to dismiss the dialog and edit the file. Gutter displays a search icon for lines with a match.
* WebStorm
* Coda
* Notepad++
* Dreamweaver

### Search History

* Sublime - Find Results "editor" remains open unless closed by user. Subsequent results are appended to this editor. Query history is traversed with Arrow Up/Down.
* TextMate - Most recent query results are displayed when dialog is re-opened. Query history is traversed with Arrow Up/Down or dropdown menu. 
* WebStorm
* Coda
* Notepad++
* Dreamweaver

### Replace

How multi-file replace is handled.

* Sublime - Find query is executed just-in-time. Confirmation dialog "Replace N occurrences across M files?". Files are opened in editors without saving changes. User must manually Save or Save All.
* TextMate - Find query is executed just-in-time. Changes are pending until find dialog is closed. Save confirmation dialog upon closing.
* WebStorm
* Coda
* Notepad++
* Dreamweaver

### Undo 
* Sublime - No additional undo feature. Each dirty editor is treated normally.
* TextMate - Once dialog is confirmed, there is no undo.
* WebStorm
* Coda
* Notepad++
* Dreamweaver

### Features to investigate

* Find within file
    * Types of queries supported
    * How results are presented (highlighting, is it incremental, etc.)
    * Disposition of results while editing
* Replace within file
* Find across multiple files
    * Search scope filtering
    * Handling large numbers of results
* Replace across multiple files
    * Undo replace across multiple files
* Extensibility
* Keyboard shortcuts