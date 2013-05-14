_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 25 -- approximately May 21._

What's New in Sprint 25
-----------------------
* **Live Preview**
    * [Improved connection reliability](https://trello.com/card/2-live-development-improve-launching-chrome-on-win/4f90a6d98f77505d7940ce88/835) on Windows: no longer need to restart Chrome.
* **Extensions**
    * [Extension manager](https://trello.com/card/2-extension-listing-remove-manage/4f90a6d98f77505d7940ce88/815): Listing of your currently installed extensions. Easier to uninstall extensions.
* **Code Editing**
    * [Improved typing performance](https://trello.com/card/3-research-rendering-typing-performance/4f90a6d98f77505d7940ce88/860): _MAYBE_


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-24...sprint-25#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-24...sprint-25#commits_bucket)


UI Changes
----------
No major changes to existing features.


API Changes
-----------
**Quick Open** - Search heuristics no longer applies special weighting automatically to match items that look like a path. Quick Open providers can override this behavior by supplying `matcherOptions: { segmentedSearch: true }` in the settings passed to `addQuickOpenPlugin()`.

**showOSFolder()** - placeholder...

New/Improved Extensibility APIs
-------------------------------
**Quick Open** - TODO: new `matcherOptions: { preferPrefixMatches: true }` option.

Known Issues
------------
* 

Community contributions to Brackets
-----------------------------------

Contributions _from_ the Brackets community
-------------------------------------------
**Contributions to [CodeMirror](https://github.com/marijnh/CodeMirror):**
* [CSS mode: record better info for @import directives](https://github.com/marijnh/CodeMirror/pull/1487) (enables Brackets URL hinting when typing an `@import`)
* [Don't continue line numbers next to horizontal scrollbar](https://github.com/marijnh/CodeMirror/pull/1493)
* [vim mode: Fix dialog box used for ':' commands](https://github.com/marijnh/CodeMirror/pull/1509)

**Contributions to [Tern](https://github.com/marijnh/tern):**
* [Fix capitalization error in jQuery API hint](https://github.com/marijnh/tern/pull/127)

Bugs fixed in Sprint 25
-----------------------
For details on the bugs addressed, please refer to [closed sprint 25 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=12&state=closed). A few of the fixed bugs might not be caught by this search query, however.