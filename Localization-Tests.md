* _citrus completed_ test project: https://github.com/adobe/brackets/tree/master/test/smokes/citrus%20completed
* To setup each test, load the _citrus completed_ project via _File > Open folder..._
* Placeholders for images are written as ``<image_filename_placeholder>``

# General UI

## Menus
1. Top Level
2. File
3. Edit
4. View
5. Navigate
6. Debug (TODO exclude?)

## Toolbar
1. Experimental Build
2. Lightning bolt icon tooltip ``Live File Preview``

## About
1. On mac, ``Brackets > About Brackets``. On win, ``Help > About``
2. Confirm about dialog text ``<dialog_about>``

# File Operations

## Open

1. Select the ``File > Open`` menu
2. Confirm localized file open dialog title ``<dialog_file_open>``

## Error opening file

1. In the project tree, expand the ``images`` folder.
2. Click on ``events.jpg``
3. Confirm error message ``<dialog_error_opening_file>``

## Invalid file name

1. Select the ``File > New`` menu
2. ``untitled.js`` will appear editable
3. Rename ``untitled.js`` to ``foo:js``
4. Confirm error message ``<dialog_error_invalid_file_name>``
6. Select ``index.html``
7. Select the ``File > New`` menu
8. Rename ``untitled.js`` to ``index.html``
9. Confirm error message ``<dialog_error_file_already_exists>``

## Save

1. Open ``index.html``
2. Make any change
3. ``File > Close``
4. Confirm dialog ``<dialog_save_changes_one_file>``
5. Choose "Cancel"
6. From the project tree, open ``desktop.css``
7. Make any change
8. ``File > Quit``
9. Confirm dialog ``<dialog_save_changes_multiple_files>``
10. Choose cancel
11. Open ``desktop.css`` in a different program.
12. Dirty the file so that the last modified time updates. Does not necessarily require a change to the file content.
13. Return to Brackets
14. Confirm dialog ``<dialog_external_changes>``
15. Choose "Reload from Disk"

# Editor

## Find/Replace
1. Open ``index.html``
2. ``Edit > Find``
3. Confirm find UI appears at the top of the editor ``<dialog_find>``
4. Press escape
5. ``Edit > Replace``
6. Confirm replace UI appears at the top of the editor ``<dialog_replace_1>``
7. Type any character(s) into the text box, press Enter
8. Confirm replace UI appears at the top of the editor ``<dialog_replace_2>``

# Live Preview

1. Exit Chrome if it is running.
2. Open ``index.html``
3. ``File > Live Preview``
3. In Brackets, hover over the lightning bold icon
4. Confirm tooltip ``<tooltip_disconnect_live_file_preview>``
5. Quit Chrome
6. Start Chrome