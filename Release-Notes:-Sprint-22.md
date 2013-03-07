_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 22 -- approximately March 22._

What's New in Sprint 22
-----------------------
* **TODO: category headings**
    * [Install extension from URL](https://trello.com/card/3-extension-installation-url/4f90a6d98f77505d7940ce88/789)
    * [Word wrap](https://trello.com/card/2-word-wrap/4f90a6d98f77505d7940ce88/270): To enable, ...


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-21...sprint-22#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-21...sprint-22#commits_bucket)


UI Changes
----------
No major changes to existing features.


API Changes
-----------

New/Improved Extensibility APIs
-------------------------------

Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet.  To work around this, right click the Brackets app and choose Open.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.


Community contributions to Brackets
-----------------------------------

Contributions _from_ Brackets
-----------------------------

Bugs fixed in Sprint 22
-----------------------
For details on the bugs addressed, please refer to [closed sprint 22 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=9&state=closed). A few of the fixed bugs might not be caught by this search query, however.
