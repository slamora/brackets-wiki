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

## Jump-To-Definition (of current identifier)
Jump-To-Definition allows the user to have the cursor move to the definition of the variable or function to which the cursor currently points.  This works for definitions within the current file and within all other open files.  Definitions are selected/highlighted when found, with the cursor positioned at the end of the selection.

**Interaction:**

This feature is controlled by the Navigation Menu -> Jump to Definition (control-J).

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
An array of strings or regular expressions that match directories relative to the project root. Matching directories will be excluded from analysis. Directories may be excluded if they contain automated tests that aren’t relevant for code hinting. The default value is an empty array. Filter values are matched against the _project-relative_ path of the directory, excluding trailing slash. Two types of filter values are supported:
    * A simple string, which may contain the wildcards `*` and `?`. (Note: unlike [search file exclusions](Using File Filters), `*` here matches all characters _including_ path separators). This string must match the _entire_ project-relative path in order to exclude the directory, so you may need to add leading/trailing `*`s.
    * A regular expression literal embedded in a string (wrapped in `/`s), e.g. `"/thirdparty/mylibrary-1\\.\\d/"` (note the double-escaping due to the regexp being inside a JSON string literal). This need not match the entire project-relative path, unless you manually add `^` and `$` to it.

* **excluded-files**  
An array of strings or regular expressions that match files that will be excluded from analysis. Files are typically excluded because their API is in a JSON file or they are known to cause problems with either stability or performance. Brackets always excludes ["require.js", "jquery*.js", "less*.min.js", "ember*.js", ".*"]. Any settings you apply will be _in addition to_ these defaults.
    * A simple string, which may contain the wildcards `*` and `?`. This string must match the _entire_ filename including extension.
    * A regular expression literal embedded in a string (wrapped in `/`s), e.g. `"/mylibrary-1\\.\\d\\.js/"` (note the double-escaping due to the regexp being inside a JSON string literal). This need not match the entire filename, unless you manually add `^` and `$` to it.

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

## Advanced Hinting
JS Function Parameter and Documentation Hints

In addition to the current hints, the JS code hints will provide independent windows for function parameter hints and documentation hints. The look and feel of the three windows will be similar to the Eclipse implementation. The function parameter hints will be displayed above the current line; the main hints below the current line, and documentation on a highlighted hint will pop out to the side of the main hint window.

 

The function parameter hints will be displayed after the user chooses a function from the hints popup, inserting the function text and dismissing the main hint window. Function parameter hints can be re-displayed anytime the cursor is inside a function by pressing Shift+Ctrl+Space. The parameters will be displayed in a format similar to closure annotation but without enclosing brackets around the type annotation. Optional parameters will be enclosed in brackets.

The main hints themselves will be unchanged from the current format. 

Documentation on a function or variable will pop out to the side of the main hint window when a hint is highlighted. A small delay between highlighting a hint and displaying the documentation will prevent flash in the case where the user is quickly moving through the hints. The documentation for functions will contain the function signature, origin, description, parameters, and return type. The documentation window for variables and properties will show the type, origin, and description. The figure below shows a documentation hint for a highlighted function.

# Refactoring Project (spring 2014)

Two significant features with an impact on JS Code Hints shipped in Brackets 36: file watchers and preferences. As a result, we've chosen this time to [refactor the JS code hints code](https://trello.com/c/BMVw25vU/75-research-js-code-hints-cleanup) in order to improve the overall performance of code hints.

## Preferences

This is the easiest piece to refactor, because the JS Code Hints-specific preferences system in Preferences.js should just go away. In its place, JS Code Hints will rely on a prefixed preferences system for "jscodehints". `max-file-count` and `max-file-size` are replaced by `jscodehints.maxFileCount` and `jscodehints.maxFileSize`. Ideally, we can eliminate `maxFileCount`.

The excluded directories and excluded files should be path-based preferences. It is a llittle more verbose:

```javascript
{
    "path": {
        "foo/foobar.js": {
            "jscodehints.exclude": true
        },
        "thirdparty/**.js": {
            "jscodehints.exclude": true
        }
    }
}
```

vs.

```javascript
{
    "jscodehints.exclude": ["foo/foobar.js", "thirdparty/**.js"]
}
```

But it provides consistent handling of file matching and using the current APIs JS Code Hints can simply request the value of `jscodehints.exclude` with a context of each file it needs to check.


## Code Hints "Light"

If there are too many files, can we do something small and lightweight to gather up symbols from the other files? Or is this too difficult to integrate with Tern results?

## Breakdown of Code Hints issues

### Crashes

