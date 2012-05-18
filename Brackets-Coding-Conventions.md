Brackets uses some specific coding conventions. All of the pull requests that come in should adhere to the following rules:
## Basics ##
* Use 4 space indents (spaces, not tabs)

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
```

* Must pass JSLint. The default settings for JSLint are:

```
/*jslint vars: true, plusplus: true, devel: true, nomen: true, indent: 4, maxerr: 50 */
/*global define */
```

## Naming and Syntax ##
<br/>
Variable and function names use camelCase (not under_scores):
> Do this:
>
> ```
> var editorHolder; 
> function getText();
> ```
>
> Not this:
>
> ```
> var editor_holder; // Don't use underscore!
> function get_text(); // Don't use underscore!
> ```

<br/>
Use $ prefixes on variables referring to jQuery objects:
> Do this:
>
> ```
> var $sidebar = $("#sidebar");
> ```
>
> Not this:
>
> ```
> var sidebar = $("#sidebar"); // Use '$' to prefix variables referring to jQuery objects
> ```

<br/>
Use _ prefixes on private variables/methods:
> Do this:
>
> ```
> var _privateVar = 42;
> function _privateFunction() 
> ```
>
> Not this:
>
> ```
> var privateVar = 42; // Private vars should start with _
> function privateFunction() // Private functions should start with _
> ```

<br/>
Classes and id's in HTML use all lower-case with dashes (-), not camelCase or under_scores:
> Do this:
>
> ```
> <div id="search-results">
> <span class="title-wrapper">
> ```
>
> Not this:
>
> ```
> <div id="searchResults">  // Don't use camel-case for ids
> <span class="title_wrapper"> // Don't use underscore
> ```

<br/>
Use semicolons:
> Do this:
>
> ```
> var someVar;
> someVar = 2 + 2;
> ```
>
> Not this:
>
> ```
> var someVar   // Missing semicolon!
> someVar = 2 + 2   // Missing semicolon!
> ```

<br/>
Use double quotes in JavaScript:
> Do this:
>
> ```
> var aString = "Hello";
> someFunction("This is awesome!");
> ```
>
> Not this:
>
> ```
> var aString = 'Hello'; // Use double quotes!
> someFunction('This is awesome!'); // Use double quotes!
> ```

## Code Structure ##
<br/>
Use of Array.forEach() instead of a for loop or $.each():
> Do this:
>
> ```
> var anArray = [1, 2, 3, 4];
> anArray.forEach(function (value, index) {
>     // ...
> });
> ```
>
> Not this:
> ```
> var anArray = [1, 2, 3, 4];
> for (var i = 0; i < anArray.length; i++) {  // Use Array.forEach()
>     // ...
> }
> ```
>
> Or this:
> ```
> var anArray = [1, 2, 3, 4];
> $.each(anArray, function (index, value) {  // Use Array.forEach()
>     // ...
> })
> ```


## Comments ##
* All comments should be C++ single line style //comment.
* Even multiline comments should use // at the start of each line
* Use C style /* comments */ for notices at the top and bottom of the file
* Annotations should use the /** annotation */
* Annotate all functions