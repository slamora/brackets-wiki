Brackets doesn't yet have functionality for discovering and installing extensions. In the meantime, here's a list of extensions that people have built. You can copy them into the `brackets/src/extensions/user` folder, then reload or restart Brackets.

If you've written an extension (even just as an experiment), please feel free to edit this page directly and add it (with some indication of what state it's in). Thanks!

**Code/text editing**
* [Snippets](https://github.com/jrowny/brackets-snippets): Assign trigger keys to insert snippets. Configurable with JSON
* [String Convert](https://github.com/mikechambers/StringConvert): Provides shortcuts for modifying and encoding strings within the editor.
* [Prefixr](https://github.com/davidderaedt/prefixr-extension): Generate browser specific CSS prefixes using the prefixr service.
* [Show Indentations](https://github.com/DennisKehrig/brackets-show-indentations): Visualizes the characters used for indenting lines
* [TabToSpace](https://github.com/davidderaedt/tabtospace-extension): Converts indentation to tabs or spaces
* [Align Assignments](https://github.com/deemeetar/AlignAssignments): Aligns assignment operators(Equal sings)

**Code generation**
* [App Cache Buddy](https://github.com/davidderaedt/appcache-gen): Generate and validate application cache manifests.
* [Annotate](https://github.com/davidderaedt/annotate-extension): Generates JSDoc annotations for your functions (VERY EARLY IMPLEMENTATION)

**General functionality**
* [Extension Manager](https://github.com/jdiehl/brackets-extension-manager): Install, Remove, and upgrade your extensions from the cloud from inside Brackets (requires node).
* [Related Files](https://github.com/jhatwich/brackets-related-files): Discovers and allows you to open related files in your project.
* [File Navigation Shortcuts](https://github.com/peterflynn/brackets-editor-nav): shortcuts for switching to next/previous editor (_non_-MRU order), and a version of Quick Open scoped to only open files.
* [Open File from URL](https://github.com/deemeetar/OpenFileFromUrl): Opens any ```href``` and ```rel``` atribute urls in editor on ```ALT+0``` shortcut. Currently works only with existing files. 
* [Brackets Commands Guide](https://github.com/peterflynn/brackets-commands-guide): Search and execute commands by typing part of their name, similar to Quicksilver (or Sublime's Ctrl+Shift+P or Eclipse's Ctrl+3).

**Live development**
* [Debugger](https://github.com/jdiehl/brackets-debugger): Brackets Debugger for the Live Development browser.
* [Everyscrub](https://github.com/peterflynn/everyscrub): Everything's a scrubber! Cmd/Ctrl + drag on any number or hex color to scrub its value and update the browser in real time.
* [Reload in Browser](https://github.com/DennisKehrig/brackets.ReloadInBrowser): Adds a toolbar button and shortcut to reload the page in the browser
* [V8/Node Live Development](https://github.com/DennisKehrig/brackets-v8-node-live): Updates scripts running in Node.js as you type

**Visual editing**
* [Color Picker](https://github.com/jdiehl/brackets-color-picker): Quick Edit on a hex color opens an inline color picker.
* [Color Editor](https://github.com/GarthDB/brackets-inline-color-editor): Quick Edit on a hex/rgb/hsl color opens an inline color picker, plus a listing of all colors used in the file.

**External tools**
* [GitHub](https://github.com/jrowny/brackets-github): Implements the GitHub API, including oAuth. Currently functionality limited to Gists.
* [ToGist](https://github.com/davidderaedt/togist): Create an anonymous gist from the current selection.
* (See also [Prefixr](https://github.com/davidderaedt/prefixr-extension) above).
* [PhoneGap Build](https://github.com/tpryan/brackets-phonegapbuild): Eventual goal is to be able to send a project to PhoneGap Build from Brackets. Still in early stages. 

**Documentation**
* [MDNLookup](https://github.com/pamelafox/brackets-MDNLookup-extension): Includes a way of creating an extensions toolbar and adding buttons to the toolbar with callbacks. Requires a slight change to the core.

**Linting & warnings**
* [CSSLint](https://github.com/cfjedimaster/brackets-csslint): CSSLint your documents.
* [JSHint](https://github.com/cfjedimaster/brackets-jshint): Performs a JSHint report.
* [W3CValidator](https://github.com/cfjedimaster/brackets-w3cvalidation): Run the W3C Validator on your HTML.
* (See also [App Cache Buddy](https://github.com/davidderaedt/appcache-gen) above).