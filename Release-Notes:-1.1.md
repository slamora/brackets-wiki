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
    * Fixed [issue](https://github.com/adobe/brackets/issues/9813) that caused changes inside `<style>` blocks in PHP files to be lost in some cases.
    * Fixed [two](https://github.com/adobe/brackets/issues/10013) [issues](https://github.com/adobe/brackets/issues/9840) that caused Quick Edit to miss some CSS rules.


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

**Chromium version** - Updated from Chromium 29 to 39, **except on Linux** which still uses Chromium 29 for now ([help needed for Linux build - see here for details](https://groups.google.com/forum/#!topic/brackets-dev/QkYuQuCsqdY)).

**Developer tools** - _Debug > Show Developer Tools_ now opens a Brackets window instead of a tab in your browser. If Brackets gets badly hosed, you can still open dev tools in Chrome browser by manually visiting [http://localhost:9234/](http://localhost:9234/).

**jQuery** - Upgraded from 2.1.0 to 2.1.1 <br>
**Lodash** - Upgraded from 2.2.0 to 2.4.1 <br>
**LESS** - Upgraded from 1.7.0 to 1.7.5 <br>
**RequireJS** - Upgraded from 2.1.5 to 2.1.15

**SVG language** - SVG files are now reported by LanguageManager as `"svg"` instead of `"xml"` as before.

**Markdown language** - Markdown files may now be reported by LanguageManager as "`gfm`" in addition to the `"markdown"` language reported before. This lets users distinguish GitHub-flavored Markdown files from "classic" Markdown.

**File tree icon providers** - Providers can now return falsy to add no icon to an item, consistent with working set icon providers. Clarified documentation for all provider hooks. Also, the large negative margin + large positive padding hack used in tree items' CSS [has been removed](https://github.com/adobe/brackets/pull/10011).

New/Improved Extensibility APIs
-------------------------------
**Themes** - specify `"addModeClass": true` in the theme info JSON to enable using language-specific CSS selectors (e.g. use different colors for the same token name, depending on file type). This causes a slight performance hit, so please only enable if you're using it.


Known Issues
------------
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [New Live Preview implementation](https://github.com/adobe/brackets/pull/10010) (+ [part 2](https://github.com/adobe/brackets/pull/10101), [part 3](https://github.com/adobe/brackets/pull/10129)) _(disabled by default for now)_ by [Sebastian Salvucci (Intel Corp)](https://github.com/sebaslv) & [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Show current project name in titlebar](https://github.com/adobe/brackets/pull/7789) by [Triangle717](https://github.com/le717)
* [Select enabled code linters via preferences](https://github.com/adobe/brackets/pull/7362) (+ [part 2](https://github.com/adobe/brackets/pull/10164)) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Update Web Platform Docs content; new script for easier updating](https://github.com/adobe/brackets/pull/9686) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Programming language-specific preferences via `"language"` layer](https://github.com/adobe/brackets/pull/7889) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Limit code hint list length to improve typing performance](https://github.com/adobe/brackets/pull/9791) (+ [part 2](https://github.com/adobe/brackets/pull/10093)) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Separate GitHub Flavored Markdown from regular Markdown mode; recognize .mdown/.mkdn as Markdown](https://github.com/adobe/brackets/pull/9780) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add theme option "addModeClass" (for language-aware themes)](https://github.com/adobe/brackets/pull/10039) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Update to latest dot releases of jQuery, LESS, Lodash, RequireJS](https://github.com/adobe/brackets/pull/9968) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Expose internal linting API to get list of providers](https://github.com/adobe/brackets/pull/9189) (+ [part 2](https://github.com/adobe/brackets/pull/10068))  by [Mark Simulacrum](https://github.com/Mark-Simulacrum)
* [Fix: No image preview if filename contains "%" or "#"](https://github.com/adobe/brackets/pull/9190) by [Mark Simulacrum](https://github.com/Mark-Simulacrum)
* [Fix Quick Docs links that point to currently viewed page](https://github.com/adobe/brackets/pull/8724) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix: Find In Files result disappear after saving file with prefix of other filename](https://github.com/adobe/brackets/pull/9940) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Fix: Warnings logged with async linters when launching Brackets](https://github.com/adobe/brackets/pull/9392) (+ [part 2](https://github.com/adobe/brackets/pull/10023)) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Fix freeze in new Live Preview implementation](https://github.com/adobe/brackets/pull/10207) by [Scott Wadden](https://github.com/dsrw)
* [Fix new event API .off() chaining](https://github.com/adobe/brackets/pull/10024) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Allow focusing all types of `<input>` fields in dialogs](https://github.com/adobe/brackets/pull/9915) by [Eliezer Bernart](https://github.com/eliezerb)
* [Rename "Go to First Error/Warning" -> "Go to First Problem" for consistency](https://github.com/adobe/brackets/pull/9835) by [Jacob Lauritzen](https://github.com/Jacse)
* [Ensure preferences error dialog gets focus](https://github.com/adobe/brackets/pull/10022) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Fix error message wording when unable to read directory](https://github.com/adobe/brackets/pull/9948) by [Kevin Mo](https://github.com/encadyma)
* [In About Dialog, don't show "{0}" when running from git source with detached head](https://github.com/adobe/brackets/pull/9777) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Add performance measurement for indexing project files](https://github.com/adobe/brackets/pull/9905) by [Arzhan "kai" Kinzhalin (Intel Corp)](https://github.com/busykai)
* [Chinese (Simplified) translation update](https://github.com/adobe/brackets/pull/9874) by [Michael J.](https://github.com/michaeljayt)
* [Dutch translation fix](https://github.com/adobe/brackets/pull/9804) by [Pieter Willekens](https://github.com/ispieter)
* [Finnish translation update](https://github.com/adobe/brackets/pull/9998) by [valtlait](https://github.com/valtlait)
* [German translation update](https://github.com/adobe/brackets/pull/10176) by [Marcel Gerber](https://github.com/MarcelGerber)
* [Italian translation update](https://github.com/adobe/brackets/pull/9962) by [Christopher Pecoraro](https://github.com/chrispecoraro)
* [Italian translation fixes](https://github.com/adobe/brackets/pull/9885) by [Elia Cereda](https://github.com/EliaCereda)
* [Korean translation update](https://github.com/adobe/brackets/pull/10053) by [Taegon Kim](https://github.com/taggon)
* [Persian-Farsi translation update](https://github.com/adobe/brackets/pull/10062) by [Pooya Parsa Dadashi](https://github.com/datamweb)
* [Spanish translation update](https://github.com/adobe/brackets/pull/10181) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Ukrainian translation update](https://github.com/adobe/brackets/pull/9967) by [Maks Lyashuk](https://github.com/probil)
* [Cleanup: Fix some @link / @see tags in docs](https://github.com/adobe/brackets/pull/9782) by [Chema Balsas](https://github.com/jbalsas)
* [Cleanup: Remove some whitespace](https://github.com/adobe/brackets/pull/9869) by [Pavel Dvořák](https://github.com/dvorapa)


#### Pulling source code from Git
* A new brackets-shell build is _required_ for this sprint.
    * If you build brackets-shell yourself, you _must_ re-run `grunt setup` before building (due to the CEF update).
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and _then_ `git submodule update --init --recursive` to ensure your local source tree reflects the update.


Bugs fixed in Release 1.1
-------------------------
For details on the bugs addressed, please refer to [closed Release 1.1 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.1%22). Not all fixed bugs will be caught by this search query, however.