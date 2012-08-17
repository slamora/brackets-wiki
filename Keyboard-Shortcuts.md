Note: Mac shortcuts use CMD instead of CTRL and OPTION instead of ALT unless otherwise mentioned.

# Menus

*  File
  * New - Ctrl-N
  * Open - Ctrl-O
  * Open Folder - (none)
  * Close - Ctrl-W
  * Save - Ctrl-S
  * Save All - Ctrl-Alt-S
  * Live File Preview - Ctrl-Alt-P
  * Quit - Ctrl-Q
* Edit
  * Select All - Ctrl+A
  * Find - Ctrl+F
  * Find in Files - Ctrl+Shift+F
  * Find Next - F3 (win), Cmd-G (mac)
  * Find Previous - Shift-F3 (win), Cmd+Shift+G (mac)
  * Replace - Ctrl-H (win), Cmd-Alt-F (mac)
  * Indent - Tab
  * Unindent - Shift-Tab
  * Duplicate - Ctrl+D
  * Move Line(s) Up - Ctrl-Shift-Up (win), Cmd-Ctrl-Up (mac)
  * Move Line(s) Down - Ctrl-Shift-Down (win), Cmd-Ctrl-Down (mac)
  * Comment/Uncomment Lines - Ctrl-/
* View
  * View Sidebar / Hide Sidebar - Ctrl-Shift-H
  * Increase Font Size - Ctrl-+
  * Decrease Font Size - Ctrl--
  * Restore Font Size - Ctrl-0
* Navigate
  * Quick Open - Ctrl-Shift-O
  * Go to Line - Ctrl-G (win), Cmd-L (mac)
  * Go to Definition - Ctrl-T
  * Quick Edit - Ctrl-E
  * Previous Match - Alt-Up
  * Next Match - Alt-Down
* Debug
  * Show Developer Tools - F12 (win), Cmd-Opt-I (mac)
  * Refresh Brackets - F5 (win), Cmd-R (mac)

# Editor (via CodeMirror)

Some of these CodeMirror commands are not documented. Many of these commands aren't explicitly tested by the Brackets team yet.

* killLine (none, emacs mode only)
* deleteLine (overridden by Duplicate Line)
* undo - Ctrl-Z (not in menus due to native text input conflict)
* redo - Ctrl-Y or Shift-Ctrl-Z
* goDocStart - Ctrl-Home or Alt-Up (win), Cmd-Up (mac)
* goDocEnd - Ctrl-End or Ctrl-down (win), Cmd-Down (mac)
* goLineStart - Alt-Left (win), Cmd-Left (mac)
* goLineStartSmart - Home 
* goLineEnd - End (mac/win), Alt-Right (win), Cmd-Right (mac)
* goLineUp - Up
* goLineDown - Down
* goPageUp - Page Up
* goPageDown - Page Down
* goCharLeft - Left
* goCharRight - Right
* goColumnLeft (none) 
* goColumnRight (none)
* goWordLeft - Ctrl-Left (win), Option-Left (mac)
* goWordRight - Ctrl-Right (win), Option-Right (mac)
* delCharLeft - Delete
* delCharRight - Backspace
* delWordLeft - Ctrl-Backspace (win), Option-Backspace (mac)
* delWordRight - Ctrl-Delete (win), Option-Delete (mac)
* indentAuto - (overridden by Brackets)
* indentMore - Tab
* indentLess - Shift-Tab
* insertTab - (none)
* defaultTab - (none)
* transposeChars (none, emacs mode only)
* newlineAndIndent - Enter
* toggleOverwrite - Insert

```
  keyMap.basic = {
    "Left": "goCharLeft", "Right": "goCharRight", "Up": "goLineUp", "Down": "goLineDown",
    "End": "goLineEnd", "Home": "goLineStartSmart", "PageUp": "goPageUp", "PageDown": "goPageDown",
    "Delete": "delCharRight", "Backspace": "delCharLeft", "Tab": "defaultTab", "Shift-Tab": "indentAuto",
    "Enter": "newlineAndIndent", "Insert": "toggleOverwrite"
  };
  // Note that the save and find-related commands aren't defined by
  // default. Unknown commands are simply ignored.
  keyMap.pcDefault = {
    "Ctrl-A": "selectAll", "Ctrl-D": "deleteLine", "Ctrl-Z": "undo", "Shift-Ctrl-Z": "redo", "Ctrl-Y": "redo",
    "Ctrl-Home": "goDocStart", "Alt-Up": "goDocStart", "Ctrl-End": "goDocEnd", "Ctrl-Down": "goDocEnd",
    "Ctrl-Left": "goWordLeft", "Ctrl-Right": "goWordRight", "Alt-Left": "goLineStart", "Alt-Right": "goLineEnd",
    "Ctrl-Backspace": "delWordLeft", "Ctrl-Delete": "delWordRight", "Ctrl-S": "save", "Ctrl-F": "find",
    "Ctrl-G": "findNext", "Shift-Ctrl-G": "findPrev", "Shift-Ctrl-F": "replace", "Shift-Ctrl-R": "replaceAll",
    "Ctrl-[": "indentLess", "Ctrl-]": "indentMore",
    fallthrough: "basic"
  };
  keyMap.macDefault = {
    "Cmd-A": "selectAll", "Cmd-D": "deleteLine", "Cmd-Z": "undo", "Shift-Cmd-Z": "redo", "Cmd-Y": "redo",
    "Cmd-Up": "goDocStart", "Cmd-End": "goDocEnd", "Cmd-Down": "goDocEnd", "Alt-Left": "goWordLeft",
    "Alt-Right": "goWordRight", "Cmd-Left": "goLineStart", "Cmd-Right": "goLineEnd", "Alt-Backspace": "delWordLeft",
    "Ctrl-Alt-Backspace": "delWordRight", "Alt-Delete": "delWordRight", "Cmd-S": "save", "Cmd-F": "find",
    "Cmd-G": "findNext", "Shift-Cmd-G": "findPrev", "Cmd-Alt-F": "replace", "Shift-Cmd-Alt-F": "replaceAll",
    "Cmd-[": "indentLess", "Cmd-]": "indentMore",
    fallthrough: ["basic", "emacsy"]
  };
```