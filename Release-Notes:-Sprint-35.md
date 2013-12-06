_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 35 -- approximately December 16._

What's New in Sprint 35
-----------------------
* **Find/Replace**
    * [Major Find/Replace UI overhaul](https://trello.com/c/pQb32zjf/1072-3-find-replace-ui-cleanup): Streamlined, condensed Replace command with the same incremental search and highlighting previously available only for Find. In both Find/Replace and Find in Files, new toggle buttons make case-sensitive and regexp searches easier to access (replacing the unwritten heuristics that used to decide these options based on your search text). After closing the search bar, Find Next/Previous now reflect any manual changes you make to the cursor position.
* **Live Development**
    * [Mac: No browser restart needed](https://github.com/adobe/brackets-shell/pull/359): Launching Live Preview no longer requires closing and reopening Chrome on Mac. (This was [already](https://github.com/adobe/brackets/wiki/Release-Notes:-Sprint-25) true on Windows).
* **Preferences**
    * [Project- and file-specific editor settings](https://trello.com/c/kqFFDqhR/523-3-infrastructure-for-project-file-scoped-preferences): Use JSON configuration files to specify different indentation, word wrap, etc. settings for different folders or different file types. (TODO: link to detailed how-to)
* **Performance**
    * [Faster Brackets launch](https://github.com/adobe/brackets/pull/5776): Brackets now loads up to 50% faster thanks to minified JS and pre-compiled LESS.
* **CSS Animation**
    * [Visually edit CSS transition step timing functions](https://github.com/adobe/brackets/pull/5799): Invoke Quick Edit when your cursor is on any `step()` function in a CSS rule to edit it (building on top of the existing visual `cubic-bezier()` editor).
* **Linting**
    * [Multiple linting providers per Language](https://github.com/adobe/brackets/pull/5235): If multiple providers are registered, they will all be run and the results consolidated.
* **Build Process**
    * [Use release branches](https://trello.com/c/nOXlN0yd/1069-1-infrastructure-support-for-release-timing): Branching a few days in advance of Brackets releases gives more 'stabilization time' to find bugs before releasing, and also allows work for the next sprint's features to start sooner.
    * Optimized builds: See notes below on new build step for minifying JS and precompiling LESS.

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-34...sprint-35#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-34...sprint-35#commits_bucket)


UI Changes
----------
TODO: link to info on preferences file format

TODO: any more detail needed for Find/Replace?

API Changes
-----------
TODO: document debugging workflow for minified installed builds

TODO: linting API change for overriding built-in JSLint provider

**ModalBar** - The bar will no longer take any action when the Enter key is pressed, even when the `autoClose` argument was true. The "commit" event has been removed.

New/Improved Extensibility APIs
-------------------------------
TODO: custom viewer extensibility

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

Bugs fixed in Sprint 35
-----------------------
For details on the bugs addressed, please refer to [closed sprint 35 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=22&state=closed). Not all fixed bugs will be caught by this search query, however.