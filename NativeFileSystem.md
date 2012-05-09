**Module Name** : NativeFileSystem

**Path**        : file/NativeFileSystem.js

The NativeFileSystem module provides an API for accessing and working with files and directories on the users system. It follows the [w3c File System API Draft Specification](http://www.w3.org/TR/file-system-api/). From the source comments, it appears that is was implemented using [April 11, 2011 draft](http://www.w3.org/TR/2011/WD-file-system-api-20110419/) of the specification.

## Usage

    var NativeFileSystem = require("file/NativeFileSystem").NativeFileSystem;
    var DirectoryEntry = NativeFileSystem.DirectoryEntry;

## Things to Watch out for

### Encodings

When specifying UTF file encodings, you should use _utf8_ as opposed to _utf-8_.

### Security Errors

Currently, any errors that occur which do not map to standard FileError errors, will be mapped to FileError.SECURITY_ERR (as [specified by the File API draft](http://www.w3.org/TR/file-system-api/#definitions)).

This includes:

* ERR_UNKNOWN
* ERR_INVALID_PARAMS
* ERR_UNSUPPORTED_ENCODING

If you run into a FileError.SECURITY_ERR while working with the API, you should debug as if it is a security error, or one of the errors listed above.