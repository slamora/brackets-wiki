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

##Initializing a hinting context on file open

If the opened file had already been added to Tern then no new files need to be read in. If the previous file has been modified (does not need to be saved) then the contents of the new file will be updated in Tern.

When a JavaScript or HTML file is opened in Brackets the JS Code Hinter reads additional files and adds those to Tern to be analyzed. All of the JS files in the directory of the opened file are added to Tern. More files may be added depending on if the opened file uses requirejs. This directory of the initial file is considered the root directory of the JS Code Hinter.

After the files in the current directory are added to Tern, the opened file is analyzed. If the opened file uses requirejs to create dependencies, all of the dependent files will be added to Tern.

If the opened file does not have dependencies on other files, it is assumed that more files will be needed to provide a better context for hinting. First the JS files in the subdirectories of the opened file will be added to Tern to provide more context. If the opened file is in a subdirectory of the Bracket's project root, then JS files from the project's root directory will be added to Tern as well. 

The JS Code Hinter limits the number files that can be added to Tern at 100.

## Configuration

Very large and complicated JavaScript files can cause performance issues. A JS Code Hints configuration file provides the ability to work around these issues. When Brackets opens a new project root, if a configuration file is found, the settings are used to control loading files for code hinting. Configuration files are only updated when changing the project root. Opening new files will not cause a search for a new configuration file. The configuration file is a JSON formatted file named “.jscodehints”.

The following properties are supported:

* **excluded-directories**   
An array of strings or regular expressions that match directories relative to the project root. Matching directories will be excluded from analysis. Directories may be excluded if they contain automated tests that aren’t relevant for code hinting. There are two kinds of strings that are supported. The first is a simple string that may contain the wildcards “*” and “?”. The second is a regular expression literal embedded in a string. The default value is an empty array.

* **excluded-files**  
An array of strings or regular expressions that match files that will be excluded from analysis. Files are typically excluded because their API is in a JSON file or they are known to cause problems with either stability or performance. There are two kinds of strings that are supported. The first is a simple string that may contain the wildcards “*” and “?”.  The second is a regular expression literal embedded in a string. The default value is ["require.js", "jquery*.js", "less*.min.js", "ember*.js"].
* **max-file-count**   
Limits the total number of files that can be processed for hints. This limit only applies when an opened file does not use "require" and files under the project root are being added to the hinting context. The default value is 100.

* **max-file-size**  	
Files larger than this number of bytes will not be parsed. The default value is 524,288 bytes.

The strings in "excluded-directories" or "excluded-files" will be treated as a regular expression if the first and last characters of the string are the '/' character. Note the '\' character in a regular expression needs to be escaped to be valid in a JSON formatted file. For example "/[\d]/" becomes "/[\\\\d]/".

**Example file:**

    {               
         "excluded-directories" : ["excluded-dir", "/tests-.*files/"],  
         "excluded-files" : ["require.js", "jquery*.js", "/lib-1.[12].js/"],  
         "max-file-count": 100,   
         "max-file-size": 524288  
    }
