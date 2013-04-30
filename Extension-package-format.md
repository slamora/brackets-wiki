# Extension Package Format #

Status: Implemented

Note: this will likely undergo revision during the [extension API research](https://trello.com/card/5-research-extension-api/4f90a6d98f77505d7940ce88/769)

Brackets extensions follow a format established by CommonJS and npm.

## Package Files ##

Package files are zip files, with a package.json file in the top-level directory.

npm uses gzipped tar files, but the standard for Brackets extensions is zip files because:

1. they are more convenient for users to create on Windows and Mac
2. they can be created with a single click from GitHub

If more compatibility with npm proves to be desirable, we can always repack the extensions into gzipped tar files.

## package.json ##

Brackets uses the same [package.json format as npm](https://npmjs.org/doc/json.html). There is a sample package.json in the section below. Many of the fields that make sense for Node are ignored in Brackets (such as scripts and man pages).

* `name` and `version` are required as in Node
* Brackets adds an optional `title` that is the pretty display form of `name`
* `description` is required, but will optionally be pulled from a README.txt or README.md file at the top of the package
* the `keywords` field will help users find the extensions they need (see below)
* `files` works the same as it does for npm, including the optional use of an `.npmignore` file
* Brackets does not support any of npm's dependencies-related features on installation. When we implement extension dependencies, those will likely be expressed as [peerDependencies](http://blog.nodejs.org/2013/02/07/peer-dependencies/)
* `engines` is used to specify the compatible Brackets versions. For most extensions, the version should be expressed as `>= (some Brackets version)` (in other words, there's no max version).
* We should honor the `private` flag to not publish private extensions

**note** by reusing `engines` and `peerDependencies`, we may run into friction with npm, assuming that we allow users to install and use npm modules. If need be, we can change to using two other Brackets-specific fields in package.json for these needs.

## Sample package.json ##

`name` and `version` are required. `title`, `description` and `license` are also recommended.

```javascript
﻿﻿{
    "name": "unique-lowercase-identifier",
    "title": "Pretty Title for the UI",
    "description": "More useful details to let someone know what your extension is all about.",
    "version": "1.0.0",
    "author": "Your Name <your@email> (http://your.url)",
    "license": "MIT",
    "engines": {
        "brackets": ">=0.24.0"
    }
}
```

## Categories ##

Our package.json files include a "categories" field which is not present in npm's format. Categories, unlike keywords, are primarily a "browsing" feature rather than a searching one.

The `categories` field can be either a string (the common case, since most extensions have only one), or an array of strings.

Each of the strings should be one of the following boldface values:

* **editing** – Editing and Navigation
* **snippets** – Snippets and Shorthand
* **formatting** – Formatting
* **codegen** – Code Generation
* **language** – Language Support
* **general** – General Functionality
* **livedev** – Live Development
* **visual** – Visual Editing
* **external** – External Tools and Online Content
* **docs** – Documentation
* **linting** – Linting and Warnings
* **testing** – Testing and Code Metrics

## Keywords ##

Keywords can be anything that will help users find the right extension. Some conventions will help Brackets steer users to the right extensions quicker:

* Relevant file extensions including a leading ".". For example, if your extension is specifically for JavaScript files, include a ".js" keyword.

Additional keyword conventions will be created as we review the categories of extensions.

## Extension Modules ##

Subject to change from the [Extension API research](https://trello.com/card/5-research-extension-api/4f90a6d98f77505d7940ce88/769), Brackets will automatically execute `main.js` at the root of the package when the extension is loaded.

Modules that are imported via `require` are searched for from the root of the extension. For example, `require("foo/bar")` will look for `foo/bar.js` in the extension.

## Node Modules ##

Brackets extensions will be able to provide modules that run in a Node process. The exact location of these modules will depend on the answers to the open questions at the end of this section.

Brackets extensions will also be able to use modules downloaded via npm. The `node_modules` directory will be incorporated directly into the extension package. Including dependencies directly in a package for deployment [is considered a best practice](http://www.futurealoof.com/posts/nodemodules-in-git.html).

Node modules (or any modules for that matter) are *not* shared between extensions.

There are open questions surrounding the lifecycle of the Node code and the module format differences between Brackets and Node and these will be resolved as we head toward the Extension API research story.

## Changelog ##

If there is a CHANGELOG.md file at the package root, the h2 headers (delimited by ##) are assumed to contain version numbers and the sections of the file will be pulled apart to display update information to users.

## Unit Tests ##

A unit test module can be located at `unittests.js` at the root of the package.

## module files ##

**Open question: npm uses CommonJS modules, Brackets uses AMD, how do we reconcile?**

(This will be investigated during the extension API research. We have discussed a move to pure CommonJS modules.)

## Localization ##

The pattern for localizing extensions is to put an `nls` directory at the root, containing a `strings.js` module. Next to that module, there is a `root` directory with the baseline set of strings and a directory for each locale's specific strings. See the [Localization Example extension](https://github.com/adobe/brackets/tree/master/src/extensions/samples/LocalizationExample).