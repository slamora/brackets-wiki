Work in Progress, Sprint 34

### Editors to investigate

* Sublime Text 2
* TextMate 2
* WebStorm
* Coda 2
* Notepad++
* Dreamweaver
* Visual Studio

## Find in Current File

* Dreamweaver
    * All types of find and replace (in-file and multi-file) are integrated into a single dialog, controlled by a "Find in:" dropdown at the top that sets the scope of the search. (See the Find in Files section for more details on that case.) For single-file searches, the options available are "Current File" and "Selected Text".
    * The dialog always has both Find and Replace fields. Dialog actions include "Find Next" (go to next match), "Replace" (replace the currently selected text if it matches, then do a Find Next), "Find All" (open a panel listing all matches in the current scope), and "Replace All" (replace all matches in the current scope and open a panel listing the changes).
    * Options are "Match case", "Match whole word", "Ignore whitespace", and "Use regular expression".
    * In addition to simple textual searches in code, you can do “HTML-aware” searches that parse the HTML and let you do complex queries:
        * Search in “HTML text” (handles entity translation, ignores markup)
        * Structural searches: you can search for instances of a specific tag with or without particular attributes/values, or containing/not containing other tags or specific text, and specify multiple criteria at once.
        * When searching for tags, you can choose particular actions to do on those tags, like setting/clearing attributes, adding new tags inside or around, stripping tags, etc.
    * You can save and load queries.
    * I’m pretty sure you used to be able to choose particular items in the result list of a Find All and then replace them individually, but it looks like that functionality no longer exists. I’m guessing this is because the logic for keeping the Find list up to date as you did other replacements or made edits to the same documents was fragile.

## Find/Replace in Multiple Files

### Scope

How the user selects the scope of files to search.

* Sublime - In Selected Folder, In Project, In explicit white-list/black-list, Open Folder Dialog
* TextMate - Project Folder, Other Folder
* WebStorm - Project Folder, Open Folder Dialog w/ sub-folder option, Custom (Project and Libraries, Project Production Files, Project Test Files, Open Files, Module '<name>', Files in Previous Search Result, Selected Files), Source Control (Changed Files, Default). Optional file masks, e.g. *.mxml. Scope can be narrowed further to string literals and comments.
* Coda - Open Folder Dialog, File Tree (left), Sidebar (right, also file tree?), Open Files
* Notepad++ - Single absolute path (or Open Folder Dialog) w/ sub-folder option. Combo box input with path history. Optional file mask, also with history.
* Dreamweaver - Integrated with the ordinary Find dialog. Dropdown at the top lets you choose Selected Text, Current Document, Open Documents, Folder, Selected Files (in the Files panel), Entire Site (project).

### Performance

* Sublime - Async results display. Total file count, but no progress. Cancelable.
* TextMate - Async results display. Progress is reported only as the current file being read. Total files searched shown when finished. Cancelable.
* WebStorm - Foreground search with option to move to background. Async results display. Progress bar. Cancelable. "Too many usages" warning when results greater than 1001.
* Coda - Async results display. Progress is reported only as the current file being read. Cancelable.
* Notepad++ - Synchronous results display. No progress. Cancelable.
* Dreamweaver - Async results display with progress bar showing files being scanned. Cancelable.

### Results

How the results are displayed and accessed

* Sublime - New "editor" opened with results grouped by file. Double click a match to open an editor.
* TextMate - Find dialog groups results by file. Select a match to display the file in the editor. Double click a match to dismiss the dialog and edit the file. Gutter displays a search icon for lines with a match.
* WebStorm - Find Occurrences panel groups results by folder, then by file. Double click a match to open an editor. Total number of occurrences displayed at each group level. Toolbar buttons for expand/collapse all results. Toolbar buttons and keyboard shortcuts for next/previous result.
* Coda - Sidebar groups results by file with a per-file result count. Select a match to display the file in the editor.
* Notepad++ - Find result panel groups result by file. Double click a match to open an editor.
* Dreamweaver
    * If you're doing a multi-file search, clicking Find Next repeatedly will find instances in the current file, and then when you get to the end of the file, clicking Find Next will open the next file in the scope that has a match and go to the first match.
    * You can also click a separate Find All button to have it search the entire scope and open a results panel at the bottom, similar to what's in Brackets. You can click a button to reopen the Find dialog with the same query, or export the results to a text file. (I don't think those are important for us.)
    * All the same query types are available as in single-file Find.

### Search History

* Sublime - Find Results "editor" remains open unless closed by user. Subsequent results are appended to this editor. Query history is traversed with Arrow Up/Down.
* TextMate - Most recent query results are displayed when dialog is re-opened. Query history is traversed with Arrow Up/Down or dropdown menu. 
* WebStorm - Find in Path dialog combo box lists search history, has keyboard navigation.
* Coda - History appears in the Sidebar search dropdown menu. No keyboard navigation in input field.
* Notepad++ - Find results panel groups queries in a tree view. Newer queries are appended to the bottom of the tree.
* Dreamweaver - No query history, but you can save/load queries.

