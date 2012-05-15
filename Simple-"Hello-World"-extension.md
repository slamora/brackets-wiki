```javascript
/*jslint vars: true, plusplus: true, devel: true, nomen: true, indent: 4, maxerr: 50 */
/*global define, $, brackets */

define(function (require, exports, module) {
    'use strict';
    
    // Brackets modules
    var DocumentManager = brackets.getModule("document/DocumentManager");
    
    var $item = $("<li></li>");
    $("<a href='#' id='hello-world'>Hello World</a>")
        .click(function() {
            alert("Hello Document: " + DocumentManager.getCurrentDocument().file.fullPath);
        })
        .appendTo($item);
    $("#menu-experimental-usetab").parent().after($item);
});
```