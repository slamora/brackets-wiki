# Extension Package Format #

Status: Proposal

Brackets extensions follow a format established by CommonJS and npm. Where there are differences between the two, preference is given to the npm format (due to the size of the ecosystem). Differences between npm and Brackets usage of the packages will be noted below.

## Package Files ##

Package files themselves are gzipped tar files, with a package.json file in the top-level directory.

## package.json ##

Brackets uses the same [package.json format as npm](https://npmjs.org/doc/json.html). Many of the fields that make sense for Node are ignored in Brackets (such as scripts and man pages).

* `name` and `version` are required as in Node
* Brackets adds an optional `title` that is the pretty display form of `name`
* `description` is required, but will optionally be pulled from a README.txt or README.md file at the top of the package
* the `keywords` field will help users find the extensions they need (see below)
* `files` works the same as it does for npm, including the optional use of an `.npmignore` file

## keywords ##

Keywords can be anything that will help users find the right extension. Some conventions will help Brackets steer users to the right extensions quicker:

* Relevant file extensions including a leading ".". For example, if your extension is specifically for JavaScript files, include a ".js" keyword.

Additional keyword conventions will be created as we review the categories of extensions.

## module files ##

**npm uses CommonJS modules, Brackets uses AMD**