This page describes how user can override key bindings in Brackets.

## Quick Start
Use `Debug > Open User Key Map` to open the `keymap.json` in Brackets so it can be edited. If the file does not already exist, then it will be created.

Edits to file are applied immediately on File Save.

## Location
The `keymap.json` file is located in the the user data folder. The user data folder is located at:

* Mac: ```~/Library/Application Support/Brackets```
* Win XP: ```C:\Documents and Settings\<username>\Application Data\Brackets``` -- (aka ```%appdata%\Brackets```)
* Win Vista/7/8: ```C:\Users\<username>\AppData\Roaming\Brackets``` -- (aka ```%appdata%\Brackets```)
* Linux: ``~/.config/brackets`` -- (aka ```$XDG_CONFIG_HOME/brackets```)

## JSON Data Format

The JSON  data format is:

    {
        "overrides": {
            "<key>": "<command-id>"
            [, "<key>": "<command-id>"]
        }
    }

Where `<key>` is ...

## Exceptions

### Special Commands
Currently, the shortcuts to these commands cannot be updated or re-assigned:
* Commands.EDIT_UNDO 
* Commands.EDIT_REDO 
* Commands.EDIT_SELECT_ALL
* Commands.EDIT_CUT 
* Commands.EDIT_COPY 
* Commands.EDIT_PASTE


