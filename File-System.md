# File System Evolution #

Status: Proposal

## Introduction ##

The file system APIs in Brackets are a bit chaotic, and usage is inconsistent. Here are just a few issues:

* No centralized file system model. This makes it difficult to add file watchers, and update all file references in the app after operations like rename.
* Inefficient. We constantly hit the disk to read timestamps, content, etc.
* Two APIs for file i/o: `brackets.fs` and `NativeFileSystem`
* Incorrect creation of `FileEntry` objects (these should never be directly instantiated)
* No way to use alternate storage--Dropbox, Google Drive, etc.

I'm pretty sure there are many more...

## Overview ##

The high-level goals of the new file system are:

* Clean, consistent API.
* Unified file system model, including file watchers.
* High Performance. Only hit the disk when we need to.
* Ability to swap out low-level I/O routines (for cloud storage, etc.).

Here is a block diagram of the major parts:

![File System Block Diagram](screenshots/filesystem/filesystem-block-diagram.png)

Clients only interact with the blue boxes. The green boxes represent low-level file i/o implementations, which can be added as extensions. The red box replaces the old `FileIndexManager` functionality, and does not have a public API (indexed files are accessed through `FileSystem`).


### Basic Usage ###
There are a few basic rules for using the new file system.

* Persist full pathnames, but convert into `File` and `Directory` objects for in-memory use. Creating `File` and `Directory` objects is cheap so there is no need to worry about performance.
* There is a forced 1:1 relationship between `File` and `Directory` objects and their representation on disk. If two pieces of code ask the `FileSystem` for the same file path, they will both be returned the same `File` object.
* When caching file (or directory) related data, you should use the file.id property as the key, and _not_ the file.fullPath. The path of a file is subject to change if any of its parent directories are renamed.
* Listen for `"change"` events on `FileSystem` to be notified of file or directory changes. 

### Prototype ###
A prototype implementation can be found in the `glenn/file-system` [branch](https://github.com/adobe/brackets/tree/glenn/file-system). 

Most functionality works. Existing unit tests pass, but there are no unit tests for the new code (yes, shame on me...). Many extensions are broken. 

##API##

For a guide to converting from current Brackets file APIs to these new APIs, see [[File System API Migration]].

### FileSystem ###
The main module is `FileSystem`. This is the public API for getting files and directories, showing open/save dialogs, and getting notified about file system changes.

[/src/filesystem/FileSystem.js](https://github.com/adobe/brackets/blob/glenn/file-system/src/filesystem/FileSystem.js)

###FileSystemEntry###
This is an abstract representation of a FileSystem entry, and the base class for the `File` and `Directory` classes. FileSystemEntry objects are never created directly by client code. Use `FileSystem.getFileForPath()`, `FileSystem.getDirectoryForPath()`, or `Directory.getContents()` to create the entry.

[/src/filesystem/FileSystemEntry.js](https://github.com/adobe/brackets/blob/glenn/file-system/src/filesystem/FileSystemEntry.js)


###File###
This class represents a file on disk (this could be a local disk or cloud storage). This is a subclass of `FileSystemEntry`.

[/src/filesystem/File.js](https://github.com/adobe/brackets/blob/glenn/file-system/src/filesystem/File.js)


###Directory###
This class represents a directory on disk (this could be a local disk or cloud storage). This is a subclass of `FileSystemEntry`.

[/src/filesystem/Directory.js](https://github.com/adobe/brackets/blob/glenn/file-system/src/filesystem/Directory.js)

##Performance##
The main performance gains come from caching. The stats and contents of files and directories are cached in memory. This has a huge impact on i/o-intensive operations like "find in files", and generally speeds up many other parts of the system.

##File Watchers##
`FileSystem` dispatches a `"change"` event whenever a file or directory changes on disk. This event is passed an `entry` parameter, which can be a `File`, `Directory`, or `null`. If `entry` is `null`, a "wholesale" change has occurred and the code should assume everything has changed.

For now, the prototype refreshes the entire file tree whenever a directory changes. This is overkill, but was easy to implement. Ideally, only the directory that changed will be updated.

##Next Steps##
Several core developers are working on getting this implemented. We will share a more detailed API proposal with the community soon, but in the meantime feel free to take a look at the [unstable file-system dev branch](https://github.com/adobe/brackets/tree/glenn/file-system).
