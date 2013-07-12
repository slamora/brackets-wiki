# File System Evolution #

Status: Proposal

Email comments to: [gruehle@adobe.com](mailto:gruehle@adobe.com)

## Introduction ##

The file system APIs in Brackets are a bit chaotic, and usage is inconsistent. Here are just a few issues:
* Two APIs for file i/o: `brackets.fs` and `NativeFileSystem` (HTML5 File API)
* Inconsistent use of `fullPath` vs. `FileEntry`
* Incorrect creation of `FileEntry` objects (these should never be directly instantiated)
* Inefficient. We constantly hit the disk to read timestamps, content, etc.
* No centralized file system model. This makes it difficult to add file watchers and update all file references in the app after operations like rename.
* No way to use alternate storage--Dropbox, Google Drive, etc.

I'm pretty sure there are many more...

## Overview ##

The high-level goals of the file system are:
* Clean and consistent API.
* High Performance. Cache wherever possible.
* Ability to swap out the low-level I/O routines.  

Here is a block diagram of the major parts:

TODO: Add Diagram

Clients only interact with the blue boxes. The green boxes represent low-level file i/o implementations, which can be added as extensions.


## API ##

### Basic Usage ###
TODO

### FileSystem ###
The main module is `FileSystem`. This is the public API for getting files and directories, showing open/save dialogs, and getting the list of all the files in the project.

```js
    
    /**
     * Register a file system implementation
     * @param {string} name Name of the implementation
     * @param {object} impl File system implementation. Must implement the methods 
     *          described in FileSystemImpl. 
     */
    function registerFileSystemImpl(name, impl);
    
    /**
     * Set the file system to use.
     * @param {string} system The system to use. The system must be registered with
     *        registerFileSystemImpl. The built-in systems are "appshell" and 
     *        "dropbox", but extensions can register additional implementations.
     */
    function setFileSystem(system);
    
    /**
     * Returns false for files and directories that are not commonly useful to display.
     *
     * @param {string} path File or directory to filter
     * @return boolean true if the file should be displayed
     */
    function shouldShow(path);
    
    /**
     * Return a File object for the specified path.
     *
     * @param {string} path Path of file. 
     *
     * @return {File} The File object. This file may not yet exist on disk.
     */
    function getFileForPath(path);
     
    /**
     * Return an File object that does *not* exist on disk. Any attempts to write to this
     * file will result in a Save As dialog.
     *
     * @return {File} The File object.
     */
    function newUnsavedFile();
    
    /**
     * Return a Directory object for the specified path.
     *
     * @param {string} path Path of directory. Pass NULL to get the root directory.
     *
     * @return {Directory} The Directory object. This directory may not yet exist on disk.
     */
    function getDirectoryForPath(path);
    
    /**
     * Check if the specified path exists.
     *
     * @param {string} path The path to test
     * @return {$.Promise} Promise that is resolved if the path exists, or rejected if it doesn't.
     */
    function pathExists(path);
    
    /**
     * Read the contents of a Directory. 
     *
     * @param {Directory} directory Directory whose contents you want to get
     *
     * @return {$.Promise} Promise that is resolved with the contents of the directory.
     *         Contents is an Array of File and Directory objects.
     */
    function getDirectoryContents(directory);
    
    /**
     * Return all indexed files, with optional filtering
     *
     * @param {=function (entry):boolean} filterFunc Optional filter function. If supplied,
     *         this function is called for all entries. Return true to keep this entry,
     *         or false to omit it.
     *
     * @return {Array<File>} Array containing all indexed files.
     */
    function getFileList(filterFunc);
    
    /**
     * Show an "Open" dialog and return the file(s)/directories selected by the user.
     *
     * @param {boolean} allowMultipleSelection Allows selecting more than one file at a time
     * @param {boolean} chooseDirectories Allows directories to be opened
     * @param {string} title The title of the dialog
     * @param {string} initialPath The folder opened inside the window initially. If initialPath
     *                          is not set, or it doesn't exist, the window would show the last
     *                          browsed folder depending on the OS preferences
     * @param {Array.<string>} fileTypes List of extensions that are allowed to be opened. A null value
     *                          allows any extension to be selected.
     *
     * @return {$.Promise} Promise that will be resolved with the selected file(s)/directories, 
     *                     or rejected if an error occurred.
     */
    function showOpenDialog(allowMultipleSelection,
                            chooseDirectories,
                            title,
                            initialPath,
                            fileTypes);
    
    /**
     * Show a "Save" dialog and return the path of the file to save.
     *
     * @param {string} title The title of the dialog.
     * @param {string} initialPath The folder opened inside the window initially. If initialPath
     *                          is not set, or it doesn't exist, the window would show the last
     *                          browsed folder depending on the OS preferences.
     * @param {string} proposedNewFilename Provide a new file name for the user. This could be based on
     *                          on the current file name plus an additional suffix
     *
     * @return {$.Promise} Promise that will be resolved with the name of the file to save,
     *                     or rejected if an error occurred.
     */
    function showSaveDialog(title, initialPath, proposedNewFilename);
    
    /**
     * Set the root directory for the project. This clears any existing file cache
     * and starts indexing on a new worker.
     *
     * @param {string} rootPath The new project root.
     */
    function setProjectRoot(rootPath)
```

