_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 0.44 -- approximately October 5._

What's New in Release 0.44
--------------------------
* **Split View**
    * **[Vertical & Horizontal Split View](https://trello.com/c/WLeAC84F/1290-splitview-landing-in-master)**: View two files side by side or one above the other. (Note: it is not yet possible to view the same file in both panes).
* **Stylesheet Editing**
    * Fix bug [#9002](https://github.com/adobe/brackets/issues/9002) - Brackets freezes during Live Preview on a line within a CSS/SCSS/LESS block comment that contains nothing but "}" with no indent.
    * Fix bug [#8966](https://github.com/adobe/brackets/issues/8966) - Inline editor is blank if your CSS rule contains a vendor-prefixed property that uses a rgb()-like color value.
* **Files and Folders**
    * [Project tree improvements](https://trello.com/c/R5VQiTnS/1353-project-manager-revamp): Many small bug fixes in the sidebar file tree. Notably, it's now possible to right-click files that Brackets cannot open (such as binary files and non-UTF8 text files).
* **Preferences**
    * [New preference: show cursor in selected text](https://github.com/adobe/brackets/pull/8972): By default, Brackets hides the blinking cursor when you have a text selection; this behavior can now be disabled.
* **Localization**
    * [Extension name/description localization](https://github.com/adobe/brackets/pull/8987): The info displayed in Extension Manager, such as extension name & description, can now be localized via the [`package-i18n` property](https://github.com/adobe/brackets/wiki/Extension-package-format#packagejson-format).


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-0.43...release-0.44#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-0.43...release-0.44#commits_bucket)


UI Changes
----------
TODO


API Changes
-----------
TODO

New/Improved Extensibility APIs
-------------------------------
TODO - [Working Set View Extensibility](https://github.com/adobe/brackets/pull/9054)


Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
TODO

#### Pulling source code from Git
* TODO: any brackets-shell updates?
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 0.44
--------------------------
For details on the bugs addressed, please refer to [closed Release 0.44 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+0.44%22). Not all fixed bugs will be caught by this search query, however.