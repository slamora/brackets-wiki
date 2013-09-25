## Opening Extension Manager

To the right of the work area, click on the icon that looks like a LEGO(r) brick.  This will open the Extension Manager.

## Installing and Removing Extensions

You can browse a list of available extensions in the **Available** tab of the Extension Manager, and click **Install** to install an extension with one click. This list comes from the [Brackets Registry](https://brackets-registry.aboutweb.com), a web app that makes it easy for extension authors to upload new extensions. The registry doesn't yet have the full set of extensions from the original wiki page list (see below), but over time it should become more complete. 

You can remove an extension from Extension Manager by clicking on the **Remove** button in the item's listing in the "Installed" tab.

If you're an extension author, please upload your extension to the registry at https://brackets-registry.aboutweb.com, and move your listing on this page to the "Moved Extensions" list. Our hope is to eventually migrate all the extensions into the registry. For more information, see [Extension Registry Help](https://github.com/adobe/brackets/wiki/Extension-Registry-Help).

![Extension Manager Registry](screenshots/extension-manager/registry.png)

### Updating Extensions

If you've installed an extension from the registry and the developer has uploaded an update, an **Update** button will appear next to it in the Installed tab, and you can simply click on that button to update it.

If you installed the extension using the "Install from URL" dialog, and you have a URL for a newer version (or the GitHub repo has been updated since you installed it), you can just use the "Install from URL" dialog again to update it.

### Manual Install

You can manually install extensions that aren't in the registry yet by choosing **File > Extension Manager...** (or clicking on the "brick" icon in the toolbar), then clicking the **Install from URL...** button at the bottom. From there, you can install most of the extensions below by doing the following:

* On this page, right-click the extension link and copy the URL.
* Switch back to Brackets.
* Paste the link into the Install Extension dialog, then click **Install**.

(Some extensions might not work by just copying and pasting the GitHub repo URL if they have submodules. Authors who post such extensions are encouraged to link directly to a ZIP file with all the submodules included.)

## Old Extensions List ##

Here's a list of extensions that people have built that haven't yet been uploaded to the registry. **If you write an extension, please upload it to the Brackets registry at https://brackets-registry.aboutweb.com instead of posting it here.** Once all the existing extensions have been moved, we'll be deprecating this page.

Also be sure to use the [Brackets Shortcuts](https://github.com/adobe/brackets/wiki/Brackets-Shortcuts) page to see which shortcuts are available and also to add the shortcuts that you have used. Thanks!

**Editing & Navigation**
* [Quick Navigate](https://github.com/jeffslofish/quick-navigate): Navigate back to previous cursor and edit locations quickly with toolbar buttons.
* [Toolbar Horizontal](https://github.com/ruanmer/brackets-toolbar-horizontal): Move toolbar from vertical (right) to horizontal (top).
* [Image Viewer] (https://github.com/warabe/brackets.image-viewer): Adds image and dimensions viewing instead of the error modal (currently for OSX only).

**Snippets & Shorthand**
* [HTML Templates](https://github.com/talmand/Brackets-HTML-Templates): Pastes in barebones HTML code for different doctypes.
* [Auto-match pairs](https://github.com/zr0z/brackets-automatch-pairs): Auto-complete parenthesis, brackets, braces, double and single quotes.
* [Disable AutoClosing Tags](https://github.com/talmand/Brackets-Disable-AutoClose-Tags): Disables auto-insertion of closing HTML tag when typing the opening tag, but preserves auto-completion of the closing tag when `</` is typed.

**Formatting**
* [Auto Formatter](https://github.com/shumpei/brackets-formatter-extension): Auto formatter for XML/HTML, CSS, JavaScript files.
* [Auto Indent](https://github.com/shumpei/brackets-autoindent-extension): Indent automatically for whole file.
* [Remove Trailing Spaces](https://github.com/pockata/brackets-StripTrailingSpaces): Removes unnecessary whitespace when saving files.
* [Quote Converter](https://github.com/drewhjava/brackets-quoteconverter): Converts double to single quotes or single to double quotes

**Language Support**
* [PHP](https://github.com/aonic/brackets-QuickOpenPHP): Adds PHP function definition support to QuickOpen search
* [MVC.net](https://github.com/edwinvankoppen/Brackets-MVC.net): Adds cshtml (views in MVC.net) to the HTML highlighting.
* [Ruby](https://github.com/TheresNoBox/Brackets-Ruby): Adds support for Haml, ERB, and Ruby line and block comments. 

**General Functionality**
* [Tabs](https://github.com/albertxing/brackets-tabs): Show tabs in place of title when sidebar is hidden
* [Debug] (https://github.com/aghiura/brackets-console): See console.log & console.error messages directly in Brackets
* [Parent Dir] (https://github.com/katsh/brackets-parent-dir): Show parent directory of opened files in the Working Set.

**Live Development**
* [Debugger](https://github.com/jdiehl/brackets-debugger) *Incompatible with Brackets >= 0.20*: Brackets Debugger for the Live Development browser.
* [V8/Node Live Development](https://github.com/DennisKehrig/brackets-v8-node-live): Updates scripts running in Node.js as you type
* [Evaluate Clojure Expressions](https://github.com/yehohanan7/clj-brackets): Select any expression and evaluate it in wrepl

**Visual Editing**
* [CSS Exclusion Shape Viewer](https://github.com/adobe/brackets-plugin-exclusions): Quick Edit on an exclusion shape definition in CSS displays the shape.

**External Tools & Online Content**
* [BracketLESS] (https://github.com/olsgreen/BracketLESS): Compiles LESS files to CSS on save
* [Open File from URL](https://github.com/deemeetar/OpenFileFromUrl): Opens any ```href``` and ```rel``` attribute urls in editor on ```ALT+0``` shortcut. Currently works only with existing files. 
* [PhoneGap Extension for Brackets](https://github.com/adobe/brackets-phonegap): Manage PhoneGap Build projects from Brackets. 
* [Ant for Brackets](https://github.com/jbalsas/brackets-ant): Launch any target in your ant files. (Uses Node. Requires Brackets Sprint 21 or greater)
* [Grunt for Brackets](https://github.com/markrendle/brackets-grunt): Run any task in your Gruntfile.js. (Uses Node. Tested on Brackets Sprint 25.)
* [Project Links](https://github.com/chrismatheson/brackets-project-links): Add some links to the sidebar for quick access **dev** still a lot of stuff i want to add
* [JSCompressor](https://github.com/slorenzot/brackets-jscompressor): This extension allows automatically compress javascript and css files using YUI compressor.

**Source Control**
* [GitHub](https://github.com/jrowny/brackets-github): Implements the GitHub API, including oAuth. Currently functionality limited to Gists.
* [GitHubAccess](https://github.com/WebsiteDeveloper/GitHubAccess): Is able to completly clone a repo from GitHub into a selected Folder. (Works only for non-binary data)

**Documentation**
* [Docco](https://github.com/aghiura/brackets-docco): Runs [Docco](http://jashkenas.github.io/docco/) on a js file.
* [MDNLookup](https://github.com/pamelafox/brackets-MDNLookup-extension): Includes a way of creating an extensions toolbar and adding buttons to the toolbar with callbacks.
* [Bottom Browser](https://github.com/larz0/bottom-browser): Allows you to browse certain sites in the bottom panel and lets you do a Bing search on highlighted text by pressing Shift+Cmd+B.

**Linting & Warnings**
* [Continuous Compilation](https://github.com/JoachimK/brackets-continuous-compilation): Displays JSLint error messages inline, highlighting infringing code and checking the code while you type.
* [JSONLint](https://github.com/katsh/brackets-jsonlint) JSONLint your documents.
* [PHPCS](https://github.com/jeffslofish/brackets-PHPCS-client): PHP_CodeSniffer for brackets. Lints your PHP through a web service.

**Testing & Code Metrics**
* [complexityReport.js](https://github.com/sahlas/brackets-crjs): A Brackets extension that enables phil booth's complexityReport.js tool. Displays complexity statistics on a per-function and aggregate basis. 
* [Jasmine](https://github.com/dschaffe/brackets-jasmine): Runs the Jasmine-node unit test tool against the
current file.
* [MaDGe](https://github.com/sahlas/brackets-node-madge): An extension that enables node-madge functionality. Search for module dependencies, circular dependencies and more.

**Fun & Games**
* [Snake](https://github.com/AboutWebLLC/brackets-snake): Because sometimes you need to eat your code.

---

## Moved Extensions

These extensions are now available via the [Brackets Registry](https://brackets-registry.aboutweb.com), which is definitely the easiest way to get the extensions and keep them up-to-date.

* [Reload in Browser](https://github.com/DennisKehrig/brackets.ReloadInBrowser): Adds a toolbar button and shortcut to reload the page in the browser
* [Show Whitespace](https://github.com/DennisKehrig/brackets-show-whitespace): Visualizes spaces and tabs
* [Minifier](https://github.com/alfredxing/brackets-minifier): Minifies JavaScript and CSS files using Google Closure Compiler (JS) and YUI (CSS). Working but under development.
* [Quick Search] (https://github.com/enturn/brackets-quick-search): Automatically highlights occurrences of the selected word (like Notepad++ smart highlighting)
* [Cowsay](https://github.com/lkcampbell/brackets-cowsay): An extension for Brackets to generate a cow saying very profound and silly things.
* [Fortune](https://github.com/lkcampbell/brackets-fortune): An extension for Brackets to insert a random fortune into the editor.
* [Special HTML Characters] (https://github.com/thaneuk/brackets-special-html-chars): Addition to the context menu to insert from a special HTML character list.
* [Karma](https://github.com/artoale/karma-brackets): Run mocha, jasmine, qunit and more tests inside brackets
* [TestQuickly](https://github.com/dangoor/TestQuickly): Simple extension to run unit tests with a keystroke (handy for Brackets core and extension developers)
* [xUnit](https://github.com/dschaffe/brackets-xunit): Extension to run current buffer as a javascript unit test with right click (supports jasmine, yui3, qUnit, more coming soon...)
* [CSSLint](https://github.com/cfjedimaster/brackets-csslint): CSSLint your documents.
* [JSHint](https://github.com/cfjedimaster/brackets-jshint): Performs a JSHint report.
* [W3CValidator](https://github.com/cfjedimaster/brackets-w3cvalidation): Run the W3C Validator on your HTML.
* [Interactive Linter](https://github.com/MiguelCastillo/Brackets-InteractiveLinter): Get JSHint/JSLint/CoffeeLint feedback on your source as you work on it.
* [CanIUse](https://github.com/cfjedimaster/brackets-caniuse): An inline viewer of CanIUse.com support data.
* [Display Shortcuts](https://github.com/redmunds/brackets-display-shortcuts): Display Shortcuts defined to Brackets.
* [DevDocs Viewer](https://github.com/gruehle/dev-docs-viewer) (use [this link](https://github.com/gruehle/dev-docs-viewer/archive/sprint-27.zip) for Brackets Sprint 27 or earlier): Search and view content from [devdocs.io](http://devdocs.io).
* [brackets-perforce] (https://github.com/JoshGalvin/brackets-perforce): Integration of Perforce (no GUI).
* [brackets-git](https://github.com/zaggino/brackets-git): Integration of Git features into Brackets UI. Features in development, see link for details.
* [git-project-info] (https://github.com/couzteau/brackets-git-info): A minimalistic approach to git integration: Just show the git branch next to the project. That's it.
* [Tomcat Manager](https://github.com/MiguelCastillo/Brackets-TomcatManager): Tomcat server integration with Brackets
* [ImageToData](https://github.com/FloValence/brackets-ImageToData): Converts any image, local or on the web into data URI with its URL.
* [PageSuck](https://github.com/timburgess/brackets-pagesuck): Prompts for a URL and pulls that page's HTML directly into the editor as a new file
* [ToGist](https://github.com/davidderaedt/togist): Create an anonymous gist from the current selection.
* [SVG Preview](https://github.com/peterflynn/svg-preview): Live preview SVG files in an inline panel while you edit them.
* [Markdown Preview](https://github.com/gruehle/MarkdownPreview/archive/sprint-27.zip) (use [this link](https://github.com/gruehle/MarkdownPreview/archive/sprint-27.zip) for Brackets Sprint 27 or earlier): Live preview of Markdown files, updated as the document is edited.
* [Edge Web Fonts](https://github.com/adobe/brackets-edge-web-fonts/): Browse free fonts from the Edge Web Fonts collection, with thumbnails. Activated via CSS code hints for font-family.
* [Theseus](https://github.com/adobe-research/theseus): Retroactive debugging for JavaScript in Chrome and Node.js
* [Everyscrub](https://github.com/peterflynn/everyscrub): Everything's a scrubber! Cmd/Ctrl + drag on any number or hex color to scrub its value and update the browser in real time.
* [WD Minimap] (https://github.com/websiteduck/brackets-wdminimap): The minimap shows a smaller version of your code at the right of the screen.  It can be used to quickly scroll to a certain part of your file.
* [bracketsFileWatcher] (https://github.com/WebsiteDeveloper/bracketsFileWatcher/releases/download/0.0.1/bracketsFileWatcher-0.0.1.zip): Watches the current project file tree for changes and refreshes the file tree in brackets if necessary. [Source Code] (https://github.com/WebsiteDeveloper/bracketsFileWatcher)
* [Key Remapper] (https://bitbucket.org/sacah/brackets-key-remapper/overview) : Shows current keyboard shortcuts and allows you to set new ones. These are stored in settings, so should persist across Brackets installations.
* [Themes for Brackets](https://github.com/Jacse/themes-for-brackets): Customize your Brackets experience with themes. Make Brackets easy on your eyes, and improve the coding experience.
* [Themes] (https://github.com/MiguelCastillo/Brackets-Themes): CodeMirror themes for brackets from the main menu.
* [Code folding] (https://github.com/thehogfather/brackets-code-folding): Enables code folding in brackets.
* [File Navigation Shortcuts](https://github.com/peterflynn/brackets-editor-nav): Shortcuts for switching to next/previous editor (<em>non</em>-MRU order), and a version of Quick Open scoped to only open files.
* [Brackets Commands Guide](https://github.com/peterflynn/brackets-commands-guide): Search and execute commands by typing part of their name, similar to Quicksilver (or Sublime's Ctrl+Shift+P or Eclipse's Ctrl+3).
* [Extension Toolkit](https://github.com/davidderaedt/Brackets-extension-toolkit): An extension to make building Brackets extensions easier.
* [Gherkin](https://github.com/tregusti/brackets-gherkin): Adds syntax highlighting for Gherkin (.feature) files.
* [Jade Templating](https://github.com/dbratcher/brackets-jade): Adds syntax highlighting for .jade files.
* [Simple JS code hints](https://github.com/iwehrman/brackets-simple-js-code-hints): Like the Tern-based JS hinter, but stupid!
* [Tern JS hints](https://github.com/MiguelCastillo/Brackets-Tern): Use tern capabilities in Brackets.
* [TypeScript Code Intel](https://github.com/tomsdev/brackets-typescript-code-intel): Adds TypeScript support in Brackets (Auto-completion, Quick Edit and more soon).
* [Creative Cloud Extension Builder](http://davidderaedt.github.io/CC-Extension-Builder-for-Brackets/): Create HTML based extensions for Adobe tools like Photoshop and Illustrator.
* [App Cache Buddy](https://github.com/davidderaedt/appcache-gen): Generate and validate application cache manifests.
* [Annotate](https://github.com/davidderaedt/annotate-extension): Generates JSDoc annotations for your functions.
* [Copy as HTML](https://github.com/peterflynn/copy-as-html): Copy code to the clipboard with full color syntax highlighting (Edit > Copy as Colored HTML).
* [Beautify](https://github.com/drewhjava/brackets-beautify): Beautify(Tidy, Format) HTML, CSS, and Javascript (uses js-beautify)
* [Reasonable Comments](https://github.com/peterflynn/reasonable-comments): More reasonable behavior when pressing Enter inside a JS block comment - the "*" with proper indentation is inserted for you.
* [CharLimit](https://github.com/cezarywojcik/cezarywojcik.charlimit): Draws a line at character limit to help formatting; includes settings screen to choose character limit, line color, and line toggle.
* [Cleaner](https://github.com/cezarywojcik/cezarywojcik.cleaner): Cleans code to ensure that there is a newline at the end of a file, removes trailing whitespace, and converts tabs to spaces if the user is using spaces.
* [TabToSpace](https://github.com/davidderaedt/tabtospace-extension): Converts indentation to tabs or spaces
* [String Convert](https://github.com/mikechambers/StringConvert): Provides shortcuts for modifying and encoding strings within the editor.
* [Edit History](https://github.com/iwehrman/brackets-edit-history): Yet another extension for navigating forward and backward among edit positions.
* [Toolbar Toggle](https://github.com/ruanmer/brackets-toolbar-toggle): Toggle toolbar view based on sidebar view.
* [Document Outline](https://github.com/soimon/brackets-document-outliner): Shows the document outline of the currently selected HTML5 document.
* [Select Parent](https://github.com/njx/select-parent): Quick way to select the block enclosing the selection
* [Kill Ring](https://github.com/iwehrman/brackets-kill-ring): Adds an Emacs-style kill ring to the editor 
* [Bookmarks](https://github.com/toshsharma/brackets-bookmarks): Navigate within a document using bookmarks.
* [Spell Checker] (https://github.com/couzteau/SpellCheck): Integrates the spell checker web service <i>After The Deadline</i> - now in beta - Supports English, German, French, Spanish and Portuguese. _Note: Now compatible with brackets build >== build 0.18.x /Sprint 18_ 
* [Indent Guides] (https://github.com/lkcampbell/brackets-indent-guides): An extension for Brackets to show indent guides in the code editor.
* [Go to Last Edit](https://github.com/peterflynn/goto-last-edit): Jump to the location of your last edit in the current file with a quick keyboard shortcut (Ctrl+*)
* [Close Others](https://github.com/sathyamoorthi/brackets-close-others): This will give you additional options like Close others, Close top and bottom files to operate 'Working Files' section efficiently.
* [Sidebar Plus](https://github.com/sathyamoorthi/brackets-sidebar-plus): Show / Hide sidebar using mouse over.
* [Ruler] (https://github.com/lkcampbell/brackets-ruler): A column ruler for Brackets
* [Delete to Start/End of line](https://github.com/probertson/brackets-delete-line-start-end): Shortcuts to delete from the current cursor position to the start or end of the line (common shortcuts Cmd+Backspace and Cmd+Delete on Mac)
* [Various Improvements] (https://github.com/Dammmien/brackets-various-improvements): Add some small improvements ( close all folders in file tree, case converter, additional informations in status bar, super clipboard)
* [Todo] (https://github.com/mikaeljorhult/brackets-todo): Find and displays all todo comments in current document.
* [Unused Files] (https://github.com/Dammmien/brackets-unused-files): Find unused files in your web project.
* [Emmet/Zen Coding](https://github.com/emmetio/brackets-emmet): Adds Emmet (Zen Coding) support to Brackets.
* [Snippets](https://github.com/jrowny/brackets-snippets): Assign trigger keys to insert snippets. Configurable with JSON
* [Prefixr](https://github.com/davidderaedt/prefixr-extension): Generate browser specific CSS prefixes using the prefixr service.
* [Quick Markup](https://github.com/redmunds/brackets-quick-markup): Fast HTML markup generation as you type.
* [Lorem Ipsum] (https://github.com/lkcampbell/brackets-lorem-ipsum): Generates Lorem Ipsum text and inserts it into any Brackets document.
* [Quick Require Import](https://github.com/peterflynn/quick-import): Quickly generate RequireJS/CommonJS import statements - just press Ctrl+I and type a module name.
* [Whitespace Normalizer](https://github.com/dsbonev/whitespace-normalizer): Trims trailing whitespaces, transforms tabs to spaces and adds newline at the last line when saving files.
* [Vimderbar] (https://github.com/fontface/brackets-vimderbar): Adds vim-like functionality & status bar to Brackets.
* [Surround] (https://github.com/pedelman/brackets-surround): Wraps selected text with HTML, comments, brackets, quotes, etc.
* [Related Files](https://github.com/jhatwich/brackets-related-files): Discovers and allows you to open related files in your project.
* [Rename JavaScript Identifier](https://github.com/asgerf/bracket-rename): Renames JavaScript identifiers.
* [git-integration](https://github.com/pollensoft/git-integration): Git Command line integration

---

**Deprecated Extensions**
* [Brace Helper] (https://github.com/Zolmeister/brackets-helper): Insert closing `}` on Enter
* [Open Containing Folder](https://github.com/ujjaval/Open-containing-folder): Opens folder containing current file or a file/folder in Sidebar. Added keyboard shortcut ```CTRL+ALT+O``` for opening folder containing document opened in editor. _(Brackets Sprint 25 includes an enhanced version of this functionality: when invoked on a file, the file is automatically selected inside its containing folder)._
* [Hover Preview](https://github.com/gruehle/HoverPreview): Displays a preview when hovering over a color value, gradient, or image filename. _(Merged into Brackets as of Sprint 24)._
* [JavaScript Code Hints](https://github.com/iwehrman/brackets-js-code-hints): Code hinting (aka autocompletion) for JavaScript files in Brackets. _(Merged into Brackets as of Sprint 21)._
* [Color Editor](https://github.com/GarthDB/brackets-inline-color-editor): Quick Edit on a hex/rgb/hsl color opens an inline color picker, plus a listing of all colors used in the file. _(Merged into Brackets as of Sprint 17)._
* [Color Picker](https://github.com/jdiehl/brackets-color-picker): Quick Edit on a hex color opens an inline color picker. _(A color picker is built into Brackets as of Sprint 17)._
* [Editor Shortcuts](https://github.com/aonic/editor-shortcuts): Keyboard shortcut to delete line. _(This command is built into Brackets as of Sprint 15)._
* [HTML Tag Highlight](https://github.com/sathyamoorthi/brackets-html-tag-highlight): Highlight start and end tags in HTML document. _(This is not working in sprint 28. Already there is a [pull request](https://github.com/adobe/brackets/pull/4504) for this functionality.)._
* [Web Fonts](https://github.com/talmand/Brackets-Web-Fonts): Simple interface for adding/deleting/swapping Google Web Fonts in a CSS file (README says that it's no longer being updated)
* [Extension Manager](https://github.com/jdiehl/brackets-extension-manager): Install, Remove, and upgrade your extensions from the cloud from inside Brackets (requires node). _(Superseded by the built-in Extension Manager.)_