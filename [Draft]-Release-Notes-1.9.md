What's New in Release 1.9
-------------------------

*  **[Reverse inspect in Live Preview](https://github.com/adobe/brackets/pull/13103)** by [saurabh95](https://github.com/saurabh95) : 
    Brackets 1.9 ships with Reverse-inspect in Live Preview. Now clicking on an element in Live Preview highlights the corresponding tag in the source code.

*  **[Support for “Replace All” action in Find & Replace](https://github.com/adobe/brackets/pull/12988)** by [amrelnaggar](https://github.com/amrelnaggar) : 
    Brackets now supports “Replace All” in Find & Replace along with batch operation.

*  **[Download count display and extension sorting based on download count](https://github.com/adobe/brackets/pull/13080)** by [saurabh95](https://github.com/saurabh95) : 
   Extension Manager now displays download count for listed extensions. Extensions can be sorted based on download count or published date in ‘Available’ and ‘Themes’ tab.

*  **[Mode toggle in Untitled document](https://github.com/adobe/brackets/pull/13086)** by [saurabh95](https://github.com/saurabh95) : 
    Language mode can be toggled for Untitled documents. Code coloring and Code Hints for the selected mode are also supported for Untitled documents.

*  **[Document toggle in split view](https://github.com/adobe/brackets/pull/12853)** by [arthur801031](https://github.com/arthur801031) : 
    Focus can be swapped between panes using keyboard shortcut.

*  **[Github organization support for Brackets extensions](https://github.com/adobe/brackets-registry/pull/70)** by [ficristo](https://github.com/ficristo) : 
    Github organizations can now own brackets extensions. All public owners of organization can update extensions.


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.8...release-1.9#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.8...release-1.9#commits_bucket)



Known Issues
------------
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------


#### Pulling source code from Git
_TODO: any brackets-shell updates? which of the below messages are applicable?_

* A new brackets-shell build is _required_ for this sprint. Be sure to rerun `grunt setup` before building.
* Recommended: rebuild or reinstall an updated brackets-shell (no critical updates, but there are bugfixes).
* Rebuilding/updating brackets-shell is _optional_ for this release.
* Rebuilding/updating brackets-shell is _not_ required for this release.
* brackets-shell's Node dependencies have changed. Run `npm install` before rebuilding brackets-shell.
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date.
* A submodule _URL_ was changed this sprint. Run `git submodule sync` and _then_ `git submodule update --init --recursive` to ensure your local source tree reflects the update.


Bugs fixed in Release 1.9
-------------------------
For details on the bugs addressed, please refer to [closed Release 1.9 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.9%22). Not all fixed bugs will be caught by this search query, however.