### Replace

How multi-file replace is handled.

* Sublime - Find query is executed just-in-time. Confirmation dialog "Replace N occurrences across M files?". Files are opened in editors without saving changes. User must manually Save or Save All.
* TextMate - Find query is executed just-in-time. Changes are pending until find dialog is closed. Save confirmation dialog upon closing.
* WebStorm - Replace in Path dialog nearly identical to Find in Path dialog. Find query is executed just-in-time. After find completes, popup appears with options for: Cancel, Replace, Skip, All in this File, All Files. 
* Coda - Prior query required, *not* just-in-time. If files are open the editors are dirtied. If not, changes are saved and not undoable.
* Notepad++ - Find query is executed just-in-time. Synchronous replace. If files are open the editors are dirtied. If not, changes are saved and not undoable.
* Dreamweaver
    * Same as multi-file search; the replace field is always visible in the dialog, so if you enter some replacement text and hit Replace it will replace the current instance (if you're already at one) and then find the next occurrence within the scope you've set (so if you're at the end of one file, it will find the next file and open it); or you can hit Replace All to replace all instances within the selected scope.
    * All the same replacement types and options are available as in single-file Replace.

### Undo 
* Sublime - No additional undo feature. Each dirty editor is treated normally.
* TextMate - Once dialog is confirmed, there is no undo.
* WebStorm - Can only undo files that were opened before Replace All operation.
* Coda - Can only undo files that were opened before Replace All operation.
* Notepad++ - Can only undo files that were opened before Replace All operation.
* Dreamweaver - For documents that are in the scope but not currently open, if you do Replace, it will navigate to and open the document, so when you do the replacement, it just does it in memory and doesn't save it to disk automatically. If you do Replace All, the replacement is done in memory on files that are open, but is done directly in files on disk that aren't open (so those replacements can't be undone; you get a warning to that effect).

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

## Existing Stories

These stories aggregate multiple smaller stories:

* Find in Current File https://trello.com/c/MBroELwS
* Find in Files https://trello.com/c/AFF2Lnll

## Proposed Stories

1. Find/Replace in Current File UI
    * Search query history - https://trello.com/c/IzACddEv/344-2-recent-search-history
    * Enter key does not close modal - https://github.com/adobe/brackets/issues/2299 and https://trello.com/c/MBroELwS
    * Search in selection - https://trello.com/c/PeF376TR/307-search-replace-within-selected-text-3
    * Incremental find (i.e. highlight results as user types) for replace queries
        * Unify Find modal and Replace modal? https://trello.com/c/7TjNR4lD/361-3-incremental-search and https://trello.com/c/MbM48GiP/262-replace-in-single-file
    * Keyboard accessibility - https://trello.com/c/7HVPLeer/975-replace-dialog-keyboard-support
1. Find/Replace and Find in Files Query Options
    * Case-sensitive, regexp, whole word - https://trello.com/c/MBroELwS/260-3-find-in-current-file
1. Find/Replace in Current File - Clickable tick marks in scrollbar - https://trello.com/c/sdJg5Dal/355-119-indicate-find-matches-via-tick-marks-in-near-scroll-bar-gutter-5
1. Find in Files - Search Scope UI
    * Find all results in single/current file https://trello.com/c/FIQZdqrH/308-1-71-single-file-find-all
    * White-list/Black-list options like Sublime Text?
    * Global (e.g. Find, Quick Open, Code Hints) file include/exclude? https://trello.com/c/LgdleOVx/695-exclude-ignore-binary-files-libraries-or-subfolders-from-find-all-other-searches
        * Ignore results found in globally filtered files (e.g. files under .git do not appear in project tree, but can be found with Find in Files) https://trello.com/c/AFF2Lnll/266-8-find-in-project-aka-find-in-files from https://github.com/adobe/brackets/issues/917
1. Find in Files - Background search, async results
1. Find in Files - Cancel search
1. Find in Files - Results UI
    * Display the first result - https://trello.com/c/AFF2Lnll/266-8-find-in-project-aka-find-in-files from https://github.com/adobe/brackets/issues/995
    * Results history - https://trello.com/c/KY4eJL7V
1. Find in Files - Maintain result integrity after edits
    * https://trello.com/c/AFF2Lnll/266-8-find-in-project-aka-find-in-files from https://github.com/adobe/brackets/issues/780
1. Replace in Files - No Undo
    * https://trello.com/c/NbNEOs4S/264-replace-across-multiple-files-13
1. Replace in Files - With Undo

### Wishlist

From [@larz0](http://github.com/larz0)

![test](https://trello-attachments.s3.amazonaws.com/4f90a6d98f77505d7940ce88/52163e8fd04a56d028002d89/d1303cc6d9a72ef37684d064d7f4506d/WishList_Find_004.png)