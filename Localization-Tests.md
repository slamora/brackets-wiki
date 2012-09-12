# Notes for testing

* ``citrus completed`` test project: https://github.com/adobe/brackets/tree/master/test/smokes/citrus%20completed
* To setup each test, load the ``citrus completed`` project via ``File > Open folder...``
* Placeholders for screenshots are written as ``<image_filename_placeholder>``
* To open files from the project panel (i.e. to add them to the working set), double-click on the file.

# General UI

## Menus

**Note: Debug menu is not tested**

1. Top Level
2. File
3. Edit
4. View
5. Navigate
6. Help

## Toolbar
1. Experimental Build
2. Lightning bolt icon tooltip ``Live File Preview``

## About
1. On mac, ``Brackets > About Brackets``. On win, ``Help > About``
2. Confirm about dialog text ``<dialog_about>``

## Sidebar

1. Click ``View > Hide Sidebar`` menu
2. Open the ``View`` menu again
3. Confirm the label has changed to ``View > Show Sidebar`` ``<menu_view_show_sidebar>``
4. Click ``View > Show Sidebar``

# File Operations

## Open

1. Click ``File > Open...`` menu
2. Confirm file open dialog ``<dialog_file_open>``
3. Choose ``Cancel``
4. Click ``File > Open Folder...`` menu
5. Confirm open folder dialog ``<dialog_folder_open>``
6. Click ``File > Open Folder...`` menu 

## Quick Open

1. Click ``Navigate > Quick Open``
2. Confirm the quick open UI appears at the top of the editor ``<dialog_quick_open>``

## Error opening file

1. In the project tree, expand the ``images`` folder.
2. Click on ``events.jpg``
3. Confirm error message ``<dialog_error_opening_file>``

## Invalid file name

1. Click ``File > New`` menu
2. Confirm ``untitled.js`` editable file name <project_panel_untitled_file>
3. Rename ``untitled.js`` to ``foo:js``
4. Confirm error message ``<dialog_error_invalid_file_name>``
5. Select ``index.html``
6. Click ``File > New`` menu
7. Rename ``untitled.js`` to ``index.html``
8. Confirm error message ``<dialog_error_file_already_exists>``

## Save

