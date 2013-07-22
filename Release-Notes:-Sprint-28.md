_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 28 -- approximately June 24._

What's New in Sprint 28
-----------------------
* **Extensions**
    * [Extension Registry](https://trello.com/card/2-extension-registry-support-in-extension-manager/4f90a6d98f77505d7940ce88/911): .........
* **File Management**
    * [New Untitled documents](https://trello.com/card/3-create-save-untitled-file-file-new/4f90a6d98f77505d7940ce88/291): File > New now instantly creates an untitled document. You are not prompted for a name/location until save.
* **Drag & Drop**
    * [Drag _file_ onto window to open](https://trello.com/card/2-native-drag-n-drop-open-file/4f90a6d98f77505d7940ce88/281): Dragging a file onto the Brackets window works the same as File > Open. (This functionality was previously working on Windows only).
    * [Drag _folder_ onto window to switch projects](https://trello.com/card/2-native-drag-n-drop-open-folder/4f90a6d98f77505d7940ce88/873): Dragging a folder onto the Brackets window works the same as File > Open Folder.
* **Overall UI**
    * [Compact, dark titlebar (Windows)](https://trello.com/card/5-into-darkness-shell-windows/4f90a6d98f77505d7940ce88/874): A similar change will be [coming soon for Mac](https://trello.com/card/into-darkness-shell-osx/4f90a6d98f77505d7940ce88/900) too.
    * [Animation cues on open/close inline editor & hover preview (Quick View)](https://trello.com/card/1-ux-animate-inline-editor-and-quick-view/4f90a6d98f77505d7940ce88/809)

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-27...sprint-28#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-27...sprint-28#commits_bucket)


UI Changes
----------
* TODO - Windows dark titlebar
* TODO - Extension Registry

**New File** - File > New now creates an untitled document (name and location aren't specified until the first time you save). The folder tree context menu's New File command continues to work as before (location is determined by what you right-clicked, name is given immediately). Both commands no longer default to a .js file extension.

**New Folder** - Has been removed from the File menu. Use the context menu in the folder tree to access New Folder.

**Live Highlighting** - This toggle has been moved from the File menu to the View menu.

API Changes
-----------
* TODO - Extension Registry

New/Improved Extensibility APIs
-------------------------------


Known Issues
------------
* This build _does not support Mac OS 10.6 (Snow Leopard)_. We are hoping to restore 10.6 support very soon (see [#4394](https://github.com/adobe/brackets/issues/4394)).
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* [#4362](https://github.com/adobe/brackets/issues/4362): Slow startup of Brackets and Live Preview on Windows due to Chrome proxy settings. See workaround https://support.google.com/chrome/answer/106010?hl=en.
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------

Contributions _from_ the Brackets community
-------------------------------------------

Bugs fixed in Sprint 28
-----------------------
For details on the bugs addressed, please refer to [closed sprint 28 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=15&state=closed). A few of the fixed bugs might not be caught by this search query, however.