_This is a draft!_ 
-------------------- 
_This page will not be finalized until the end of Release 1.2 -- approximately January 29._ 

What's New in Release 1.2 
------------------------- 
* _**TODO: draft**_
* **Code Editing**
    * [Drag & drop to move selected text](https://github.com/adobe/brackets/pull/9584)
    * [SVG code hints](https://github.com/adobe/brackets/pull/10294)
* **Bug Fixes**
    * [Multi-monitor window positioning](https://github.com/adobe/brackets-shell/pull/498): Brackets no longer starts up positioned offscreen after a monitor is unplugged or screen resolution is changed.
* **Linux**
    * [Open dialog improvements](https://github.com/adobe/brackets-shell/pull/496)


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/release-1.1...release-1.2#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/release-1.1...release-1.2#commits_bucket) 


UI Changes 
---------- 
**TODO** / or 
No major changes to existing features. 


API Changes 
----------- 
**TODO** 

New/Improved Extensibility APIs 
------------------------------- 
**TODO** 


Known Issues 
------------ 
* Activity Monitor in Mavericks (OS X 10.9) says the Brackets Helper process is "Not Responding" even when it's working normally ([#5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text. 
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead. 


Community contributions to Brackets 
----------------------------------- 
**TODO** 

#### Pulling source code from Git 
* Recommended: rebuild or reinstall an updated brackets-shell (no critical updates, but there are significant bugfixes).
* Some submodules were updated this sprint. Run `git submodule update` to ensure your source tree is fully up to date. 


Bugs fixed in Release 1.2 
------------------------- 
For details on the bugs addressed, please refer to [closed Release 1.2 bugs](https://github.com/adobe/brackets/issues?q=is%3Aclosed+milestone%3A%22Release+1.2%22). Not all fixed bugs will be caught by this search query, however.