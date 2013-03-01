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
* **EditorUtils removed** - ``EditorUtils.getModeFromFileExtension()`` changed to ``Languages.getLanguageForFileExtension()``
* **LanguageManager and Language** - ``LanguageManager`` allows extensions to define new language support in Brackets without making modifications to core Brackets code. In this first iteration, a ``Language`` can be defined declaratively in JSON as a set of properties including a CodeMirror syntax highlighting mode, file extensions to map to the language, and finally line and block commenting syntax. Optionally, extensions may define custom CodeMirror modes. For an [overview of language support](https://github.com/adobe/brackets/wiki/Language-Support) and our future direction please see the [wiki](https://github.com/adobe/brackets/wiki/Language-Support).

New/Improved Extensibility APIs
-------------------------------

* LanguageManager and Language - [See Language Support](https://github.com/adobe/brackets/wiki/Language-Support)

Known Issues
------------
* [#1551](https://github.com/adobe/brackets/issues/1551): Changes within an extension (or a unit test) are not reflected by a simple "Debug > Reload Brackets." Workarounds:
    * Quit and re-launch Brackets to pick up the changes.
    * Open Developer Tools, click the gear icon in the lower-right, and select "Disable cache." This setting is remembered, but is only in effect so long as the Developer Tools browser tab remains open.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.


Community contributions to Brackets
-----------------------------------

Contributions _from_ Brackets
-----------------------------

Bugs fixed in Sprint 21
-----------------------
For details on the bugs addressed, please refer to [closed sprint 21 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=8&state=closed). A few of the fixed bugs might not be caught by this search query, however.