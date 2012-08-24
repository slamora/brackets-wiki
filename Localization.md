## TL;DR

### Using Localized Strings

In JavaScript
```
var Strings = require("strings"); // load the Strings module
...
$("<span/>").text(Strings.CMD_ABOUT); // insert a localized string
```

In an HTML template
```
<!-- templateContent.html -->
<span>{{CMD_ABOUT}}</span>

/* JavaScript */
var Strings = require("strings"),
    templateContent = require("text!templateContent.html");

var html = Mustache.render(templateContent, Strings);
```

### Creating New Translations
[Creating new translations](https://github.com/adobe/brackets/blob/master/src/nls/README.md)

### Localizing Extensions
[Example: Localized Extension](https://github.com/adobe/brackets/tree/master/src/extensions/disabled/LocalizationExample)
[README.MD](https://github.com/adobe/brackets/tree/master/src/extensions/disabled/LocalizationExample/README.MD)