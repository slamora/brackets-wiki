This page describes how user can update key bindings in Brackets.

Key bindings for default commands or extensions can be overridden or removed.

Currently, only key bindings for commands can be updated -- not editor bindings currently handled by the CodeMirror keymap. Key bindings cannot be modified for special commands: cut, copy, paste, undo, redo, or select all.

### Quick Start
Use `Debug > Open User Key Map` to open the `keymap.json` in Brackets so it can be edited. If the file does not already exist, then it will be created.

Edits to file are applied immediately on File Save.

### Location
The `keymap.json` file is located in the the user data folder. The user data folder is located at:

* Mac: ```~/Library/Application Support/Brackets```
* Win XP: ```C:\Documents and Settings\<username>\Application Data\Brackets``` -- (aka ```%appdata%\Brackets```)
* Win Vista/7/8: ```C:\Users\<username>\AppData\Roaming\Brackets``` -- (aka ```%appdata%\Brackets```)
* Linux: ``~/.config/brackets`` -- (aka ```$XDG_CONFIG_HOME/brackets```)

### JSON Data Format

The JSON  data format is:

    {
        "overrides": {
            "<key>": "<command-id>"
            [, "<key>": "<command-id>"]
        }
    }

Where:

`<key>` is a unique key.
`<command-id>` is the command id string to assign, or "null" to remove a key binding.

Modifier keys are operating system specific, so on Mac use `Cmd` versus `Ctrl` on Windows. 

### Exceptions

#### Special Commands
Currently, the shortcuts to these commands cannot be updated or re-assigned:
* key: `Ctrl/Cmd-A`, command: `"edit.selectAll"`
* key: `Ctrl/Cmd-C`, command: `"edit.copy"`
* key: `Ctrl/Cmd-V`, command: `"edit.paste"`
* key: `Ctrl/Cmd-X`, command: `"edit.cut"`
* key: `Ctrl/Cmd-Z`, command: `"edit.undo"`
* key: `Ctrl/Cmd-Y`, command: `"edit.redo"` (also `Cmd-Shift-Z`)

#### Mac Reserved Shortcuts
These shortcuts cannot be overridden on Mac:
* `"Cmd-,"`
* `"Cmd-H"`
* `"Cmd-Alt-H"`
* `"Cmd-M"`
* `"Cmd-Q"`

