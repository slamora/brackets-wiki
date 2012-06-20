**Sprint 10 is still in progress.** These are **draft** notes.

UI Changes
----------
* "Debug > Enable JSLint" has moved to "View > Enable JSLint"
* "Debug > Use Tab Characters" has moved to "Edit > Use Tab Characters"

Menu API Changes
----------------
The following functions no longer take an ID parameter:

addMenuItem()
addMenuDivider()

Also, the relativeID parameter has changed. Previously this parameter specified the id of another MenuItem. Now it references the id of a Command that exists in the parent menu to specify a location. The net result is that MenuItems no longer have externally exposed ID’s.

These changes simplify the API, but will break existing extensions that add menus, so please update your Brackets extensions with this change.

Project API Changes
-------------------
The events dispatched by ProjectManager have changed. Instead of dispatching `initializeComplete` and `projectRootChanged`, we now dispatch `projectOpen` and `beforeProjectClose` events. `beforeProjectClose` is dispatched before the project root is about to change, and `projectOpen` is dispatched afterwards. Also, `projectOpen` is dispatched after the initial project is opened when Brackets first launches.