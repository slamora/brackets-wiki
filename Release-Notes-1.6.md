_This is a draft!_
--------------------
_This page will not be finalized until the end of Release 1.6 -- approximately January 14._

What's New in Release 1.6
-------------------------
*  **[Add flip-view and close buttons to pane-headers](https://github.com/adobe/brackets/pull/11749)** by [petetnt](https://github.com/petetnt) : In split view, documents now can be closed via the new close button added at the right corners of both the panes. Also, document now can be easily moved into the other pane, using the new flip button.

* **[Type Inference in hint list](https://github.com/adobe/brackets/pull/11949)** by [swmitra](https://github.com/swmitra) : Javascript code hints are now further improved to show more information like the type and documentation associated with the function/variable along with any hyperlinks.

* **[Split View (Same Document)](https://github.com/adobe/brackets/pull/11820)** by [swmitra](https://github.com/swmitra) : Same document can now be opened in both the panes, when in split view.

* **[Toggle panels and no-Distraction mode ](https://github.com/adobe/brackets/pull/11732)** by abose : Brackets now has a no-distraction mode which can be enabled using *Cmd-Shift-2* shortcut. Once enabled, this mode will hide the toolbar bar panel on the right as well as the project tree.


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.5...release-1.6#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.5...release-1.6#commits_bucket)


UI Changes
----------
**TODO** - list notable changes in _existing_ features ...or:
No major changes to existing features.


API Changes
-----------
**TODO** - list breaking API changes


Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [[New feature] Add flip-view and close buttons to pane-headers](https://github.com/adobe/brackets/pull/11749) by [petetnt](https://github.com/petetnt)
* [Allow CSS Code Hints in PHP](https://github.com/adobe/brackets/pull/11751) by [Amin Ullah Khan](https://github.com/sprintr)
* [Revert "Move PathUtils to Brackets core code"](https://github.com/adobe/brackets/pull/11745) by [Martin Zagora](https://github.com/zaggino)
* [Add <template> to HtmlTags.json](https://github.com/adobe/brackets/pull/11486) by [verballyinsane](https://github.com/verballyinsane)
* [Simplified Chinese: Fix a few translation messages and placeholders](https://github.com/adobe/brackets/pull/10331) by [Michael J.](https://github.com/michaeljayt)
* [Remove references to deprecated FileSystem APIs in SpecRunner](https://github.com/adobe/brackets/pull/11781) by [petetnt](https://github.com/petetnt)
* [Finnish translation, release 1.5, part 2](https://github.com/adobe/brackets/pull/11816) by [valtlait](https://github.com/valtlait)
* [Code folding unit tests](https://github.com/adobe/brackets/pull/11584) by [Patrick Oladimeji](https://github.com/thehogfather)
* [addresses #11356 xml start tags spanning multiple lines](https://github.com/adobe/brackets/pull/11366) by [Patrick Oladimeji](https://github.com/thehogfather)
* [Czech translation for v1.5](https://github.com/adobe/brackets/pull/11834) by [Pavel Dvořák](https://github.com/dvorapa)
* [Typo in nls/root/strings.js](https://github.com/adobe/brackets/pull/11835) by [Pavel Dvořák](https://github.com/dvorapa)
* [Add UrlCodeHints for poster-attribute](https://github.com/adobe/brackets/pull/11885) by [petetnt](https://github.com/petetnt)
* [Ensure that .cm-error gets applied last](https://github.com/adobe/brackets/pull/11894) by [petetnt](https://github.com/petetnt)
* [turtle support added](https://github.com/adobe/brackets/pull/11895) by [bozicb](https://github.com/bozicb)
* [remove predefined values from cubic-bezier()](https://github.com/adobe/brackets/pull/11786) by [myakura](https://github.com/myakura)
* [Port missing piece of the indent-wrap hack from Codemirror. Fixes #11963](https://github.com/adobe/brackets/pull/11964) by [petetnt](https://github.com/petetnt)
* [Eslint](https://github.com/adobe/brackets/pull/11693) by [ficristo](https://github.com/ficristo)
* [Update Tern submodule URL](https://github.com/adobe/brackets/pull/11994) by [Marcel Gerber](https://github.com/MarcelGerber)
* [ESLint: enabled no-trailing-spaces and eol-last rules](https://github.com/adobe/brackets/pull/11998) by [ficristo](https://github.com/ficristo)
* [Reintroduce JSLint as a prefered linter](https://github.com/adobe/brackets/pull/12002) by [ficristo](https://github.com/ficristo)
* [Fix Flipview focus issues](https://github.com/adobe/brackets/pull/12060) by [petetnt](https://github.com/petetnt)

#### Pulling source code from Git
_TODO: any brackets-shell updates? which of the below messages are applicable?_

* A new brackets-shell build is _required_ for this sprint. Be sure to rerun `grunt setup` before building.
* Recommended: rebuild or reinstall an updated brackets-shell (no critical updates, but there are bugfixes).
* Rebuilding/updating brackets-shell is _optional_ for this release.
* Rebuilding/updating brackets-shell is _not_ required for this release.
* brackets-shell's Node dependencies have changed. Run `npm install` before rebuilding brackets-shell.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and _then_ `git submodule update --init --recursive` to ensure your local source tree reflects the update.


Bugs fixed in Release 1.6
-------------------------
For details on the bugs addressed, please refer to [closed Release 1.6 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.6%22). Not all fixed bugs will be caught by this search query, however.