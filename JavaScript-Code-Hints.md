## Tern.JS Integration 
The JavaScript Code Hints feature is implemented as a Brackets extension. For hinting, it relies on the [TernJS Library](http://www.ternjs.net).  

The extension manages an instance of the Tern server, which runs in a web-worker, and makes requests to it whenever it needs information to calculate hints.  The information requested includes function types, types of the base of a member expression, and potential property or identifier names.  The extension caches most of the information it requests so that it does not need to continuously requery tern during the same code hinting session.

When the extension starts up, it reads in some .json files that provide type information for the environment.  Currently it reads in ecma5.json, which contains type info for the javascript builtins, and browser.json, which provides type information for the methods that the browser exposes.  These files are only parsed once, as they should never change.  When the tern server is initialized, the environment built from those files is passed to tern to initialize its environment.  To support different modes of development (e.g. editing a node.js file vs. a browser based file) we could supply tern with different environments to get the most appropriate hints.

The hints are calculated based on the files in the current directory, and on the calls to requirejs seen in the file.

## API Proposals and Guesses
Hints are displayed as the user is typing or explicitly by the Ctrl+space key. Three different kinds of hints are available: property hint, identifier hint, and function hint. If the cursor is following a "." then property hints are displayed. If the cursor is inside function parentheses, then the function's argument types and return type are displayed. Otherwise all known identifiers, classes and top-level functions are hinted.

**Property hints:**
* When the type of the object is known, the properties for that type are displayed.
* When the type of the object is unknown or there are no more matching entries in the objects's properties, guesses are shown. Guesses are shown in italics.

**Identifier hints:**

Identifier hints contains the list of identifiers following by literals, and keywords.

* Identifiers - Known identifiers, classnames, and functions. Sorted by depth – current scope depth shown in green, the next outer scope in yellow, then blue, and the rest in black.
* Literals - "true", "false", "null", "undefined".
* Keywords – sorted alphabetically, displayed in monospaced font.

**Function hint:**

Displays the functions parameter types and the return type.

**String Literals:**

Currently we do not hint string literals.

**Interaction:**

As the user types, the lists of choices in the hints pop-up is narrowed. All the character matches are case-insensitive. Character matching and sorting is handled by the [StringMatcher](https://github.com/adobe/brackets/blob/master/src/utils/StringMatch.js) utility. It has an advanced character matching algorithm and sorts the best matches to the top.

The code that filters and sorts the hints is in the getHints() method in [Session.js](https://github.com/adobe/brackets/blob/master/src/extensions/default/JavaScriptCodeHints/Session.js). The results are returned to the getHints() in [main.js](https://github.com/adobe/brackets/blob/master/src/extensions/default/JavaScriptCodeHints/main.js) where they are formatted. Formatting is done by adding CSS class names to hint objects. The CSS classes are defined in [brackets-js-hints.css](https://github.com/adobe/brackets/blob/master/src/extensions/default/JavaScriptCodeHints/styles/brackets-js-hints.css)

## Go-To-Definition (of current identifier)
Go-To-Definition allows the user to have the cursor move to the definition of the variable or function to which the cursor currently points.  This works for definitions within the current file and within all other open files.  Definitions are selected/highlighted when found.

**Interaction:**

This feature is controlled by the Navigation Menu -> Jump to Definition (control-J).  Note that there is already a different Brackets feature called Go-To-Definition.  We expect the names of both features to change.