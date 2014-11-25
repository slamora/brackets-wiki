_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 1.1 -- approximately December 9._

What's New in Release 1.1
-------------------------
* _TODO: draft_
* **Display**
    * [Windows High DPI support](https://trello.com/c/CySUsuf4/1188-m-integrate-cef-2171-branch-chrome-39): Like the existing support for Retina Macs, Brackets will automatically rescale on Windows high-DPI displays.
* **Localization**
    * [Allow typing accent characters with AltGr](https://github.com/adobe/brackets/issues/8666): Brackets shortcuts that use Ctrl+Alt no longer supercede typing letter keys via AltGr.
    * Translation updates for: _...TODO..._
* **Stability**
    * [New event dispatching system](https://trello.com/c/ogaZoRHJ/1377-events-infrastructure-improvements): Brackets core objects use a new event-listener API that protects against misbehaved extensions. See below for details.


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.0...release-1.1#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.0...release-1.1#commits_bucket)


UI Changes
----------
Brackets now shows the project name in the title bar after the current filename (inside parentheses).


API Changes
-----------
**Event listeners** - Instead of using jQuery events, Brackets core modules and model objects now provide their own simpler on()/off() event listener API. (For DOM objects, continue to use jQuery events). When the legacy `$().on()/off()` APIs are used on objects that support the new API, they automatically call the new API instead. The new API is more robust to listeners that throw exceptions, uses less memory, and is easier to debug.

Related changes:

* Editor's `"keyEvent"` event is deprecated. Use the more specific events `"keyup"`, `"keypress"`, or `"keydown"` instead. (However, in most cases you should use Document's `"change"` event instead of _any_ of the Editor events).
* NodeConnection events now only use `:` separators - the deprecated `.` separators (which conflict with the more standard use of `.` as an event-listener namespace) are no longer supported.

**Chromium version** - Updated from Chromium 29 to 39.

**Developer tools** - _Debug > Show Developer Tools_ now opens a Brackets window instead of a tab in your browser. If Brackets gets badly hosed, you can still open dev tools in the browser by manually visiting http://localhost:9234/.

New/Improved Extensibility APIs
-------------------------------
_TODO_


Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
_TODO_

#### Pulling source code from Git
* TODO: any brackets-shell updates?
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.


Bugs fixed in Release 1.1
-------------------------
For details on the bugs addressed, please refer to [closed Release 1.1 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.1%22). Not all fixed bugs will be caught by this search query, however.