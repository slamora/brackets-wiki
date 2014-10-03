What's New in Sprint 36
-----------------------
* **Preferences**
    * [Project- and file-specific editor settings](https://trello.com/c/kqFFDqhR/523-3-infrastructure-for-project-file-scoped-preferences): Use JSON configuration files to specify different indentation, word wrap, etc. settings for different projects, folders, or file types. Initial instructions are on the [How To Use Brackets](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#wiki-preferences) page.
* **File System & Performance**
    * [File/directory watchers](https://trello.com/c/zldzEXmk/292-2-file-directory-watching): Brackets will detect external modifications to files almost instantly and update the project tree, editor contents, etc. automatically (manually refreshing the tree is no longer needed).
    * [File caching](https://trello.com/c/zldzEXmk/292-2-file-directory-watching): Operations like Find in Files will be _much_ faster when used repeatedly, as the contents of files are now cached in memory.
* **Extensions**
    * [Extension download counts](https://trello.com/c/qOy9Slr1/799-2-extension-download-counts): We will begin tracking how many times each extension has been installed, and will post the download counts at periodic intervals. This will help extension authors decide where to prioritize their efforts, and will help Brackets developers understand what functionality is most important for our users.
    * ["Safe Mode"](https://github.com/adobe/brackets/issues/5078): Choose _Debug > Reload Without Extensions_ to temporarily run Brackets without any extensions loaded for troubleshooting. Extensions are re-enabled when you restart Brackets through any other means.
    * [Automated restart after updating/removing extensions](https://github.com/adobe/brackets/pull/6487): Previously, Brackets would quit and force you to relaunch it manually; now Brackets reloads automatically.
* **LESS Support**
    * [Code hints for CSS property names & values](https://github.com/adobe/brackets/pull/6268): The same code hints you see in CSS & SCSS files will now also appear in LESS files.
    * [Quick Docs support](https://github.com/adobe/brackets/pull/6419): Ditto – just like in CSS/SCSS files.
* **CSS Editing**
    * [Visually edit CSS transition step timing functions](https://github.com/adobe/brackets/pull/5799): Invoke Quick Edit when your cursor is on any `steps()` function in a CSS rule to edit it (building on top of the existing visual `cubic-bezier()` editor). Also works in LESS & SCSS files.
* **Overall UI**
    * [Windows: many window-chrome visual glitches fixed](https://github.com/adobe/brackets-shell/pull/408): Notably, the window border no longer bleeds onto other monitors.
    * [Windows: new, flatter scrollbar appearance](https://github.com/adobe/brackets/pull/6305)
* **Linting**
    * [Multiple linting providers per language](https://github.com/adobe/brackets/pull/5935): If multiple providers are registered for a given language, all the results will be shown together in the linting panel. (The built-in JSLint is disabled if other JavaScript linters are installed, however - as in previous sprints).
* **Quick Open**
    * [':line,col' syntax in Quick Open and Go to Line](https://github.com/adobe/brackets/pull/5612)
* **Localization**
    * [Greek translation added](https://github.com/adobe/brackets/pull/5378)
    * [Korean translation added](https://github.com/adobe/brackets/pull/6272)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-35...sprint-36#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-35...sprint-36#commits_bucket)


UI Changes
----------
**Scrollbars** - Scrollbars on all versions of Windows now have a flatter, Windows-8-style appearance that matches the overall Brackets UI design better.

**CSS code hints** - When selecting a property name hint, a space is now automatically inserted after the ":".


API Changes
-----------
**FileSystem** - Nothing needs to change for most code; the public FileSystem APIs are unchanged except for the `"change"` event ([see docs for details](https://github.com/adobe/brackets/blob/master/src/filesystem/FileSystem.js#L55)). All file operations automatically receive the benefits of caching, and all Document objects are automatically updated (firing appropriate events) when file watchers detect an update.

The API for developing an [alternative file system implementation](https://github.com/adobe/brackets/wiki/File-System-Implementations) has changed, however.

**Linting** - The process for registering a linter is unchanged, but it's now possible to register more than one linter per file type. (Except that, as before, the built-in JSLint provider is automatically disabled when any other JS linter is registered).

The result yielded by `CodeInspection.inspectFile()` has changed: instead of a single linter result, it now returns an array of objects, each containing the result of one specific linter.

**CSS tokens** - The latest version of CodeMirror parses CSS files differently. If you rely on the raw tokens emitted by CodeMirror, your code will likely need updates; see [PR #6268](https://github.com/adobe/brackets/pull/6268) for notes. However, the Brackets `CSSUtils` API remains unchanged.

**Node version** - The internal copy of Node.js included in Brackets has been upgraded to 0.10.24 (from 0.10.18).

**Dialog boxes** - The built-in dialog box shown via `showModalDialog()` no longer has a close button: it can only be dismissed by clicking the main buttons at bottom, or by pressing a shortcut such as Escape. Custom dialogs displayed via `showModalDialogUsingTemplate()` are unaffected.

New/Improved Extensibility APIs
-------------------------------
**Preferences** - Although new preferences APIs have been added, please _continue to use the old APIs_ for now. The old APIs remain unchanged. The new APIs are still undergoing some flux and will not be ready for wider use until Sprint 37.

**Node integration** - New, simplified `NodeDomain` API makes connecting to Node from Brackets code easier and less error-prone. Full docs TBD, but [details here](https://github.com/adobe/brackets/pull/6193) in the meantime.

Also, Node-side APIs can now send back binary data to Brackets by returning a `Buffer`, which will show up on the Brackets side as an `ArrayBuffer`. [Read more](https://github.com/adobe/brackets/pull/6339).

**StatusBar** - `addIndicator()` can now actually be used to add indicators to the status bar (previously it did not do what the name implied, making it less useful). It also has a new optional parameter, `insertBefore`. If specified, the new statusbar indicator is inserted before the indicator with id of `insertBefore`. If omitted, the indicator will be inserted at the beginning. (In some previous sprints, the indicator 


Known Issues
------------
* **Windows XP issues:** Unable to install extensions, run Live Preview, or detect external file changes instantly. [Bug 6951](https://github.com/adobe/brackets/issues/6951).
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. [See workaround](https://support.google.com/chrome/answer/106010?hl=en).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Debug > Reload Without Extensions command](https://github.com/adobe/brackets/pull/6334) by [Lance Campbell](https://github.com/lkcampbell) ([part 2](https://github.com/adobe/brackets/pull/6411), [part 3](https://github.com/adobe/brackets/pull/6484))
* [After extension is updated/removed, reload Brackets instead of quitting](https://github.com/adobe/brackets/pull/6487) by [Lance Campbell](https://github.com/lkcampbell)
* [Support ':line,col' syntax in Quick Open and Go to Line](https://github.com/adobe/brackets/pull/5612) by [Chema Balsas](https://github.com/jbalsas)
* [Add Greek translation](https://github.com/adobe/brackets/pull/5378) by [Theodore Keloglou](https://github.com/sirodoht)
* [Add Korean translation](https://github.com/adobe/brackets/pull/6272) by [Ki-soo Kim](https://github.com/heisice)
* [Include space after colon when inserting CSS code hints](https://github.com/adobe/brackets/pull/6317) by [Matt Stow](https://github.com/stowball)
* [Mac: Ensure Chrome is focused when launching Live Preview](https://github.com/adobe/brackets-shell/pull/404) by [fungl164](https://github.com/fungl164)
* [Mac: Don't force a switch from mobile to discrete graphics GPU](https://github.com/adobe/brackets-shell/pull/399) by [David Mead](https://github.com/daveygm)
* [Improve code hint behavior when pressing Backspace](https://github.com/adobe/brackets/pull/6242) by [Yu Ao](https://github.com/YuAo) ([partially disabled](https://github.com/adobe/brackets/pull/6397) for now)
* [Disable "Update" button for "dev" folder extensions (which can't actually be updated)](https://github.com/adobe/brackets/pull/6222) by [Marcel Gerber](https://github.com/SAPlayer)
* [Custom, flatter Windows scrollbar appearance](https://github.com/adobe/brackets/pull/6305) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Remove "X" close button from all dialog boxes](https://github.com/adobe/brackets/pull/6329) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Fix StatusBar.addIndicator() so it's usable by extensions](https://github.com/adobe/brackets/pull/6304) by [Lance Campbell](https://github.com/lkcampbell)
* [Fix problem with new preferences scopes implementation](https://github.com/adobe/brackets/pull/6653) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Add CSS value hints for `list-style`, `list-style-image`](https://github.com/adobe/brackets/pull/6360) by [Matt Stow](https://github.com/stowball)
* [Add CSS value hints for `margin` and related properties (enumerated values only)](https://github.com/adobe/brackets/pull/6359) by [Matt Stow](https://github.com/stowball)
* [Fix CSS value hints for `font-variant`](https://github.com/adobe/brackets/pull/6313) by [Matt Stow](https://github.com/stowball)
* [Fix Find in Files UI wrap issues at narrow window sizes](https://github.com/adobe/brackets/pull/6529) by [ishanatmuz](https://github.com/ishanatmuz)
* [Fix UI overlapping the side toolbar at narrow window sizes](https://github.com/adobe/brackets/pull/6569) by [ishanatmuz](https://github.com/ishanatmuz)
* [Make CodeInspection API robust to linters that return bad line numbers](https://github.com/adobe/brackets/pull/6442) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Fix UrlParams utility to handle URLs without parameters](https://github.com/adobe/brackets/pull/6246) by [Lance Campbell](https://github.com/lkcampbell)
* [Fix spacing in "Install extension" progress dialog](https://github.com/adobe/brackets/pull/6063) by [Marcel Gerber](https://github.com/SAPlayer)
* [Change AppShellFileSystem impl to match docs about optional args](https://github.com/adobe/brackets/pull/6194) by [Kyle Binns](https://github.com/klbinns) (subsequently switched to change docs to match impl instead)
* [Update localization README to include new languages](https://github.com/adobe/brackets/pull/6490) by [Marcel Gerber](https://github.com/SAPlayer)
* [Update About box copyright year](https://github.com/adobe/brackets/pull/6349) by [Marcel Gerber](https://github.com/SAPlayer)
* [Czech translation update](https://github.com/adobe/brackets/pull/6299) by [kvarel](https://github.com/kvarel)
* [Dutch translation fixes](https://github.com/adobe/brackets/pull/6394) by [Eric Smekens](https://github.com/EricSmekens)
* [German translation update](https://github.com/adobe/brackets/pull/6353) by [Marcel Gerber](https://github.com/SAPlayer) ([part 2](https://github.com/adobe/brackets/pull/6366), [part 3](https://github.com/adobe/brackets/pull/6632), [part 4](https://github.com/adobe/brackets/pull/6754))
* [Italian translation update](https://github.com/adobe/brackets/pull/6650) by [asbruff](https://github.com/asbruff)
* [Italian translation fixes](https://github.com/adobe/brackets/pull/6292) by [Soleedus](https://github.com/Soleedus)
* [Russian translation update](https://github.com/adobe/brackets/pull/6628) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Russian translation update](https://github.com/adobe/brackets/pull/6123) by [sergeizelenyi](https://github.com/sergeizelenyi)
* [Spanish translation update](https://github.com/adobe/brackets/pull/6627) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Swedish translation update](https://github.com/adobe/brackets/pull/6636) by [Mikael Jorhult](https://github.com/mikaeljorhult)


#### Pulling source code from Git
* A new brackets-shell build is _required_ for this sprint.
* If you have ever built brackets-shell before, **delete** the `brackets-shell\deps\node` folder and any Node-related files in `brackets-shell\downloads`, then re-run `grunt setup` (this one-time step is required due to a change in the node executable filename).
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.

Bugs fixed in Sprint 36
-----------------------
For details on the bugs addressed, please refer to [closed sprint 36 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=23&state=closed). Not all fixed bugs will be caught by this search query, however.