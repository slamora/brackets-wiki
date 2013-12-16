_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 35 -- approximately December 17._

What's New in Sprint 35
-----------------------
* **Find/Replace**
    * [Major Find/Replace UI overhaul](https://trello.com/c/pQb32zjf/1072-3-find-replace-ui-cleanup): Streamlined, condensed Replace command with the same incremental search and highlighting previously available only for Find. In both Find/Replace and Find in Files, new toggle buttons make case-sensitive and regexp searches easier to access (replacing the heuristics that used to decide these options based on your search text). After closing the search bar, Find Next/Previous now reflect any manual changes you make to the cursor position.
* **Live Development**
    * [Mac: No browser restart needed](https://github.com/adobe/brackets-shell/pull/359): Launching Live Preview no longer requires closing and reopening Chrome on Mac. (This was [already](https://github.com/adobe/brackets/wiki/Release-Notes:-Sprint-25) true on Windows).
* **Performance**
    * [Faster Brackets launch](https://github.com/adobe/brackets/pull/5776): Brackets now loads up to 50% faster thanks to minified JS and pre-compiled LESS.
* **Build Process**
    * [Use release branches](https://trello.com/c/nOXlN0yd/1069-1-infrastructure-support-for-release-timing): Branching a few days in advance of Brackets releases gives more 'stabilization time' to find bugs before releasing, and also allows work for the next sprint's features to start sooner.
    * Optimized builds: See notes below on new build step for minifying JS and precompiling LESS. _This is optional_: Brackets can still run directly from Git source without any build steps required, keeping the dev process lightweight.

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-34...sprint-35#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-34...sprint-35#commits_bucket)


UI Changes
----------
**Find/Replace & Find in Files** - Search options are now controlled by toggle buttons; regexp searches no longer need to be wrapped in `/`s. Options are remembered for your next search, even across Brackets sessions.

After closing the Find/Next bar, the Find Next/Previous shortcuts will operate relative to your current cursor position. (Previously, the shortcuts continued to follow the original search result sequence even if you moved the cursor).

API Changes
-----------
**Optimized builds** - _Working with a Brackets dev setup is unaffected_, but you can create optimized builds of Brackets for distribution using `grunt build`. This minifies and concatenates most JS code in Brackets, and precompiles LESS to CSS. Source in `src` is not modified; the build output is in a new sibling folder named `dist`. The build also generates a [source map](http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/), so optimized builds can still be debugged smoothly with dev tools (with no extra steps needed - the source map is automatically detected by dev tools) Stack traces reported by end users will reflect the line numbers of the original src, not the minified build.

**ModalBar** - The bar will no longer take any action when the Enter key is pressed, even when the `autoClose` argument was true. The "commit" event has been removed.

New/Improved Extensibility APIs
-------------------------------
**Custom viewers for binary files** - To register a custom viewer for files that cannot be represented as text (e.g. video or audio files): first register the file extensions with `LanguageManager().defineLanguage()`, then associate your custom UI with that language using `EditorManager.registerCustomViewer()`.


Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. [See workaround](https://support.google.com/chrome/answer/106010?hl=en).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
TODO

#### Pulling source code from Git
* A new brackets-shell build is _required_ for this sprint on Mac (Live Preview will not work correctly otherwise). Be sure to rerun `grunt setup` before building.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.
* As noted above, you _do not_ need to do anything with the new "optimized build" Grunt tasks. A dev environment still runs directly against the Git source with no JS/LESS build steps needed.


Bugs fixed in Sprint 35
-----------------------
For details on the bugs addressed, please refer to [closed sprint 35 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=22&state=closed). Not all fixed bugs will be caught by this search query, however.