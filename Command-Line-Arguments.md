#Brackets-App Command Line Argument Processing

## Introduction
Starting with the 1.3 release, Mac and Windows users can open Brackets from the command line. Mac users can install the command line tools by going to the _File_ menu and selecting _Install Command Line Shortcut_. Windows users can install the command line tools by leaving the _Add "brackets" launcher to PATH for command line use_ checkbox checked when they install Brackets 1.3.

## How to Use the Command Line Tool

When the command line tool has been set up correctly, the `brackets` command will work from the Terminal (Mac) and Command Prompt (Windows) in any folder. Following the `brackets` command with a file or folder path will open that file or folder in Brackets. If Brackets is open, it will open it in the current window. If Brackets is not open it will open Brackets and display the file/folder.

Some example usage:

Open the current folder in Brackets: `brackets .`

Open `index.html` in the current folder in Brackets: `brackets index.html`

Open up a specific folder: `brackets ~/Sites/mysite`

## How to Remove/Undo the Command Line Tools

## Troubleshooting

## Linux

## Future Ideas
### Arguments on Launch

   Specifying a path to a file that can be edited by Brackets should open that file for editing immediately after launch.  This path can be a fully qualified filesystem path or a relative path.  The behavior is analogous to executing the File->Open... menu item and selecting a file for edit.

   Specifying a path to a folder on the filesystem will open the folder as a node in the sidebar.  This folder will be added to the sidebar immediately after launch.  This path can be a fully qualified filesystem path or a relative path.  The behavior is analogous to executing the File->Open Folder... menu item and selecting a folder from the resulting dialog.
### Troubleshooting
### Communicating with a running Application

### File Association

### Interaction with Extensions

## Implementation Notes