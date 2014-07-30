_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 0.42 -- approximately July 31._

What's New in Release 0.42
--------------------------
* **Themes**
    * **[Editor color schemes](https://trello.com/c/LHhAcbcU/1260-c-editor-themes):** Find themes in Extension Manager, and instantly switch between them using View > Themes.
    * [Built-in dark mode](https://github.com/adobe/brackets/pull/8462): Choose the built-in "Brackets Dark" theme for easy access to a nighttime color palette. Third-party themes can also trigger "dark mode," darkening many parts of the Brackets automatically without needing to style everything manually.
    * Configure font settings: Use View > Themes to set a custom font and font size (the font must be installed in your OS).
* **File Types**
    * [Switch language/syntax mode of a single file](https://github.com/adobe/brackets/pull/6409): Use the language indicator in the status bar as a dropdown to override the language Brackets chose to treat a file as. (These changes are _not_ saved when you close the file; use [preferences](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#preferences) to permanently treat all files with a certain extension as a different language).
* **Replace in Files**
    * [Check/uncheck all matches for a file](https://github.com/adobe/brackets/pull/8260): The headings for each file include a checkbox for quickly including or excluding _all_ matches in the file.
* **Extension Management**
    * [Show supported translations in Extension Manager](https://github.com/adobe/brackets/pull/7995): Extension listings indicate which languages each extension has been translated for (at bottom).
    * [Install extension from local .zip file](https://github.com/adobe/brackets/pull/8166): Drag a .zip file onto the drop zone in Extension Manager's lower-left corner to install.
* **Localization**
    * [Traditional Chinese translation added](https://github.com/adobe/brackets/pull/8332)
    * [Galician translation added](https://github.com/adobe/brackets/pull/8226)
* **Preferences**
    * [New Preference: Hide hints from each provider](https://github.com/adobe/brackets/pull/8272): For example, setting `"codehint.UrlCodeHints": false` will hide the hints that appear when you're typing `<img src="...`. [Read more...](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#preferences)
* **Ongoing Research** (not implemented yet)
    * [Research: CSS Live Preview new implementation](https://trello.com/c/JWRhHzI6/1356-c-livedev-initial-css-implementation)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-0.41...release-0.42#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-41...release-0.42#commits_bucket)


UI Changes
----------


API Changes
-----------

New/Improved Extensibility APIs
-------------------------------


Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
TBD

#### Pulling source code from Git
* TODO: new brackets-shell build required?
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 0.42
--------------------------
For details on the bugs addressed, please refer to [closed sprint 42 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=30&state=closed). Not all fixed bugs will be caught by this search query, however.