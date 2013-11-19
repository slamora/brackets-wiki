The front-end client-facing filesystem API is decoupled from the back-end filesystem implementation in order to accommodate completely different kinds of filesystems. For example, with this API, clients can uniformly access filesystems for accessing local files based on Brackets shell or Node.JS, or for accessing remove files stored on Dropbox or SkyDrive. This page describes the requirements for implementing a filesystem back-end implementation.

## `FileSystemImpl`

The client-facing filesystem API is provided by a singleton `FileSystem` object. This object has a single method, `FileSystem.init`, to initialize the filesystem with a particular filesystem implementation, which is responsible for core functionality like reading and writing files, and providing file and directory change notifications. The implementation object should satisfy a `FileSystemImpl` interface, as described below.

### `init(callback)`
* `@param {function(?string)=} callback` 
* Initialize the implementation object, and optionally call back asynchronously when the initialization is complete. The callback takes a single, nullable error parameter, as given by the properties of `FileSystemError`, described below. 

### `showOpenDialog(allowMultipleSelection, chooseDirectories, title, initialPath, fileTypes, callback)`
* `@param {boolean} allowMultipleSelection`
* `@param {boolean} chooseDirectories`
* `@param {string} title`
* `@param {string} initialPath`
* `@param {Array.<string>=} fileTypes`
* `@param {function(?string, Array.<string>=)} callback`
* Display an open-files dialog to the user and call back asynchronously with either an error string or an array of path strings, which indicate the file or files chosen by the user.

### `showSaveDialog(title, initialPath, proposedNewFilename, callback)`
* `@param {string} title`
* `@param {string} initialPath`
* `@param {string} proposedNewFilename`
* `@param {function(?string, string=)} callback`
* Display a save-file dialog to the user and call back asynchronously with either an error or the path to which the user has chosen to save the file.

### `exists(path, callback)`
* `@param {string} path`
* `@param {function(?string, boolean)} callback`
* Determine whether a file or directory exists at the given path by calling back asynchronously with either an error or a boolean, which is true if the file exists and false otherwise. The error will never be `FileSystemError.NOT_FOUND`; in that case, there will be no error and the boolean parameter will be false.

### `readdir(path, callback)`
* `@param {string} path`
* `@param {function(?string, Array.<FileSystemEntry>=, Array.<?string|FileSystemStats>=)} callback`
* Read the contents of the directory at the given path, calling back asynchronously either with an error or an array of FileSystemEntry objects along with another consistent array, each index of which either contains a FileSystemStats object for the corresponding FileSystemEntry object in the second parameter or a FileSystemErrors string describing a stat error. 

### `mkdir(path, mode, callback)`
* `@param {string} path`
* `@param {number=} mode`
* `@param {function(?string, FileSystemStats=)=} callback`
* Create a directory at the given path, and optionally call back asynchronously with either an error or a stats object for the newly created directory. The octal mode parameter is optional; if unspecified, the mode of the created directory is implementation dependent.

### `rename(oldPath, newPath, callback)`
* `@param {string} oldPath`
* `@param {string} newPath`
* `@param {function(?string)=} callback`
* Rename the file or directory at `oldPath` to `newPath`, and optionally call back asynchronously with a possibly null error.

### `stat(path, callback)`
* `@param {string} path`
* `@param {function(?string, FileSystemStats=)} callback` 
* Stat the file or directory at the given path, calling back asynchronously with either an error or the entry's associated FileSystemStats object.

### `readFile(path, options, callback)`
* `@param {string} path`
* `@param {{encoding : string=}=} options`
* `@param {function(?string, string=, FileSystemStats=)} callback`
* Read the contents of the file at the given path, calling back asynchronously with either an error or the data and, optionally, the FileSystemStats object associated with the read file. The optional `options` parameter can be used to specify an encoding (default `"utf8"`).

### `writeFile(path, data, [options], callback)`
* `@param {string} path`
* `@param {string} data`
* `@param {{encoding : string=, mode : number=}=} options`
* `@param {function(?string, FileSystemStats=)} callback`
* Write the given data to the file at the given path, calling back asynchronously with either an error or, optionally, the FileSystemStats object associated with the written file. The optional `options` parameter can be used to specify an encoding (default `"utf8"`) and an octal mode (default unspecified and implementation dependent). If no file exists at the given path, a new file will be created.

### `unlink(path, callback)`
* `@param {string} path`
* `@param {function(string)=} callback`
* Unlink the file or directory at the given path, optionally calling back asynchronously with a possibly null error.

### *optional* `moveToTrash(path, callback)`
* `@param {string} path`
* `@param {number} mode`
* `@param {function(?string)=} callback`
* Move the file or directory at the given path to the "trash" directory, optionally calling back asynchronously with a possibly null error.

### `initWatchers(changeCallback, callback)`
* `@param {function(?string, FileSystemStats=)} changeCallback`
* `@param {function(?string)=} callback`
* Initialize file watching for this filesystem, optionally calling back asynchronously with a possibly null error. The implementation must use the supplied `changeCallback` to provide change notifications. The first parameter of `changeCallback` specifies the changed path (either a file or a directory); if this parameter is null, it indicates that the implementation cannot specify a particular changed path, and so the callers should consider all paths to have changed and to update their state accordingly. The second parameter to `changeCallback`is an optional `FileSystemStats` object that may be provided in case the changed path already exists and stats are readily available.

### `watchPath(path, callback)`
* `@param {string} path`
* `@param {function(?string)=} callback`
* Start providing change notifications for the file or directory at the given path, optionally calling back asynchronously with a possibly null error when the operation is complete. Notifications are provided using the `changeCallback` function provided by the `initWatchers` method. Note that change notifications are not to be provided recursively for directories.

### `unwatchPath(path, callback)`
* `@param {string} path`
* `@param {function(?string)=} callback`
* Stop providing change notifications for the file or directory at the given path, optionally calling back asynchronously with a possibly null error when the operation is complete. 

### `unwatchAll(callback)`
* `@param {function(?string)=} callback`
* Stop providing change notifications for all previously watched files and directories, optionally calling back asynchronously with a possibly null error when the operation is complete. 

### `normalizeUNCPaths`
* `@type {boolean}`
* Indicates whether or not the FileSystem should expect [UNC paths](http://www.uwplatt.edu/oit/terms/uncpath.html), like `//myserver/drive/folder`. If set, contiguous blocks of leading slashes as in the previous example are normalized to a pair of leading slashes instead of a single leading slash.

## `FileSystemStats` and `FileSystemError`
The stats objects passed to callbacks above are instances of the [`FileSystemStats` class](https://github.com/adobe/brackets/blob/glenn/file-system/src/filesystem/FileSystemStats.js), and the possibly null error parameters are constants defined in the [`FileSystemError` class](https://github.com/adobe/brackets/blob/glenn/file-system/src/filesystem/FileSystemError.js).