Brackets uses [Google Closure Compiler Annotation format](https://developers.google.com/closure/compiler/docs/js-for-compiler) for API Documentation.

The API Docs for the current master branch can be found at [http://brackets.io/docs/current](http://brackets.io/docs/current).

## Guidelines

### Header Comments

Header comments can _not_ have an empty line between end of comment and module definition:

```
/**
 * Header comment
 */
define(function (require, exports, module) {
```

Explicitly add an empty line for comments that should _not_ appear in API Doc:

```
/*jslint vars: true, plusplus: true, devel: true, nomen: true, indent: 4, maxerr: 50, regexp: true */
/*global define, $, brackets, window */

define(function (require, exports, module) {
```

### Markdown Formatting


## Generating Documents

The [apify nodejs app](http://github.com/jbalsas/apify) is used to generate the Brackets API Docs.

If you have the apify app installed, then you can use the [Brackets Apify extension](http://github.com/jbalsas/brackets-apify) to generate API Documentation on-the-fly for your local branch.

