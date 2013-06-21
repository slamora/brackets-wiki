_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 27 -- approximately June 27._

What's New in Sprint 27
-----------------------
* **Research**
    * [Data Structure for HTML DOM Edit Mapping](https://trello.com/c/lGIOrElQ): Research task to figure out how to do live HTML editing. 
    * [Drag and Drop](https://trello.com/c/PDyKD95J): Research task to figure out how to intercept native drag events on the mac so we can support dropping files onto the Brackets window. 
* **Architecture**
    * [Update Brackets Shell (CEF)](https://trello.com/c/YQlER69q): Update to the latest CEF, v 3.1453.1255.

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-26...sprint-27#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-26...sprint-27#commits_bucket)


UI Changes
----------

API Changes
-----------

**brackets.app**  
* `openURLInDefaultBrowser` -- The parameters for this API were reversed from what was the convention for the other APIs. This was confusing so to conform to convention and accommodate an optional callback, the parameters for this API were normalized.  The API usage is now `brackets.app.openURLInDefaultBrowser(url, err)` where `err` is an optional function callback that takes 1 argument.

New/Improved Extensibility APIs
-------------------------------


Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3458](https://github.com/adobe/brackets/issues/3458): Quick View does not yet support the latest CSS gradient syntax (using `to` keyword, unprefixed `radial-gradient`, `repeating-linear-gradient` or `repeating-radial-gradient`).
* [#3570](https://github.com/adobe/brackets/issues/3570): Mac only - Quick View popover may not appear after resizing window or going fullscreen. Move the mouse to the top of the screen to fix.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
_TODO_ - Fill out this section. This item is a placeholder template.
* [Improving the Dialog API](https://github.com/adobe/brackets/pull/3086) by [Tomás Malbrán](https://github.com/TomMalbran)

Contributions _from_ the Brackets community
-------------------------------------------

* None

Bugs fixed in Sprint 27
-----------------------
For details on the bugs addressed, please refer to [closed sprint 27 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=14&state=closed). A few of the fixed bugs might not be caught by this search query, however.