_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 24 -- approximately April 26._

What's New in Sprint 24
-----------------------
* **Visual Editing**
    * [Quick View for CSS colors, gradients, images](https://trello.com/card/2-hover-preview/4f90a6d98f77505d7940ce88/848): Hover over CSS code to see a popup preview of colors, gradients and images
    * [Quick Docs for CSS](https://trello.com/card/3-quick-docs-css/4f90a6d98f77505d7940ce88/839): Press Ctrl/Cmd+K on a CSS property to see inline documentation from the [Web Platform Docs project](http://docs.webplatform.org/).
* **Code Editing**
    * [Supercharged JavaScript code hints](https://trello.com/card/2-pull-request-tern-based-code-hinting/4f90a6d98f77505d7940ce88/849): Much more accurate code hints based on [Tern](http://ternjs.net/) type inferencing. Code hints even for items declared in other files! Now supports camelCase matching as well.
    * [Jump to Definition](https://trello.com/card/2-pull-request-tern-based-code-hinting/4f90a6d98f77505d7940ce88/849): Locate a function or property definition anywhere in your project, instantly.
    * [Function signature hints](https://trello.com/card/2-pull-request-tern-based-code-hinting/4f90a6d98f77505d7940ce88/849): Press Ctrl+Space inside a function call's `()`s to see information on its arguments and their types.
* **Overall UI**
    * [Project panel redesign](https://trello.com/card/2-ux-implement-project-panel/4f90a6d98f77505d7940ce88/807)


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-23...sprint-24#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-23...sprint-24#commits_bucket)


UI Changes
----------
**Project panel / folder tree** - _TODO!_
* Removed "Project Settings..." option from recent project drop-down menu


API Changes
-----------

New/Improved Extensibility APIs
-------------------------------
**Quick Docs** - _TODO!_

Known Issues
------------
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.

Community contributions to Brackets
-----------------------------------

Contributions _from_ the Brackets community
-------------------------------------------
**Contributions to [CodeMirror](https://github.com/marijnh/CodeMirror):**
* ...

Bugs fixed in Sprint 24
-----------------------
For details on the bugs addressed, please refer to [closed sprint 24 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=11&state=closed). A few of the fixed bugs might not be caught by this search query, however.