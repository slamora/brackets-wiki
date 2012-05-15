Brackets uses some specific coding conventions. All of the pull requests that come in should adhere to the following rules:
## Basics ##
* All JavaScript files have a single module definition using the following format:

```javascript
define(function (require, exports, module) {

    // Load dependent modules
    var Module = require("Module");

    function someFunction() {
        // ...
    }

    // Export public APIs
    exports.someFunction = someFunction;
}
````

* Must pass JSLint. The default settings for JSLint are:

````
/*jslint vars: true, plusplus: true, devel: true, nomen: true, indent: 4, maxerr: 50 */
/*global define */
````

## Naming and Syntax ##
* variable and function names use camelCase (not under_scores)

<table width="100%">
  <tr>
    <td style="background: #afa">
      Good: var 
    </td>
  </tr>
  <tr>
    <td style="background: #faa">
      Bad: var 
    </td>
  </tr>
</table>

* use $ prefixes on variables referring to jQuery objects
* use _ prefixes on private variables/methods
* classes and id's in HTML use all lower-case with dashes (-), not camelCase or under_scores
* use semicolons
* use double quotes in JavaScript

## Code Structure ##
* Closure vars should go at the top of the code block they're scoped to
* use of Array.forEach() instead of $.each()
* 4-space indents (soft tabs)

## Comments ##
* All comments should be C++ single line style //comment.
* Even multiline comments should use // at the start of each line
* Use C style /* comments */ for notices at the top and bottom of the file
* Annotations should use the /** annotation */
* Annotate all functions