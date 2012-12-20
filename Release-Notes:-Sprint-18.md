_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 18 -- approximately December 20._

Note: this final sprint of the year was largely focused on architectural work preparing for future sprints, so there are fewer new features than usual.

What's New in Sprint 18
-----------------------
* **Code Hinting**
    * [Code hint provider API improvements](https://trello.com/card/0-research-code-hint-provider-api/4f90a6d98f77505d7940ce88/723): APIs for all code hints [have been streamlined](https://github.com/adobe/brackets/wiki/New-Code-Hinting-API-Proposal) to support multiple hint providers per language more cleanly, remove bug-prone pitfalls, and do less processing per keystroke. More explanation [here](https://github.com/adobe/brackets/wiki/Code-Hinting-Functional-Specifications) and [here](https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/Luf7IN0-iAM).
    * [Improved CSS utilities](https://trello.com/card/2-css-utilities-api-to-support-code-completion/4f90a6d98f77505d7940ce88/713): A [new CSSUtils API](https://github.com/adobe/brackets/wiki/CSS-Context-API-implementation-spec) is available to support the [ongoing effort to add full CSS code hinting](https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/sAdPhV3ffOY) in a future sprint.
* **Code Editing**
    * [Progress on CodeMirror 3 migration](https://trello.com/card/2-codemirror-3-inline-editor-open-edit/4f90a6d98f77505d7940ce88/650): Inline editors are now mostly working on the [cmv3 branch](https://github.com/adobe/brackets/compare/master...cmv3), with one major exception: they are not always the correct height.
* **Extensions**
    * [Research language extensibility](https://trello.com/card/0-research-language-extensibility/4f90a6d98f77505d7940ce88/712): Planning for for a [future feature](https://trello.com/card/api-for-extensions-to-add-new-language-syntax-coloring-mode/4f90a6d98f77505d7940ce88/639) to allow extensions to add support for new file types. As a guinea pig, we're working on [moving LESS support from core out into an extension](https://trello.com/card/0-less-refactor-into-extension/4f90a6d98f77505d7940ce88/711).

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-17...sprint-18#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-17...sprint-18#commits_bucket)


UI Changes
----------


API Changes
-----------

**NativeFileSystem**
- **requestNativeFileSystem successCallback** - https://github.com/adobe/brackets/pull/2158
The successCallback now returns a FileSystem object with a root:DirectoryEntry property. Previously the callback returned the root:DirectoryEntry itself.
- **Replace FileError with NativeFileError** (implements DOMError) - https://github.com/adobe/brackets/pull/2318
The latest w3 spec passes specifies that error callbacks pass a DOMError object instead of FileError. FileError.code is replaced with DOMError.name. The deprecated enumeration for FileError types has been replaced with an enumeration in NativeFileError (module: file/NativeFileError). This pull request also fixes some consistency issues with error callbacks
- **Check examples and documentation** - https://github.com/adobe/brackets/pull/2063

**CodeHintManager**
- **registerHintProvider requires mode names and priority** - When registering with the CodeHintManager (CHM), CodeHintProviders now must also specify a list of CodeMirror mode names (e.g., "html" or "css") for which they are capable of providing hints, and also an integer priority that is used to decide the order in which the CHM polls providers for a particular mode. 
- **CodeHintProvider interface changes** - The interface that CHPs are required to implement has been simplified. There are now three required methods: 
 1. ``hasHints``, which is used to indicate whether or not a provider is willing to provide hints in a particular editor context, and begins a new hinting session; 
 2. ``getHints``, which returns either a list of hints for the editor context associated with the current hinting session, or a deferred object that will eventually resolve to a list of hints, as well as indication of whether the first hint should be selected by default; and 
 3. ``insertHint``, which inserts a hint into the editor context associated with the current session, also indicates whether the CHM should subsequently open another hinting session.
- Complete API details are available here: https://github.com/adobe/brackets/wiki/New-Code-Hinting-API-Proposal

New/Improved Extensibility APIs
-------------------------------

[TBD - ExtensionUtils]

Known Issues
------------
* [#1551](https://github.com/adobe/brackets/issues/1551): Changes within an extension (or a unit test) are not reflected by a simple "Debug > Reload Brackets." Workarounds:
    * Quit and re-launch Brackets to pick up the changes.
    * Open Developer Tools, click the gear icon in the lower-right, and select "Disable cache." This setting is remembered, but is only in effect so long as the Developer Tools browser tab remains open.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* _Debug > Show Developer Tools_ opens in a new tab in Chrome, rather than a new window in Brackets.
* [#1283](https://github.com/adobe/brackets/issues/1283): Text selection highlight sometimes jiggles when horizontally resizing window.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run since it was downloaded from the Internet (you may not see _any_ error or warning message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.


Community contributions to Brackets
-----------------------------------
* [Spanish translation for sprint18 strings](https://github.com/adobe/brackets/issues/2390) by [Chema Balsas](https://github.com/jbalsas)
* [Add pyc files to a list of ignored extensions](https://github.com/adobe/brackets/issues/2366) by ["justinabrahms"](https://github.com/justinabrahms)
* [Rename on Second Click](https://github.com/adobe/brackets/issues/2356) by ["gabriellcardoso"](https://github.com/gabriellcardoso)
* [Update 'de' l10n](https://github.com/adobe/brackets/issues/2367) by ["J.M."](https://github.com/mynetx)
* [Russian sample](https://github.com/adobe/brackets/issues/2320) by ["noway421"](https://github.com/noway421)
* [Adding python syntax highlighting](https://github.com/adobe/brackets/issues/2355) by ["justinabrahms"](https://github.com/justinabrahms)
* [Fixing several Block and Line Comment issues](https://github.com/adobe/brackets/issues/2342) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Update 'de' locale](https://github.com/adobe/brackets/issues/2224) by ["J.M."](https://github.com/mynetx)
* [NativeFileSystem documentation and examples](https://github.com/adobe/brackets/issues/2063) by [Chema Balsas](https://github.com/jbalsas)
* [Add CONTRIBUTING file](https://github.com/adobe/brackets/issues/2304) by ["J.M."](https://github.com/mynetx)
* [NativeFileError for error callbacks in NativeFileSystem](https://github.com/adobe/brackets/issues/2318) by [Chema Balsas](https://github.com/jbalsas)
* [Adding tooltip to JSLint star on start-up with JSLint disabled](https://github.com/adobe/brackets/issues/2280) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Color Editor CSS cleanups](https://github.com/adobe/brackets/issues/2225) by ["J.M."](https://github.com/mynetx)
* [Go to the most recently visited file when closing a file](https://github.com/adobe/brackets/issues/2122) by [Tomás Malbrán](https://github.com/TomMalbran)
* [HTML attributes code hinting filter](https://github.com/adobe/brackets/issues/2263) by [Chema Balsas](https://github.com/jbalsas)
* [FileSystem Implementation](https://github.com/adobe/brackets/issues/2158) by [Chema Balsas](https://github.com/jbalsas)
* [Fix: If first line in selection is a line comment, Block Comment does nothing](https://github.com/adobe/brackets/issues/2121) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Spanish translation Sprint17](https://github.com/adobe/brackets/issues/2273) by [Chema Balsas](https://github.com/jbalsas)
* [Use \u2026 for ellipsis in menu items](https://github.com/adobe/brackets/issues/2240) by ["J.M."](https://github.com/mynetx)
* [CSS and HTML Line Comment](https://github.com/adobe/brackets/issues/2133) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Create files on collapsed folders](https://github.com/adobe/brackets/issues/2198) by [Chema Balsas](https://github.com/jbalsas)
* [Use Windows key bindings on linux](https://github.com/adobe/brackets/issues/2331) and [#2000](https://github.com/adobe/brackets/pull/2000) by [Pritam Baral](https://github.com/pritambaral)
* [Russsian translation](https://github.com/adobe/brackets/pull/2268) by ["noway421"](https://github.com/noway421) and reviewed by ["TurboTurkey"](https://github.com/TurboTurkey) 
* [Change dialog label for Quick Open based on mode](https://github.com/adobe/brackets/pull/2298) and [#1706](https://github.com/adobe/brackets/pull/1706) by ["sagarsane"](https://github.com/sagarsane)

Contributions _from_ Brackets
-----------------------------

Bugs fixed in Sprint 18
-----------------------
For details on the bugs addressed, please refer to [closed sprint 18 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=5&state=closed). A few of the fixed bugs might not be caught by this search query, however.