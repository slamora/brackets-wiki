## Introduction

Brackets extensions currently reside in a directory that is next to the main brackets `index.html` file. This is problematic for several reasons:

* On the mac, the extensions are inside the `Brackets.app` package. On Windows, the extensions are in the `Program Files` directory (assuming you installed in the default location). Adding or removing extensions modifies these directories, which requires elevated user permissions.
* If the app is signed, adding or removing extensions invalidates the signature.
* Extensions are not retained when upgrading to a newer build of Brackets (this is partially due to the location of the extension, and partially due to the fact that we don't have an in-place update mechanism).

This is a proposal for moving the extensions directory to the users home directory. Here is the [backlog entry] (https://trello.com/c/xI1B3nok).

## Extension Directory

At launch, Brackets will look for extensions in the following locations:

### Mac

* `/Users/<user>/Library/Application Support/Brackets/extensions/user` - This is where the user will install extensions

### Win

* `<user folder>\AppData\Roaming\Brackets\extensions\user` (in a standard windows installation, this maps to `C:\Users\<user>\AppData\Roaming\Brackets\extensions\user`) - This is where the user will install extensions.

### Both

* `<brackets src>/extensions/default` - Extensions that are packaged with Brackets 
* `<brackets src>/extensions/dev` - For convenience when developing extensions (see below for details)

## Disabled Extensions

Next to the `extensions/user` directory is an `extensions/disabled` directory. By default, both of these directories are empty. Extensions from the `user` directory will be loaded when Brackets is launched. Extensions in the `disabled` directory are _not_ loaded. Having these two directories side-by-side enables a tool like [Extension Manager] (https://github.com/jdiehl/brackets-extension-manager) to easily enable/disable extensions.

Extensions checked in to the `src/extensions/disabled` directory will _not_ be copied to the user's `extensions/disabled` directory. We should consider eliminating this directory. 

## "Show Extensions Folder"

The _Help > Show Extensions Folder_ menu item opens up the `extensions` folder in the user directory. 

## Extension Development

Many (most?) extension developers work with a local copy of the Brackets source code. For convenience, Brackets will load extensions from a `src/extensions/dev` directory inside the Brackets repo. This way developers don't need to dig through the user directories to put their extension code (or a symlink).

## API Changes

Two new functions are added to brackets-shell:

```javascript
 
    /**
     * Returns the full path of the application support directory.
     * On the Mac, it's /Users/<user>/Library/Application Support/GROUP_NAME/APP_NAME
     * On Windows, it's C:\Users\\<user>\AppData\Roaming\GROUP_NAME\APP_NAME
     *
     * If the directory does not exist, it will be created.
     *
     * @param {function (err)} callback Asynchronous callback function with one argument (the error)
     * @return {string} Full path of the application support directory
     */
     native function GetApplicationSupportDirectory();
     appshell.app.getApplicationSupportDirectory = function (callback) {
         return GetApplicationSupportDirectory(callback);
     }
 
    /**
     * Returns the full path of the user extension directory.
     *
     * If the directory does not exist, it will be created.
     *
     * @param {function (err)} callback Asynchronous callback function with one argument (the error)
     * @return {string} Full path of the user extension directory.
     */
    native function GetExtensionDirectory();
    appshell.app.getExtensionDirectory = function (callback) {
        return GetExtensionDirectory(callback);
    }
```