# Handling Command Line Arguments

## Overview
Generally, applications are able to process arguments specified by the user upon launch.  These arguments often can be used to configure the application, retrieve information from the application or instruct it to perform various operations upon initialization.

This spec describes an initial set of arguments that Brackets supports, with a general focus on exposing existing features.  It also describes the general framework in which the command line arguments are handled.  As additional arguments are needed in the future, they should be added to this document.

###  General Description

Specifying a File Path

   A single file path can be included as a Brackets command line argument.  If the path is to a File that is supported by the application then it will be opened for editing, in the same way it would have been had the user selected it in the dialog for the File->Open command.  If the path is to a directory on the filesystem, the folder will be displayed in the side bar.  This is analogous to having selected the folder interactively via the File->Open Folder command.

### Communication with existing Brackets Process

### Scope

## Implementation Notes

### Extension Concerns