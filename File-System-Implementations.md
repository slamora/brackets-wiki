The front-end client-facing filesystem API is decoupled from the back-end filesystem implementation in order to accommodate completely different kinds of filesystems. For example, with this API, clients can uniformly access filesystems for accessing local files based on Brackets shell or Node.JS, or for accessing remove files stored on Dropbox or SkyDrive. This page describes the requirements for implementing a filesystem back-end implementation.

## FileSystemImpl

The client-facing filesystem API is provided by a singleton `FileSystem` object. This object has a single method, `FileSystem.init`, to initialize the filesystem with a particular filesystem implementation, which is responsible for core functionality like reading and writing files, and providing file and directory change notifications. The implementation object should satisfy a `FileSystemImpl` interface, as described below.

### `init(callback)`
* `@param {function(?string)}` 
* Initialize the implementation object, and callback asynchronously when the initialization is complete. The callback takes a single, nullable error parameter, as given by the properties of `FileSystemError`, described below. 

### showOpenDialog(allowMultipleSelection, chooseDirectories, title, initialPath, fileTypes, function (err, data))
### `showSaveDialog(title, initialPath, proposedNewFilename, callback)`
### *optional* `isNetworkDrive(path, callback)`
### `exists(path, callback)`
### `readdir(path, callback)`
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