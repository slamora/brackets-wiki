# Extension Package Format #

Status: Proposal

Brackets extensions follow a format established by CommonJS and npm.

## Package Files ##

Package files are zip files, with a package.json file in the top-level directory.

npm uses gzipped tar files, but the standard for Brackets extensions is zip files because:

1. they are more convenient for users to create on Windows and Mac
2. they can be created with a single click from GitHub

If more compatibility with npm proves to be desirable, we can always repack the extensions into gzipped tar files.

## package.json ##

Brackets uses the same [package.json format as npm](https://npmjs.org/doc/json.html). Many of the fields that make sense for Node are ignored in Brackets (such as scripts and man pages).

* `name` and `version` are required as in Node
* Brackets adds an optional `title` that is the pretty display form of `name`
* `description` is required, but will optionally be pulled from a README.txt or README.md file at the top of the package
* the `keywords` field will help users find the extensions they need (see below)
* `files` works the same as it does for npm, including the optional use of an `.npmignore` file
* Brackets does not support any of npm's dependencies-related features on installation. When we implement extension dependencies, those will likely be expressed as [peerDependencies](http://blog.nodejs.org/2013/02/07/peer-dependencies/)
* `engines` is used to specify the compatible Brackets versions. For most extensions, the version should be expressed as `>= (some Brackets version)` (in other words, there's no max version).
* We should honor the `private` flag to not publish private extensions

## keywords ##

Keywords can be anything that will help users find the right extension. Some conventions will help Brackets steer users to the right extensions quicker:

* Relevant file extensions including a leading ".". For example, if your extension is specifically for JavaScript files, include a ".js" keyword.

Additional keyword conventions will be created as we review the categories of extensions.

## extension modules ##

Subject to change from the [Extension API research](https://trello.com/card/5-research-extension-api/4f90a6d98f77505d7940ce88/769), Brackets will automatically execute `main.js` at the root of the package when the extension is loaded.

Modules that are imported via `require` are searched for from the root of the extension. For example, `require("foo/bar")` will look for `foo/bar.js` in the extension.

## Node Modules ##

Brackets extensions that use Node are expected to ship with the node_modules incorporated directly into the extension package. Including dependencies directly in a package for deployment [is considered a best practice](http://www.futurealoof.com/posts/nodemodules-in-git.html).

Node modules (or any modules for that matter) are *not* shared between extensions.

There are open questions surrounding the lifecycle of the Node code and the module format differences between Brackets and Node and these will be resolved as we head toward the Extension API research story.

## Changelog ##

If there is a CHANGELOG.md file at the package root, the h2 headers (delimited by ##) are assumed to contain version numbers and the sections of the file will be pulled apart to display update information to users.

## unit tests ##

A unit test module can be located at `unittests.js` at the root of the package.

## module files ##

**Open question: npm uses CommonJS modules, Brackets uses AMD, how do we reconcile?**