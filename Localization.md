## TL;DR

### Using Localized Strings

The snippets below show how to use localized strings in core Brackets code.

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
    templateContent = require("text!templateContent.html"); // load text content of template file

var html = Mustache.render(templateContent, Strings); // use Mustache to insert translated strings
```

## Implementation Details

Brackets uses the [i18n plugin for RequireJS](https://github.com/requirejs/i18n) to load translations. The locale is determined by ``brackets.app.language`` (``navigator.language`` isn't used due to a CEF3 bug). Our main ``strings`` module re-exports the root bundle (i.e. ``require("i18n!nls/strings.js")``). Client code should only use the main strings module (i.e. ``require("strings")``).

As of Sprint 13, brackets-shell hardcodes strings for both English and French, primarily for the limited set of native menus. Once [native menus](https://trello.com/c/Zc2LP82u) are implemented, there will be no translated strings in brackets-shell.

### Creating New Translations
[Creating new translations](https://github.com/adobe/brackets/blob/master/src/nls/README.md)

### Localizing Extensions
[Example: Localized Extension](https://github.com/adobe/brackets/tree/master/src/extensions/disabled/LocalizationExample)
[README.MD](https://github.com/adobe/brackets/tree/master/src/extensions/disabled/LocalizationExample/README.MD)