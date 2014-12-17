_This is a draft!_
--------------------
_This document will not be finalized until the end of Release 1.1 -- approximately December 17._


What's New in Release 1.1
-------------------------
* **Early Access**
    * [Cross-browser Live Preview with Dev Tools](https://github.com/adobe/brackets/wiki/Live-Development:-lifecycle-research-and-future-directions#live-development-managed-with-unprivileged-scripts): Although disabled by default, you can try out this brand-new version of Live Preview by enabling the `livedev.multibrowser` preference. The new Live Preview opens in your default browser, but you can paste the URL into other browsers and they'll all update at the same time. _And_ you can use the browser's dev tools without interrupting Live Preview. Does not currently support pages that use [your own custom server](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#lp-custom-server).
    * [Windows High DPI support](https://trello.com/c/CySUsuf4/1188-m-integrate-cef-2171-branch-chrome-39): To enable, right-click the app icon and select "Disable display scaling on high DPI settings" in the Compatibility tab. (The window controls in the upper right do not rescale yet, however).
* **Localization**
    * [Allow typing accent characters with AltGr](https://github.com/adobe/brackets/issues/8666): Brackets shortcuts that use Ctrl+Alt no longer supercede typing letter keys via AltGr.
    * Translation updates for: Chinese (Simplified), Dutch, Finnish, German, Italian, Korean, Persian-Farsi, Spanish, Ukrainian
* **Preferences**
    * [Select enabled code linters via preferences](https://github.com/adobe/brackets/pull/7362): For example, different projects can use a different set of linters.
    * [Programming language-specific preferences](https://github.com/adobe/brackets/pull/7889): Use the new `"language"` layer to set preferences based on file type, in addition to the path-specific or global preferences possible previously. [Read more](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#scope-of-preferences).
* **Quick Docs**
    * [Update to latest Web Platform Docs content](https://github.com/adobe/brackets/pull/9686)
* **Stability & Performance**
    * [New event dispatching system](https://trello.com/c/ogaZoRHJ/1377-events-infrastructure-improvements): Brackets core objects use a new event-listener API that protects against misbehaved extensions. See below for details.
    * [Improved typing performance while code hints open](https://github.com/adobe/brackets/pull/9791): Brackets limits the displayed list of code hints to 50 entries to improve performance. To override this limit, set the `"maxCodeHints"` preference.
    * [Improved performance when lots of files are changing in the background](https://github.com/adobe/brackets/pull/10120), such as large source-control checkouts or complex Grunt build scripts.

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.0...release-1.1#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.0...release-1.1#commits_bucket)


UI Changes
----------
Brackets now shows the project name in the title bar after the current filename (inside parentheses).

The menu item _Navigate > Go to First Error/Warning_ has been renamed to _Go to First Problem_.


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
* A new brackets-shell build is _required_ for this sprint.
    * If you build brackets-shell yourself, you _must_ re-run `grunt setup` before building (due to the CEF update).
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and _then_ `git submodule update --init --recursive` to ensure your local source tree reflects the update.


Bugs fixed in Release 1.1
-------------------------
For details on the bugs addressed, please refer to [closed Release 1.1 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.1%22). Not all fixed bugs will be caught by this search query, however.