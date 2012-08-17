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

# Editor