###FileSystemEntry###
This is an abstract representation of a FileSystem entry, and the base class for the `File` and `Directory` classes. FileSystemEntry objects are never created directly by client code. Use `FileSystem.getFileForPath()`, `FileSystem.getDirectoryForPath()`, or `FileSystem.getDirectoryContents()` to create the entry.

```js
    /**
     * Returns the path for this file entry.
     * @return {string} 
     */
    FileSystemEntry.prototype.getPath = function ();
    
    /**
     * Returns the name of the file or directory
     * @return {string}
     */
    FileSystemEntry.prototype.getName = function ();
    
    /**
     * Returns true if this entry is a file.
     * @return {boolean}
     */
    FileSystemEntry.prototype.isFile = function ();
    
    /**
     * Returns true if this entry is a directory.
     * @return {boolean}
     */
    FileSystemEntry.prototype.isDirectory = function ();
    
    /**
     * Returns true if the entry exists on disk.
     *
     * @return {$.Promise} Promise that is resolved with true if the entry exists, 
     *        or false if it doesn't.
     */
    FileSystemEntry.prototype.exists = function ();
    
    /**
     * Returns the stats for the entry.
     *
     * @return {$.Promise} Promise that is resolved with the entries stats, or rejected
     *        if an error occurred.
     */
    FileSystemEntry.prototype.stat = function ();
    
    /**
     * Rename this entry.
     *
     * @param {String} newName New name for this entry.
     *
     * @return {$.Promise} Promise that is resolved with the entries stats, or rejected
     *        if an error occurred.
     */
    FileSystemEntry.prototype.rename = function (newName);
        
    /**
     * Unlink (delete) this entry.
     *
     * @return {$.Promise} Promise that is resolved if the unlink succeeded, or rejected
     *        if an error occurred.
     */
    FileSystemEntry.prototype.unlink = function ();
        
    /**
     * Move this entry to the trash. If the underlying file system doesn't support move
     * to trash, the item is permanently deleted.
     *
     * @return {$.Promise} Promise that is resolved if the moveToTrash succeeded, or rejected
     *        if an error occurred.
     */
    FileSystemEntry.prototype.moveToTrash = function ();
```

###File###
This class represents a file on disk (this could be a local disk or cloud storage). This is a subclass of `FileSystemEntry`.

```js
    
    /**
     * Read a file as text. 
     *
     * @param {string=} encoding Encoding for reading. Defaults to UTF-8.
     *
     * @return {$.Promise} Promise that is resolved with the text and stats from the file,
     *        or rejected if an error occurred.
     */
    File.prototype.readAsText = function (encoding);
    
    /**
     * Write a file.
     *
     * @param {string} data Data to write.
     * @param {string=} encoding Encoding for data. Defaults to UTF-8.
     *
     * @return {$.Promise} Promise that is resolved with the file's new stats when the 
     *        writing is complete, or rejected if an error occurred.
     */
    File.prototype.write = function (data, encoding);
```

##Directory##
This class represents a directory on disk (this could be a local disk or cloud storage). This is a subclass of `FileSystemEntry`.

```js
    
    /**
     * Create a directory
     *
     * @param {int=} mode The mode for the directory.
     *
     * @return {$.Promise} Promise that is resolved with the stat from the new directory.
     */
    Directory.prototype.create = function (mode);
```

##Performance##

TODO

##File Watchers##

TODO

