What's New in Release 0.42
--------------------------
* **Themes**
    * **[Editor color schemes](https://trello.com/c/LHhAcbcU/1260-c-editor-themes):** Find themes in Extension Manager, and instantly switch between them using View > Themes. [Learn how to create a theme.](https://github.com/adobe/brackets/wiki/Creating-Themes)
    * [Built-in dark mode](https://github.com/adobe/brackets/pull/8462): Choose the built-in "Brackets Dark" theme for easy access to a nighttime color palette. Third-party themes can also trigger "dark mode," dimming many parts of the Brackets automatically without needing to style everything manually.
    * Configure font settings: Use View > Themes to set a custom font and font size (the font must be installed in your OS).
* **File Types**
    * [Switch language/syntax mode of a single file](https://github.com/adobe/brackets/pull/6409): Use the language indicator in the status bar as a dropdown to override the language Brackets chose to treat a file as. (These changes are _not_ saved when you close the file; use [preferences](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#preferences) to permanently treat all files with a certain extension as a different language).
* **Replace in Files**
    * [Check/uncheck all matches for a file](https://github.com/adobe/brackets/pull/8260): The headings for each file include a checkbox for quickly including or excluding _all_ matches in the file.
* **Extension Management**
    * [Show supported translations in Extension Manager](https://github.com/adobe/brackets/pull/7995): Extension listings indicate which languages each extension has been translated for (at bottom).
    * [Install extension from local .zip file](https://github.com/adobe/brackets/pull/8166): Drag a .zip file onto the drop zone in Extension Manager's lower-left corner to install.
* **JavaScript Code Hints**
    * [#8561](https://github.com/adobe/brackets/pull/8561), [#8460](https://github.com/adobe/brackets/pull/8460): Fixed 0.41 bugs where some files showed no JS hints.
    * [Notification when hints ignore a certain file due to problems](https://github.com/adobe/brackets/pull/8269): Afterward, the file will automatically be added to the `"jscodehints.detectedExclusions"` preference; it will continue to be ignored until you remove it from that list.
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
Brackets's default appearance should be unchanged, but you can now use themes to customize your color scheme & editor font (see above).


API Changes
-----------
**Extension descriptions** - Extension descriptions beyond 200 characters are now hidden with "..." by default. Users can click to expand and see the full description. The extension's `keywords` metadata is no longer shown in Extension Manager (but still used when searching/filtering the listing).

New/Improved Extensibility APIs
-------------------------------
**Themes** - Themes are a special type of extension that contains a LESS file instead of any JavaScript code. [Learn how to create a theme.](https://github.com/adobe/brackets/wiki/Creating-Themes)

**Extension localization** - Extensions can now indicate which languages they are localized for by including an [`i18n` field in package.json](https://github.com/adobe/brackets/wiki/Extension-package-format#details). The supported languages are indicated in Extension Manager below the extension's description.

Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* _**Special thanks to [Miguel Castillo](https://github.com/MiguelCastillo)** for his major effort contributing the Themes functionality_
    * Pull requests: [1](https://github.com/adobe/brackets/pull/8302) [2](https://github.com/adobe/brackets/pull/8382) [3](https://github.com/adobe/brackets/pull/8400) [4](https://github.com/adobe/brackets/pull/8405) [5](https://github.com/adobe/brackets/pull/8418) [6](https://github.com/adobe/brackets/pull/8419) [7](https://github.com/adobe/brackets/pull/8447) [8](https://github.com/adobe/brackets/pull/8459) [9](https://github.com/adobe/brackets/pull/8475) [10](https://github.com/adobe/brackets/pull/8492) [11](https://github.com/adobe/brackets/pull/8505) [12](https://github.com/adobe/brackets/pull/8512)
* [Create Traditional Chinese translation](https://github.com/adobe/brackets/pull/8332) by [Pei-Tang Huang](https://github.com/tan9)
* [Create Galician translation](https://github.com/adobe/brackets/pull/8226) by [Iván Barcia](https://github.com/ivarcia)
* [Show supported translations of each extension in Extension Manager](https://github.com/adobe/brackets/pull/7995) by [Marcel Gerber](https://github.com/SAPlayer)
* [Collapse long extension descriptions](https://github.com/adobe/brackets/pull/8282) by [Triangle717](https://github.com/le717)
* [Fix Live Preview Highlight when Live Preview started on a CSS file](https://github.com/adobe/brackets/pull/8157) by [Marcel Gerber](https://github.com/SAPlayer)
* [Don't leave trailing whitespace after auto-indenting a close brace](https://github.com/adobe/brackets/pull/8439) by [Andrew MacKenzie](https://github.com/mackenza)
* [Fix `"sortDirectoriesFirst": true` setting with numbered folder names](https://github.com/adobe/brackets/pull/8341) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Add `background-blend-mode` to CSS code hints](https://github.com/adobe/brackets/pull/8146) by [coliff](https://github.com/coliff)
* [Add `text-rendering` to CSS code hints](https://github.com/adobe/brackets/pull/8414) by [Steffen Bruchmann](https://github.com/sbruchmann)
* [Add more values to `cursor` CSS code hints](https://github.com/adobe/brackets/pull/8370) by [Amin Ullah Khan](https://github.com/sprintr)
* [Hide .sass-cache folder from view](https://github.com/adobe/brackets/pull/8458) by [Andrew MacKenzie](https://github.com/mackenza)
* [Highlight .bash files as Bash, and .ino files as C++](https://github.com/adobe/brackets/pull/8340) by [Ty-Lucas Kelley](https://github.com/tylucaskelley)
* [Highlight .xaml files as XML](https://github.com/adobe/brackets/pull/8333) by [Triangle717](https://github.com/le717)
* [Correct error message when renaming/deleting folder fails](https://github.com/adobe/brackets/pull/8008) by [Marcel Gerber](https://github.com/SAPlayer)
* [Themes: Fix tab order in Themes dialog](https://github.com/adobe/brackets/pull/8387) by [Marcel Gerber](https://github.com/SAPlayer)
* [Themes: Dark theme fixes](https://github.com/adobe/brackets/pull/8521) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Add info to Getting Started docs: What is a project, how to switch projects](https://github.com/adobe/brackets/pull/7708) by [Marcel Gerber](https://github.com/SAPlayer)
* [Cleanup: Remove unused strings](https://github.com/adobe/brackets/pull/8563) by [Marcel Gerber](https://github.com/SAPlayer)
* [Themes: Fix menu label string](https://github.com/adobe/brackets/pull/8388) by [Triangle717](https://github.com/le717)
* [Fix missing space in string](https://github.com/adobe/brackets/pull/8309) by [Marcel Gerber](https://github.com/SAPlayer)
* [Simplified Chinese translation update](https://github.com/adobe/brackets/pull/8070) by [fengdi](https://github.com/fengdi)
* [Croatian translation update](https://github.com/adobe/brackets/pull/8469) by [Kruno H](https://github.com/diomed)
* [Czech translation update](https://github.com/adobe/brackets/pull/8060) ([and](https://github.com/adobe/brackets/pull/8279)) by [kvarel](https://github.com/kvarel)
* [Dutch translation update](https://github.com/adobe/brackets/pull/8281) by [githrdw](https://github.com/githrdw)
* [Finnish translation update](https://github.com/adobe/brackets/pull/8373) ([and](https://github.com/adobe/brackets/pull/8520)) by [valtlait](https://github.com/valtlait)
* [German translation update](https://github.com/adobe/brackets/pull/8568) by [Marcel Gerber](https://github.com/SAPlayer)
* [Italian translation update](https://github.com/adobe/brackets/pull/8264) ([and](https://github.com/adobe/brackets/pull/8569)) by [Denisov21](https://github.com/Denisov21)
* [Korean translation update](https://github.com/adobe/brackets/pull/8262) by [Yongmin Hong](https://github.com/revi)
* [Persian translation update](https://github.com/adobe/brackets/pull/8421) by [Mohammad Yaghobi](https://github.com/mohammadyaghobi)
* [Romanian translation update](https://github.com/adobe/brackets/pull/8307) by [Micleusanu Nicu](https://github.com/micnic)
* [Russian translation update](https://github.com/adobe/brackets/pull/8371) by [Maxim Khlobystov](https://github.com/gthacoder)
* [Spanish translation update](https://github.com/adobe/brackets/pull/8585) by [Chema Balsas](https://github.com/jbalsas)
* [Swedish translation update](https://github.com/adobe/brackets/pull/8567) by [Mikael Jorhult](https://github.com/mikaeljorhult)

#### Pulling source code from Git
* Rebuilding brackets-shell is _not_ required for this sprint.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 0.42
--------------------------
For details on the bugs addressed, please refer to [closed Release 0.42 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+0.42%22). Not all fixed bugs will be caught by this search query, however.