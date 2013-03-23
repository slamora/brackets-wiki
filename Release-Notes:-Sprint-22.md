_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 22 -- approximately March 22._

What's New in Sprint 22
-----------------------
* **Extensions**
    * [Install extension from URL easily](https://trello.com/card/3-extension-installation-url/4f90a6d98f77505d7940ce88/789): Use File > Install Extensions... to install from a URL pointing to a ZIP file or GitHub repo.
* **Code Editing**
    * [Word wrap](https://trello.com/card/2-word-wrap/4f90a6d98f77505d7940ce88/270): Use View > Enable Word Wrap menu to toggle (enabled by default).
    * [Auto close braces](https://github.com/adobe/brackets/pull/3040): Use Edit > Auto Close Braces to enable automatically inserting closing `)` `]` `}` `"` `'` characters (disabled by default).
    * [Highlight active line](https://github.com/adobe/brackets/pull/3097): Use View > Show Active Line menu to highlight the line the cursor is currently on (disabled by default).
    * [Improved word-by-word navigation](https://github.com/adobe/brackets/issues/670): Moving the cursor with Ctrl+Left/Right arrows (Windows) or Alt+Left/Right arrows (Mac) now behaves more like other modern editors.
    * [Scroll up/down via keyboard](https://github.com/adobe/brackets/pull/3068): Use Ctrl+Up/Down arrows on Windows; Ctrl+Alt+Up/Down arrows on Mac to scroll the view one line at a time
    * [Hide line numbers](https://github.com/adobe/brackets/pull/3097): Use the View > Show Line Numbers menu to toggle (still visible by default).
    * Fixed several issues with scroll position jiggling or getting lost
* **Live Development**
    * [Improved reliability](https://trello.com/c/R0ZFrFV4): The connection to Chrome is more reliable and errors are displayed more clearly.
* **Localization**
    * [Simplified Chinese translation (zh-CN)](https://github.com/adobe/brackets/pull/3107)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-21...sprint-22#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-21...sprint-22#commits_bucket)


UI Changes
----------
**Find dialog** now shows the number of strings found.


API Changes
-----------
**Editor API updated** - `Editor.getIndentUnit()/Editor.setIndentUnit()` are replaced by `Editor.getSpaceUnits()/Editor.setSpaceUnits()`. This was done to fix indenting with mixed tab and space settings

New/Improved Extensibility APIs
-------------------------------
**LanguageManager** - 

* Programming languages can now also be associated with specific extensionless file names (e.g. "Makefile") via the new `fileNames` property on `LanguageManager.defineLanguage()`.
* Extensions can associate file names and file extensions with existing languages that are already defined, via `Language.addFileExtension()` and `.addFileName()`.
* A language can now declare multiple line comment styles (for example, Clojure supports both `;` and `;;`).

**Preferences storage keys** - `PreferencesManager.getPreferenceStorage()` can now accept a module object and automatically generate a storage id based on it. Use `PreferencesManager.handleClientIdChange()` to migrate preferences from an old storage key to a new one.

**LiveDevelopment** - `LiveDevelopment.connect()` was made public. This is called to reload agents on page update.

Known Issues
------------
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use an earlier build of Brackets (e.g. Sprint 20) after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Scroll up/down via keyboard](https://github.com/adobe/brackets/pull/3068) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Auto Close Braces option (disabled by default)](https://github.com/adobe/brackets/pull/3040) ([and](https://github.com/adobe/brackets/pull/3075)) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Support multiple line comment styles per language](https://github.com/adobe/brackets/pull/3121) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Assign language modes to extensionless filenames like 'Makefile'](https://github.com/adobe/brackets/pull/3029) by [Mark Murphy](https://github.com/MarkMurphy)
* [Remember font size between sessions](https://github.com/adobe/brackets/pull/3027) by [Lance Campbell](https://github.com/lkcampbell)
* [Simplified Chinese translation (zh-CN)](https://github.com/adobe/brackets/pull/3107) ([and](https://github.com/adobe/brackets/pull/3112)) by [yodfz](https://github.com/yodfz)
* [Fix #3100: Tab & Spaces settings can interact in an unexpected way](https://github.com/adobe/brackets/pull/3209) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #2878: Toggling folder tree node steals focus from editor](https://github.com/adobe/brackets/pull/2879) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix #1863: When no space before "{", Go to Definition doesn't select last char of selector](https://github.com/adobe/brackets/pull/3082) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix #3013: Permit uppercase letters in http:/https: in project settings](https://github.com/adobe/brackets/pull/3019) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix #2306: Hard to delete multiple Recent Projects items in succession](https://github.com/adobe/brackets/pull/2590) by [Chema Balsas](https://github.com/jbalsas)
* [Fix #3032: Restore console warnings for key binding collisions](https://github.com/adobe/brackets/pull/3081) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix #3030: Error thrown when disabled command's keyboard shortcut pressed](https://github.com/adobe/brackets/pull/3045) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix #2645: Folder tree "loading" message (rare) wasn't localized](https://github.com/adobe/brackets/pull/3020) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Expose LiveDevelopment.reconnect() API](https://github.com/adobe/brackets/pull/3214) by [Jon Rowny](https://github.com/jrowny)
* [Allow KeyBindingManager APIs to take a Command object](https://github.com/adobe/brackets/pull/3058) by [Lance Campbell](https://github.com/lkcampbell)
* [Clean up preference storage ids](https://github.com/adobe/brackets/pull/3018) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper) ([with help](https://github.com/adobe/brackets/pull/3101) from [Tomás Malbrán](https://github.com/TomMalbran))
* [Cleanup: Remove unneeded Editor -> EditorManager dependencies](https://github.com/adobe/brackets/pull/2750) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Cleanup: Consistently use Array./CollectionUtils.forEach() instead of $.each](https://github.com/adobe/brackets/pull/3087) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Spanish translation update](https://github.com/adobe/brackets/pull/3204) by [Chema Balsas](https://github.com/jbalsas)
* [Italian translation update](https://github.com/adobe/brackets/pull/2996) by [antonellopasella](https://github.com/antonellopasella)
* [German mistranslation fix](https://github.com/adobe/brackets/pull/3096) by [Markus Siemens](https://github.com/msiemens)

Contributions _from_ the Brackets team
--------------------------------------
**Contributions to [CodeMirror](https://github.com/marijnh/CodeMirror):**
* [Don't try to update scroll position while editor not visible](https://github.com/marijnh/CodeMirror/pull/1350)

Bugs fixed in Sprint 22
-----------------------
For details on the bugs addressed, please refer to [closed sprint 22 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=9&state=closed). A few of the fixed bugs might not be caught by this search query, however.