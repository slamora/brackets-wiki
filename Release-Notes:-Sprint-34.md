## _Note_
This release was superseded by [Sprint 34.1](https://github.com/adobe/brackets/wiki/Release-Notes:-Sprint-34.1), which contains two important bug fixes.

What's New in Sprint 34
-----------------------
* **Installation**
    * [Automatically replace older versions](https://trello.com/c/xxabXFIG/1017-2-brackets-update-in-place-via-installer): The Brackets installer on Windows now automatically overwrites previous versions, with no need to manually uninstall them. On all platforms, the app name no longer contains the sprint number. If you have manually assigned Brackets to any file associations, they will no longer need to be reassigned for each new build. (Note: these changes will not be evident until the first update, i.e. _next sprint_ - Sprint 35).
* **Overall UI**
    * [Dark themed window chrome on Mac](https://trello.com/c/oyGfEvrK/900-3-into-darkness-shell-osx): Similar to the update on Windows in Sprint 31.
    * Notable bug fixed in Win dark-themed window chrome
* **Image Preview**
    * [Pixel coordinates guide](https://github.com/adobe/brackets/pull/5944): When viewing an image file, a crosshair and tooltip indicate the pixel coordinates of your mouse cursor.
* **Code Hints**
    * Improved JavaScript code hints accuracy: More code hints provided for APIs in medium-sized files (500-2000 lines) and in cases where a module's exports have recently been updated.
* **Files & Folders**
    * [Linux: Support for SSHFS and REISERFS file systems](https://github.com/adobe/brackets-shell/pull/369): Resolves issue where Brackets treated project as an empty folder
    * [Mac: Any file can now be opened via drag/drop, regardless of file extension](https://github.com/adobe/brackets-shell/pull/367): Previously only certain file types were accepted.
* **Extensions**
    * [Extension Manager indicates available extension updates](https://github.com/adobe/brackets/pull/5838) via an icon overlaid on the "Installed" tab
    * **Note:** A few extensions may no longer work until you update them, due to the file system API change (see below).
* **Under the hood**
    * [New file system API](https://trello.com/c/PO7CIgqf/1050-1-merge-new-file-system): Some [API changes for extensions](https://github.com/adobe/brackets/wiki/File-System-API-Migration) -- see below. Slightly improves performance of some features, like Find in Files (larger improvements to come later!).
* **Localization**
    * New translations added: [Persian-Farsi](https://github.com/adobe/brackets/pull/5164), [Dutch](https://github.com/adobe/brackets/pull/5372) and [Romanian](https://github.com/adobe/brackets/pull/5836)
    * Updated translations: Brazilian Portuguese, Czech, Finnish, French, German, Japanese, Spanish, Swedish


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-33...sprint-34#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-33...sprint-34#commits_bucket)


UI Changes
----------
**Dark-themed window chrome on Mac** - the Mac shell now has a dark window chrome that visually complements the Brackets UI. (The Windows shell received a similar update in Sprint 31).

**Installation** - see above. Starting _next_ sprint, newer versions of Brackets will overwrite previously installed versions.

You can preserve an already installed version of Brackets to keep multiple versions of Brackets on your system at once. Simply copy it to a different location before installing the new release.  For example, on Mac, just rename `Brackets.app` to `Brackets Sprint 34.app` and then install the new release from the .dmg.  On Windows, copy the `\Program Files (x86)\Bracks` folder `Brackets Sprint 34`, and then install the new release from the .msi.  File associations will remain with the newer version that overwrites the original location.

**Extensions** - the Extension Manager 'Available' and 'Installed' tabs have switched places.

**Search** - Find in Files and Quick Open now include files you have opened that lie outside the root folder of your project.


API Changes
-----------
**File APIs** - Sprint 34 introduces a new `FileSystem` API that replaces `NativeFileSystem` and `FileIndexManager`. Some of the old APIs are removed immediately; others are deprecated and will be removed later. See the [API migration guide](https://github.com/adobe/brackets/wiki/File-System-API-Migration) and [discussion thread](https://groups.google.com/forum/#!topic/brackets-dev/95PyDKfMO0M) for details.

`FileUtils.getFilenameExtension()`, which was already deprecated, is now removed. Use `getFileExtension()` instead (note that it excludes the leading ".").


New/Improved Extensibility APIs
-------------------------------
**Documents** - New `DocumentManager.getDocumentText()` API can be significantly faster than using `getDocumentForPath()` if all you need to do is call `getText()` on it. Especially beneficial for bulk operations such as Find in Files.

**Quick Open** - QuickOpen providers can now specify a `label` property that is shown in the search bar when that provider is active. Also, some previously required properties are now optional.

**CodeMirror modes** - Brackets can now load CodeMirror modes that use the [multiplex](http://codemirror.net/doc/manual.html#addon_multiplex) or [overlay](http://codemirror.net/doc/manual.html#addon_overlay) addon utilities.


Known Issues
------------
* **Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.**
* Brackets _may freeze_ when opening a JS file whose siblings contain certain non-JS text, or whose siblings are large binary files. _Workaround:_ move those non-source-code files into a different folder. [See #6067](https://github.com/adobe/brackets/issues/6067). _This is fixed in the **[Sprint 34.1 update](https://github.com/adobe/brackets/wiki/Release-Notes:-Sprint-34.1)**._
* Editor renders incorrectly (missing text / wrong height) after opening a LESS file that begins in a tag selector (with no header comment, etc. before it). _Workaround:_ add a comment to top of each LESS file. [See #6057](https://github.com/adobe/brackets/issues/6057). _This is fixed in the **[Sprint 34.1 update](https://github.com/adobe/brackets/wiki/Release-Notes:-Sprint-34.1)**._
* [A few extensions](https://github.com/adobe/brackets/wiki/File-System-API-Migration#extensions-that-will-break) are incompatible with the file system API change. Most have updates available already; until you have updated, disable or remove the extension to avoid problems.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. [See workaround](https://support.google.com/chrome/answer/106010?hl=en).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Add Romanian translation](https://github.com/adobe/brackets/pull/5836) ([and](https://github.com/adobe/brackets/pull/5980)) by [Micleusanu Nicu](https://github.com/micnic)
* [Add Persian-Farsi translation](https://github.com/adobe/brackets/pull/5164) ([and](https://github.com/adobe/brackets/pull/5827)) by [Mohammad Yaghobi](https://github.com/mohammadyaghobi)
* [Add Dutch translation](https://github.com/adobe/brackets/pull/5372) by [Wouter92](https://github.com/Wouter92)
* [Display normal filename in URL code hints list, insteaed of URL-encoded name](https://github.com/adobe/brackets/pull/5854) by [Lance Campbell](https://github.com/lkcampbell)
* [Allow escaping "$" in regexp Replace mode](https://github.com/adobe/brackets/pull/5840) by [Marcel Gerber](https://github.com/SAPlayer)
* [Enable Visual Basic syntax highlighting](https://github.com/adobe/brackets/pull/5638) by [Michael Cole](https://github.com/micole)
* [Work on improving the Live Development experience on Mac](https://github.com/adobe/brackets-shell/pull/371) _(not enabled yet)_ by [fungl164](https://github.com/fungl164)
* [Fix #5741: Unable to properly launch Live Preview when an image file is selected](https://github.com/adobe/brackets/pull/5808) by [Marcel Gerber](https://github.com/SAPlayer)
* [Fix #5699: Re-invoking Find in Files while search bar open was unreliable](https://github.com/adobe/brackets/pull/5793) by [Marcel Gerber](https://github.com/SAPlayer)
* [Fix #4768: CSS code hints were mising some values for `display` and `transform`](https://github.com/adobe/brackets/pull/5713) by [Ross Brunton](https://github.com/RossBrunton)
* [Fix #5800: Nothing open in editor area after using Close Others](https://github.com/adobe/brackets/pull/5951) by [Sathyamoorthi](https://github.com/sathyamoorthi)
* [Fix #5575: Rule list should hide when deleting a rule leaves only one result left](https://github.com/adobe/brackets/pull/5646) by [Marcel Gerber](https://github.com/SAPlayer)
* [Fix #3063: Find doesn't scroll far enough to the right](https://github.com/adobe/brackets/pull/5861) by [Lance Campbell](https://github.com/lkcampbell)
* [Fix layout jump when previewing image](https://github.com/adobe/brackets/pull/5775) by [Bartosz Kaszubowski](https://github.com/Simek)
* [Don't show .settings, .c9revisions, or BTSync-related files](https://github.com/adobe/brackets/pull/5630) by [Michael Cole](https://github.com/micole)
* [Treat .css.erb files as CSS](https://github.com/adobe/brackets/pull/5832) by [filipemonteiroth](https://github.com/filipemonteiroth)
* [Treat Gemfile and Rakefile filenames as Ruby](https://github.com/adobe/brackets/pull/5743) by [Clay Miller](https://github.com/smockle)
* [Treat .cshtml and .vbhtml files as HTML](https://github.com/adobe/brackets/pull/5719) by [Mickael Puyfages](https://github.com/micka39)
* [Cleanup: Remove deprecated FileUtils.getFilenameExtension() API](https://github.com/adobe/brackets/pull/5828) by [Robin Venneman](https://github.com/rovenman)
* [Cleanup: Convert one more usage of CollectionUtils to Lo-Dash](https://github.com/adobe/brackets/pull/5744) by [Marcel Gerber](https://github.com/SAPlayer)
* [Cleanup: Fix misuse of brackets.getModule()](https://github.com/adobe/brackets/pull/5790) by [Marcel Gerber](https://github.com/SAPlayer)
* [Cleanup: Remove unneeded execute permissions](https://github.com/adobe/brackets/pull/5679) by [JohnnyT](https://github.com/johnnyt)
* [Spanish translation update](https://github.com/adobe/brackets/pull/5956) by [Chema Balsas](https://github.com/jbalsas)
* [Spanish translation fix](https://github.com/adobe/brackets/pull/5922) by [nikoskip](https://github.com/nikoskip)
* [German translation updates & improvements](https://github.com/adobe/brackets/pull/5959) ([part 2](https://github.com/adobe/brackets/pull/5905), [part 3](https://github.com/adobe/brackets/pull/5783), [part 4](https://github.com/adobe/brackets/pull/5820)) by [Marcel Gerber](https://github.com/SAPlayer)
* [Swedish translation update](https://github.com/adobe/brackets/pull/5955) ([part 2](https://github.com/adobe/brackets/pull/5930)) by [Mikael Jorhult](https://github.com/mikaeljorhult)
* [Brazilian Portuguese translation updates & fixes](https://github.com/adobe/brackets/pull/5146) by [Rodrigo Tavares](https://github.com/rodrigost23)
* [Czech translation fixes](https://github.com/adobe/brackets/pull/5867) ([part 2](https://github.com/adobe/brackets/pull/5862)) by [martinstarman](https://github.com/martinstarman) & [kvarel](https://github.com/kvarel)
* [Finnish translation update](https://github.com/adobe/brackets/pull/5798) by [valtlait](https://github.com/valtlait)
* [Finnish translation fix](https://github.com/adobe/brackets/pull/5812) by [Jukka Hyytiälä](https://github.com/jukkah)
* [Update list of translations in docs](https://github.com/adobe/brackets/pull/5730) by [Marcel Gerber](https://github.com/SAPlayer)

#### Pulling source code from Git
* A new brackets-shell build is _required_ for this sprint. Be sure to rerun `grunt setup` before building.
* A submodule was _added_ this sprint. Run `git submodule update --init` to ensure your source tree is fully up to date.
* A submodule was also _deleted_ this sprint. You may delete the _src/thirdparty/smart-auto-complete_ folder after syncing (Git will not automatically clean it up).


Bugs fixed in Sprint 34
-----------------------
For details on the bugs addressed, please refer to [closed sprint 34 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=21&state=closed). A few of the fixed bugs might not be caught by this search query, however.