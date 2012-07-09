#brackets-app Command line argument processing

## Overview
Currently, all interaction within the Brackets application must be performed interactively by the user.  This includes the act of opening files and folder trees for editing.  While this might be acceptable for most cases, it is desirable for the brackets shell application to be configurable at launch time via arguments specified at launch.

## Scope
### Arguments on Launch

   Specifying a path to a file that can be edited by Brackets should open that file for editing immediately after launch.  This path can be a fully qualified filesystem path or a relative path.  The behavior is analogous to executing the File->Open... menu item and selecting a file for edit.

   Specifying a path to a folder on the filesystem will open the folder as a node in the sidebar.  This folder will be added to the sidebar immediately after launch.  This path can be a fully qualified filesystem path or a relative path.  The behavior is analogous to executing the File->Open Folder... menu item and selecting a folder from the resulting dialog.

### Communicating with a running Application

### File Association

### Interaction with Extensions

## Implementation Notes

### Boost program.options?


