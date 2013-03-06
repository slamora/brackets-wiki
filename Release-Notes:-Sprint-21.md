What's New in Sprint 21
-----------------------
* **Code Hinting**
    * [JavaScript code hinting](https://trello.com/card/2-code-hinting-javascript/4f90a6d98f77505d7940ce88/775): Smart code hinting includes keywords, local variables, arguments, and property names based on nearby code.
    * _Note: If you have been using the earlier "brackets-js-code-hints" extension, you **must** uninstall it -- it cannot coexist with the newer version built into Brackets._
* **Live Development**
    * [Default localhost for Live Preview using Node](https://trello.com/card/5-live-development-on-localhost/4f90a6d98f77505d7940ce88/684): By default, Live Preview now launches an http://localhost URL instead of file:// thanks to a built-in Node.js server. Pointing Live Preview at your _own_ local server remains supported (see File > Project Settings).
* **Overall UI**
    * [Drag & drop to open files](https://github.com/adobe/brackets-shell/pull/190): Drag files onto the Dock icon (on Mac) or the Brackets window itself (on Windows) to open them.
    * [Remember cursor & scroll position across launches](https://github.com/adobe/brackets/pull/2898): Files you leave open when you quit or switch projects will be reopened right where you left off. Within a session, this also works for files you close and reopen later.
* **Extensions**
    * [Enables language extensions](https://trello.com/card/2-support-for-language-extensions/4f90a6d98f77505d7940ce88/773): New file types can be added by extensions, with support for syntax highlighting, block/line comments, and more.
    * Extension developers no longer need to disable the cache using Developer Tools -- reloading Brackets should always show changes made to extensions.
* **Code Editing**
    * SASS code coloring
    * Toggle Line/Block Comment support for PHP, XML, C/C++/C#

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-20...sprint-21#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-20...sprint-21#commits_bucket)


UI Changes
----------
No major changes to existing features.


API Changes
-----------
**EditorUtils removed** - ``EditorUtils.getModeFromFileExtension()`` is replaced by ``LanguageManager.getLanguageForFileExtension()``. However, it's usually simpler to use the other new APIs introduced this sprint instead: ``document.getLanguage()`` or ``editor.getLanguageForSelection()``.

**Editor constructor** - The ``mode`` argument has been removed, shifting other arguments over (mode is now determined by the Document). Note: you should not normally need to call the Editor constructor directly.

**Debug menu** - Removed ``Menus.AppMenuBar.DEBUG_MENU`` menu ID. This menu is now defined by an extension and may not always be present.

New/Improved Extensibility APIs
-------------------------------
**LanguageManager and Language** - ``LanguageManager`` allows extensions to define new language support in Brackets without making modifications to core Brackets code. In this first iteration, a ``Language`` can be defined declaratively in JSON as a set of properties including a CodeMirror syntax highlighting mode, file extensions to map to the language, and finally line and block commenting syntax. Optionally, extensions may define custom CodeMirror modes. For an [overview of language support](https://github.com/adobe/brackets/wiki/Language-Support) and our future direction please see the [wiki](https://github.com/adobe/brackets/wiki/Language-Support).

**Node Server** - See the [Overview for Developers](https://github.com/adobe/brackets/wiki/Brackets-Node-Process:-Overview-for-Developers). These are _preliminary_ APIs that will probably change next sprint.

Known Issues
------------
* JavaScript Code Hinting will not work properly if the earlier standalone brackets-js-code-hints extension is installed.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.


Community contributions to Brackets
-----------------------------------
The Brackets team welcomes five **new core committers**: [Jonathan Diehl](https://github.com/jdiehl), [Jon Rowny](https://github.com/jrowny), [Dennis Kehrig](https://github.com/DennisKehrig), [Chema Balsas](https://github.com/jbalsas), and [Tomás Malbrán](https://github.com/TomMalbran). ([Read more...](http://blog.brackets.io/2013/03/05/welcome-new-brackets-contributors))


* [Language extensibility APIs & refactor LESS support as a default extension](https://github.com/adobe/brackets/pull/2844) ([and cleanups](https://github.com/adobe/brackets/pull/2979)) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Enable SASS code coloring](https://github.com/adobe/brackets/pull/2609) by [Bryan Stedman](https://github.com/bryanstedman)
* [Toggle Comment support for PHP & C/C++/C#; recognize .jsx as JS; bug fixes](https://github.com/adobe/brackets/pull/2971) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Show contributors list in About dialog](https://github.com/adobe/brackets/pull/2934) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #1551: Reloading Brackets always reflects changes made to extensions](https://github.com/adobe/brackets/pull/) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Fix #1933: Move Line Up/Down in an inline editor would sometimes cause it to close](https://github.com/adobe/brackets/pull/2431) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #2875: CSS code hints overwrite 1 char too many in one-line rules](https://github.com/adobe/brackets/pull/2884) by [Tomás Malbrán](https://github.com/TomMalbran)
* [About dialog contributors list bug fixes & cleanups](https://github.com/adobe/brackets/pull/3014) by [Tucker Whitehouse](https://github.com/TuckerWhitehouse)
* [Fix bugs when renaming a file/folder whose name is a prefix of some other item](https://github.com/adobe/brackets/pull/2914) by [Dennis Kehrig](https://github.com/DennisKehrig)
* [Live development bug fixes](https://github.com/adobe/brackets/pull/2819) by [Jonathan Diehl](https://github.com/jdiehl)
* [Fix #2895: "Setup for hacking"/restore scripts didn't work if existing symlink pointed to non-existent folder](https://github.com/adobe/brackets/pull/2896) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #2877: Debug menu shortcuts are enabled even when Debug menu is hidden](https://github.com/adobe/brackets/pull/2888) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #1399: Make Update dialog's text selectable](https://github.com/adobe/brackets/pull/2990) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #2255: Recent Projects header flickers on rollover if working set was empty on launch](https://github.com/adobe/brackets/pull/2905) by [Chema Balsas](https://github.com/jbalsas)
* [Spanish translation updates](https://github.com/adobe/brackets/pull/2994) ([and](https://github.com/adobe/brackets/pull/2871)) by [Chema Balsas](https://github.com/jbalsas)
* [Spanish "Getting Started" updates](https://github.com/adobe/brackets/pull/2801) by [J.M](https://github.com/mynetx)
* [German "Getting Started" updates](https://github.com/adobe/brackets/pull/2840) by [J.M](https://github.com/mynetx)
* [Cleanup: move Update dialog UI into a template](https://github.com/adobe/brackets/pull/2938) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Cleanup: move Debug menu into a default extension](https://github.com/adobe/brackets/pull/2942) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Cleanup: move default menus into separate file from menu APIs](https://github.com/adobe/brackets/pull/2940) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)

Contributions _from_ Brackets
-----------------------------
**Contributions to [CodeMirror](https://github.com/marijnh/CodeMirror):**
* [Avoid scrolling to cursor position in a non-visible editor](https://github.com/marijnh/CodeMirror/pull/1256)

Bugs fixed in Sprint 21
-----------------------
For details on the bugs addressed, please refer to [closed sprint 21 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=8&state=closed). A few of the fixed bugs might not be caught by this search query, however.