_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 21 -- approximately March 1._

What's New in Sprint 21
-----------------------
* **Code Hinting**
    * [JavaScript code hinting](https://trello.com/card/2-code-hinting-javascript/4f90a6d98f77505d7940ce88/775): Smart code hinting includes keywords, local variables, arguments, and property names based on nearby code.
* **Live Development**
    * [Default localhost for Live Preview](https://trello.com/card/5-live-development-on-localhost/4f90a6d98f77505d7940ce88/684): By default, Live Preview now launches an http://localhost URL instead of file:// thanks to a built-in Node.js server. Pointing Live Preview at your _own_ local server remains supported (see File > Project Settings).
* **Overall UI**
    * [Drag & drop to open files](https://github.com/adobe/brackets-shell/pull/190): Drag files onto the Dock icon (on Mac) or the Brackets window itself (on Windows) to open them.
    * [Remember cursor & scroll position across launches](https://github.com/adobe/brackets/pull/2898): Files you leave open when you quit or switch projects will be reopened right where you left off. Within a session, this also works for files you open, close, then reopen later.
* **Extensions**
    * [Enables language extensions](https://trello.com/card/2-support-for-language-extensions/4f90a6d98f77505d7940ce88/773): New file types can be added by extensions, with support for syntax highlighting, block/line comments, and more.


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-20...sprint-21#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-20...sprint-21#commits_bucket)


UI Changes
----------
No major changes to existing features.


API Changes
-----------
* **Editor constructor** - The ``mode`` argument has been removed, shifting other arguments over (mode is now retrieved from the Document). Note: it would be very unusual for an extension to be calling the Editor constructor directly.
* **Debug menu** - Removed ``Menus.AppMenuBar.DEBUG_MENU`` menu ID
* **EditorUtils removed** - ``EditorUtils.getModeFromFileExtension()`` changed to ``LanguageManager.getLanguageForFileExtension()``. However, when dealing with open documents, extensions should not use this API. Instead, to detect the language for a given ``Document``, use ``document.getLanguage()`` or for a given ``Editor`` selection, use ``editor.getLanguageForSelection()``.

New/Improved Extensibility APIs
-------------------------------

* **LanguageManager and Language** - ``LanguageManager`` allows extensions to define new language support in Brackets without making modifications to core Brackets code. In this first iteration, a ``Language`` can be defined declaratively in JSON as a set of properties including a CodeMirror syntax highlighting mode, file extensions to map to the language, and finally line and block commenting syntax. Optionally, extensions may define custom CodeMirror modes. For an [overview of language support](https://github.com/adobe/brackets/wiki/Language-Support) and our future direction please see the [wiki](https://github.com/adobe/brackets/wiki/Language-Support).
* **Node Process** - [Brackets Node Process: Overview for Developers](https://github.com/adobe/brackets/wiki/Brackets-Node-Process:-Overview-for-Developers)

Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.


Community contributions to Brackets
-----------------------------------
* [Add SASS Support](https://github.com/adobe/brackets/pull/2609) by [Bryan Stedman](https://github.com/bryanstedman)
* [Update Getting started screenshot, styling for 'es' locale](https://github.com/adobe/brackets/pull/2801) by [J.M](https://github.com/mynetx)
* [Fixed: Issue #2551 Rename folder does not update jsTree completely](https://github.com/adobe/brackets/pull/2862) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Spanish strings for sprint 20](https://github.com/adobe/brackets/pull/2871) by [Chema Balsas](https://github.com/jbalsas)
* [Fix: Move Line Up/Down collapses inline editor when moving past the start/end](https://github.com/adobe/brackets/pull/2431) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Live development bugfixes](https://github.com/adobe/brackets/pull/2819) by [Jonathan Diehl](https://github.com/jdiehl)
* [Add check for broken symlinks to hacking scripts (#2895)](https://github.com/adobe/brackets/pull/2896) by [Chema Balsas](https://github.com/jbalsas)
* [Fix Issue #2875: CSS code hints overwrite 1 char too many on current line](https://github.com/adobe/brackets/pull/2884) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix Issue #2877: Debug menu shortcuts are enabled even when Debug menu is hidden](https://github.com/adobe/brackets/pull/2888) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Disable the cache before every reload](https://github.com/adobe/brackets/pull/) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Trigger scroll on project files container to reposition scroller shadows (#2255)](https://github.com/adobe/brackets/pull/2905) by [Chema Balsas](https://github.com/jbalsas)
* [Update Getting started screenshot for 'de' locale, transparent background](https://github.com/adobe/brackets/pull/2840) by [J.M](https://github.com/mynetx)
* [Fix: Move Line Up/Down collapses inline editor when moving past the start/end](https://github.com/adobe/brackets/pull/2431) by [Tomás Malbrán](https://github.com/TomMalbran)
* [LESS Refactoring](https://github.com/adobe/brackets/pull/2844) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Adding language, extensions and comments support](https://github.com/adobe/brackets/pull/2971) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Continued Language API work](https://github.com/adobe/brackets/pull/2979) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Move the Update List information to a template](https://github.com/adobe/brackets/pull/2938) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #2902: Externalize debug menu as an extension](https://github.com/adobe/brackets/pull/2942) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Prevent prefix based conflicts when renaming](https://github.com/adobe/brackets/pull/2914) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Spanish Strings for Sprint 21](https://github.com/adobe/brackets/pull/2994) by [Chema Balsas](https://github.com/jbalsas)
* [Extracted default menus into extra file](https://github.com/adobe/brackets/pull/2940) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix #1399: Make the update dialog text selectable](https://github.com/adobe/brackets/pull/2990) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Contributors List on the About Dialog](https://github.com/adobe/brackets/pull/2934) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix About Dialog Contributors (Issue 3012 from Pull 2934)](https://github.com/adobe/brackets/pull/3014) by [Tucker Whitehouse](https://github.com/TuckerWhitehouse)

Contributions _from_ Brackets
-----------------------------
**Contributions to [CodeMirror](https://github.com/marijnh/CodeMirror):**
* [Avoid scrolling to cursor position in a non-visible editor](https://github.com/marijnh/CodeMirror/pull/1256)

Bugs fixed in Sprint 21
-----------------------
For details on the bugs addressed, please refer to [closed sprint 21 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=8&state=closed). A few of the fixed bugs might not be caught by this search query, however.