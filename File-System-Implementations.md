The front-end client-facing filesystem API is decoupled from the back-end filesystem implementation in order to accommodate completely different kinds of filesystems. For example, with this API, clients can uniformly access filesystems for accessing local files based on Brackets shell or Node.JS, or for accessing remove files stored on Dropbox or SkyDrive. This page describes the requirements for implementing a filesystem back-end implementation.

## FileSystemImpl

The client-facing filesystem API is provided by a singleton `FileSystem` object. This object has a single method, `FileSystem.init`, to initialize the filesystem with a particular filesystem implementation, which is responsible for core functionality like reading and writing files, and providing file and directory change notifications. The implementation object should satisfy a `FileSystemImpl` interface, as described below.

### `init(callback)`
* `@param {function(?string)} callback` 
* Initialize the implementation object, and callback asynchronously when the initialization is complete. The callback takes a single, nullable error parameter, as given by the properties of `FileSystemError`, described below. 

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

### *optional* `isNetworkDrive(path, callback)`
* `@param {string} path`
* `@param {function(?string, boolean=)} callback`
* Determine whether the given path resides on a high-latency network-mounted drive by calling back asynchronously either with an error or a boolean, which is true if the drive is network mounted and false otherwise.

### `exists(path, callback)`
* `@param {string} path`
* `@param {function(boolean)} callback`
* Determine whether a file or directory exists at the given path by calling back asynchronously with a boolean, which is true if the file exists and false otherwise.

### `readdir(path, callback)`
* `@param {string} path`
* `@param {function(?string, Array.<FileSystemEntry>=, Array.<FileSystemStats=>=)} callback`
* Read the contents of the directory at the given path, calling back asynchronously either with an error or an array of FileSystemEntry objects. The callback may also include an array of FileSystemStats objects, which correspond to the FileSystemEntry objects. Neither the array of stats objects, nor the individual stats themselves are guaranteed to exist.

### `mkdir(path, [mode], callback)`
### `rename(oldPath, newPath, callback)`
### `stat(path, callback)`
### `readFile(path, [options], callback)`
### `writeFile(path, data, [options], callback)`
### `chmod(path, mode, callback)`
### `unlink(path, callback)`
### *optional* `moveToTrash(path, callback)`
### `initWatchers(callback)`
### `watchPath(path, callback)`
### `unwatchPath(path, callback)`
### `unwatchAll(callback)`