* [Brackets grey screens and crashes when editing JS files](https://github.com/adobe/brackets/issues/7514)
* [Mac OSX - Freeze - Grey Screen](https://github.com/adobe/brackets/issues/7308)
* [Entire window goes blank](https://github.com/adobe/brackets/issues/7262)
* [Brackets crashes](https://github.com/adobe/brackets/issues/7025)
* [CPU stays at 100% with specific project](https://github.com/adobe/brackets/issues/7245)

### Hints not behaving as expected

* [Suboptimal behaviour of Quick Edit](https://github.com/adobe/brackets/issues/7003)
* [Missing intellisense on too big file](https://github.com/adobe/brackets/issues/6986)
* [No code hints for "this" when superclass is unknown](https://github.com/adobe/brackets/issues/6929)
* [Requires don't work if extension missing in filename (node)](https://github.com/adobe/brackets/issues/5993)
* [Display redefined variables hints as guesses](https://github.com/adobe/brackets/issues/5729)
* [JS code hints aren't picking up new items added to exports](https://github.com/adobe/brackets/issues/4991)
* [code hints are affected by a javascript loading a module with require](https://github.com/adobe/brackets/issues/4192)
* [jump to definition inside quick editor on a function not working](https://github.com/adobe/brackets/issues/3951)
* [quick editor not open when function name has non ascii chars](https://github.com/adobe/brackets/issues/3941)
* [Support require() calls not in an AMD wrapper](https://github.com/adobe/brackets/issues/3801)
* [Incorrect jQuery hints when switching to a $. from $(something)](https://github.com/adobe/brackets/issues/3685)
* [Should not show hints after typing a dot with no function call before it](https://github.com/adobe/brackets/issues/3682)
* [include JSLint-defined globals story](https://trello.com/c/AOEjxuDl/1011-js-code-hints-should-include-jslint-defined-globals)
* [Code hints for require() path strings story](https://trello.com/c/0HWEDXGE/997-code-hints-for-require-path-strings)
* [Improved code hints for extension authors story](https://trello.com/c/xBlcCYju/990-improved-code-hints-for-extension-authors)
* [Better formatting for optional arguments story](https://trello.com/c/jm6sMvdz/955-js-code-hints-better-formatting-for-optional-arguments)

**Problems in Tern**

* [Inconsistent JS hints depending on whitespace](https://github.com/adobe/brackets/issues/5263)
* [Object .toString / toJson / toValue](https://github.com/adobe/brackets/issues/4183)
* [no code hint for jQuery.Event in on( events, handler(eventObject))](https://github.com/adobe/brackets/issues/4181)
* [parameter type later becomes variable name when passing object as param to a function](https://github.com/adobe/brackets/issues/3838)
* [Incorrect function return type for jQuery setter functions](https://github.com/adobe/brackets/issues/3684)
* [code hints in inherited class by Class.create not showing, later use guess hint causes existing hint changed](https://github.com/adobe/brackets/issues/3660) – this appears to be Prototype library specific. I would probably call this "no priority" or even just close it.
* [console.assert() missing from code hints](https://github.com/adobe/brackets/issues/3655)

### Performance

* [make jump to definition response faster (on large JS framework file)](https://github.com/adobe/brackets/issues/4066)
* [Apparent performance test regression in JS Quick Edit when Tern doesn't find results](https://github.com/adobe/brackets/issues/3961)
* [JS Code Hints File Handling Revamp story](https://trello.com/c/VjAiuH31/1070-js-code-hints-file-handling-revamp)

### StringMatch

* [Code hints should filter out unlikely matches](https://github.com/adobe/brackets/issues/5993)
* [Prefer results that match case of the query string](https://github.com/adobe/brackets/issues/3971)

### Code Cleanup

* [ScopeManager is inconsistent about path endings](https://github.com/adobe/brackets/issues/5529)

### Unit tests

* [Unit tests for 6931, referencing members of a class](https://github.com/adobe/brackets/issues/7152)
* [Console errors when running unit tests](https://github.com/adobe/brackets/issues/6937)
* [Intermittent test failure](https://github.com/adobe/brackets/issues/7646)
* [Automated testing for JS code hinting with various frameworks/projects](https://trello.com/c/gqdpxfM2/937-automated-testing-for-js-code-hinting-with-various-frameworks-projects)

### Preferences Handling

* [JS code hint exclusions ignored sometimes](https://github.com/adobe/brackets/issues/7342)
* [Ability to turn off code hinting](https://github.com/adobe/brackets/issues/4716)
* [max-file-count takes less files to be processed for hints](https://github.com/adobe/brackets/issues/4195)
* [excluded-files preference does not support directory names in file path](https://github.com/adobe/brackets/issues/4191)
* [when open an excluded file is opened methods and properties not excluded](https://github.com/adobe/brackets/issues/4190)
* [Understand require configuration story](https://trello.com/c/mq7dZnlv/1049-js-code-hints-understand-require-configuration)
* [JS Code Hint preferences story](https://trello.com/c/BOgGIzWW/1046-js-code-hint-preferences)

### Other

* [JavaScript Jump To Definition Ctrl+J/Cmd+J not mentioned in Right-Click Menu](https://github.com/adobe/brackets/issues/3860)

# The Rework

These are the goals for the rework:

1. Better stability
2. Better performance
3. Easier testability with more predictability
4. Improved results
5. Start integrating into a reusable, extensible project model
6. Less code

## General code notes

The line between the ScopeManager and the Session is confusing. Ahh, maybe it's the case that ScopeManager is like the model and Session connects the ScopeManager to the requests from the editor (the view model?). main.js is the one that glues it all together.

## Better Stability

The biggest stability issue is that Tern sometimes runs out of control. There's no one cause to these cases, just certain scripts that cause issues. If we can make a reproducible test case, we can pass that along to Marijn and likely get a fix. The goal should be to reduce the chances that users will have Brackets crash while continuing to give them code hints as much as possible.

* track that an operation started and didn't end – remember the file that was being used, automatic blacklist, notify user and ask them to report bug
* distribute a blacklist until issues are resolved (may not be necessary if we do the previous one). The blacklist should point to the Brackets issue
* Update Tern. Haven't done this in a while
* Question: is it possible for the main thread to detect that it hasn't received a response in a reasonable time and kill off the worker?

These blacklists would be separate from the user's own exclusions. Users may have other reasons to exclude files from hinting.

## Better Performance

The main performance issues that the JS code hints implementation has is: 

* redoing work that has already been done
* doing too much work (reading files that it doesn't need to read)

The second one is hard because it's hard to know for certain which files should be read when RequireJS is not used. The current heuristics may not be unreasonable. (**Enumerate below so that people can judge.**) From `ScopeManager.doEditorChange`:

* canSkipTernInitialization checks that the file is among the resolvedFiles
* If JSCH hasn't looked at the file before, it's going to restart Tern from scratch
* Attempts to read all files under the project root
* Read sibling files
* If not using modules, read subdirectories of new file's directory

Thanks for file watchers, we can make JS code hints smarter about redoing work. This should also allow us to improve some of the results because we won't be doing that extra work.

When starting up for a project, we send the files with full paths. Then Tern requests some of the same files again (probably because it notices the `require` calls). For example, if you have ScopeManager open, we send ScopeManager to Tern as part of the pump priming... but we send it with the full path. Tern then requests ScopeManager, without the rest of the path, so we send the same file again. I don't know if we can avoid that.

Switching out of Brackets and switching back did not cause a re-read of all of the files. Switching between files did not cause re-reading either. The re-reading that we're seeing must be the `MAX_HINTS` limit.

Todo list:

* Exclude files requested by Tern if they are in the exclusions list (confirmed that ScopeManager.handleTernGetFile doesn't do this)
* Watch for file changes and update Tern when they happen

## Easier, faster, more predictable tests

It should be possible to generate code hints without an editor or proper document for many tests. It may be cleaner if there was a way to instantiate a Session that fully encapsulates code hinting state so that it's easy to set up and tear down for tests (and would likely be more understandable as a unit than the current Session/ScopeManager split).

A Session would maintain state for a project.

On my machine, the test suite takes 60-90 seconds to run. The largest portion of that is CodeMirror-related. While it's good to have *some* cases that test the interaction between the hints and the editor, it seems to me that most of the cases should be focused on the results we expect to see from the hinting engine. Imagine the information about the JavaScript code as the model and the editor as the view. We could build a bunch of tests for the model and some that are more integration-style tests that exercise the whole stack.

Ability to switch code hints to run in the main thread would make troubleshooting considerably easier.

## Improved Results

node support? Custom configuration of Tern? Include custom def files?

* Can we eliminate the "large line count"?
* Can we eliminate MAX_HINTS?

## Tern Configuration

Should we take better advantage of [Tern's configuration](http://ternjs.net/doc/manual.html#configuration)? (Well, yes...)

For example, Tern can be told to `loadEagerly`. Perhaps that's an option instead of us just shoving files at Tern. It can also be told to `dontLoad`, which means that maybe exclusions can be implemented on the Tern side.

We should absolutely allow configuration of the RequireJS and Node plugins for Tern. We should also allow custom "libs" values.

Is there a way for us to allow a mix of extensions and preferences? For example, can an extension provide a .defs file? Is that important?