1. Open ``index.html``
2. Make any change
3. Click ``File > Close`` menu
4. Confirm dialog ``<dialog_save_changes_one_file>``
5. Choose ``Cancel``
6. From the project tree, open ``desktop.css``
7. Make any change
8. Click ``File > Quit`` menu
9. Confirm dialog ``<dialog_save_changes_multiple_files>``
10. Choose ``Cancel``
11. In the operating system, delete ``desktop.css``
12. Return to Brackets
13. Confirm the external changes dialog ``<dialog_external_changes_deleted>``
14. Choose ``Close (Don't Save)``
15. In the operating system, restore ``desktop.css``

# File Errors

**Note: These tests assume that Brackets does not update the project tree when files are added and removed. Since these tests require changes to the file system, it is recommended to work with a separate copy of the ``citrus completed`` project and to close and restart Brackets for each test.**

## Directory Permissions

1. In the operating system, remove **write** permissions from the ``css`` directory. On mac, use ``chmod 444 css``. On windows, open the file properties dialog. Under "Security", click "Edit...". Choose your user account, then check the "Deny" checkbox for the "Write" permission. Save permission changes when finished.
2. In Brackets, select the ``css`` directory in the project tree.
3. Click ``File > New`` menu
4. Hit Enter to accept the default name
5. Confirm error creating file dialog ``<dialog_error_creating_file_no_modifications>``
6. In the operating system, restore the original permissions to the ``css`` directory

## Write Permissions

1. In the operating system, remove **write** permissions from ``index.html``. On mac, use ``chmod 444 index.html``. On windows, open the file properties dialog. Under "Security", click "Edit...". Choose your user account, then check the "Deny" checkbox for the "Write" permission. Save permission changes when finished.
2. In Brackets, open ``index.html``
3. Make any edit to the file
4. Click ``File > Save`` menu
5. Confirm error saving file dialog ``<dialog_error_saving_file_no_modifications>``
6. In the operating system, restore the original file permissions to ``index.html``

## Read Permissions

1. In the operating system, remove **read** permissions from ``index.html``. On mac, use ``chmod 0 index.html``. On windows, open the file properties dialog. Under "Security", click "Edit...". Choose your user account, then check the "Deny" checkbox for the "Read" permission. Save permission changes when finished.
2. In Brackets, open ``index.html``
3. Confirm error opening file dialog ``<dialog_error_opening_file_not_readable>``
4. In the operating system, restore the original file permissions to ``index.html``

## Read Permissions on Reload

1. In Brackets, open ``index.html``
2. Make any edit, do not save. A dirty dot should appear in the working set next to the file and in the toolbar next to the file name.
3. Open ``index.html`` in a different program.
4. Dirty the file so that the last modified time metadata is updated
5. Return to Brackets
6. Confirm dialog ``<dialog_external_changes_reload>``
7. In the operating system, remove **read** permissions from ``index.html``. On mac, use ``chmod 0 index.html``. On windows, open the file properties dialog. Under "Security", click "Edit...". Choose your user account, then check the "Deny" checkbox for the "Read" permission. Save permission changes when finished.
8. Return to Brackets
9. Choose ``Reload from Disk``
10. Confirm dialog ``<dialog_error_reloading_changes_from_disk>``
11. Choose ``OK`` and confirm the file in Brackets is still dirty
12. In the operating system, restore the original file permissions to index.html
13. Open ``index.html`` in a text editor that allows saving with a different encoding (e.g. Sublime Text 2)
14. In the text editor save ``index.html`` with encoding: UTF-16 BE with BOM
15. Return to Brackets
16. Choose ``Reload from Disk``
17. Confirm dialog ``<dialog_error_reloading_changes_from_disk_generic_error_code_2>``
18. Choose ``OK``
19. In the operating system, restore the original file permissions to ``index.html`` and reset the file encoding to UTF-8.

## File not found

1. In Brackets, close ``index.html`` if open
2. In the operating system, delete ``index.html``
3. In Brackets, ``index.html`` is still listed.
4. Attempt to open ``index.html``
5. Confirm error opening file dialog ``<dialog_error_opening_file_file_not_found>``
6. In the operating system, restore ``index.html``

## Directory not found

1. In Brackets, collapse all folders (only ``css``, ``images`` and ``index.html`` should be visible in the tree)
2. Quit and restart Brackets
3. In the operating system, delete the ``images`` directory
4. Attempt to open the ``images`` directory
5. Confirm error loading project dialog ``<dialog_error_loading_project_directory_contents>``
6. In the operating system, restore the ``images`` directory

## Directory not found at startup

1. With ``citrus completed`` as the current project, quit Brackets
2. In the operating system, rename the ``citrus completed`` directory to ``foo``
3. Open Brackets
4. Confirm the error loading project dialog ``<dialog_error_loading_project_request_nfs>``
5. In the operating system, restore the original directory name to ``citrus completed``

# Editor

## Find/Replace
1. Open ``index.html``
2. Click ``Edit > Find`` menu
3. Confirm find UI appears at the top of the editor ``<dialog_find>``
4. Press escape
5. Click ``Edit > Replace`` menu
6. Confirm replace UI appears at the top of the editor ``<dialog_replace_1>``
7. Type any character(s) into the text box, press Enter
8. Confirm replace UI appears at the top of the editor ``<dialog_replace_2>``

## JSLint

1. Enable ``View > Enable JSLint`` (a checkmark will appear when enabled)
2. Confirm JSLint panel title ``<panel_jslint>``

# Live Preview

**Requires Google Chrome to be installed http://www.google.com/chrome**

## Connect

1. Exit Chrome if it is running.
2. Open ``index.html``
3. Click ``File > Live Preview`` menu
3. In Brackets, hover over the lightning bold icon
4. Confirm tooltip ``<tooltip_disconnect_live_file_preview>``
5. Quit Chrome

## Connection Error
1. Start Chrome
2. Click ``File > Live Preview`` menu
3. Confirm live development error dialog ``<dialog_live_development_connection_error>``
4. Click ``Cancel``

## Wrong File Type
1. Open ``css\desktop.css``
2. Click ``File > Live Preview`` menu
3. Confirm live development error dialog ``<dialog_live_developement_file_type_error>``

## Chrome Not Found
1. Close Chrome if running
2. Rename the Chrome executable
3. Open ``index.html``
4. Click ``File > Live Preview`` menu
5. Confirm live development error dialog ``<dialog_live_development_chrome_not_found>``
6. Restore the Chrome executable name

# Update Notification

The following tests configure fake updates for testing ``Help > Check for Updates``. Update information is normally pulled from http://brackets.io.

## Manual Check for Updates
1. Click ``Debug > Show Developer Tools`` to open dev tools window
2. Click the ``Sources`` tab at the top of the dev tools window
3. If the script console is hidden, press ``esc`` to open the console
4. In the console window, enter:
``require("utils/UpdateNotification").checkForUpdate(true, {_buildNumber: 0, _lastNotifiedBuildNumber: 0})``
5. Confirm update information dialog appears ``<dialog_update_notification_up_to_date>``
6. In the console window, enter: ``require("utils/UpdateNotification").checkForUpdate(true, {_buildNumber: 0, _lastNotifiedBuildNumber: 0, _versionInfoURL: "https://raw.github.com/adobe/brackets/master/test/spec/UpdateNotification-test-files/versionInfo.json"})``
7. Confirm update information dialog appears ``<dialog_update_notification_update_available>``
8. Press Cancel
9. Hover over the update icon (a gift box) in the toolbar
10. Confirm update information tooltip appears ``<tooltip_update_notification>``