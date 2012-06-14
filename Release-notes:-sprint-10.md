Sprint 10 in progress

API Changes
-----------------
The following functions no longer take an ID parameter:

addMenuItem()
addMenuDivider()

Also, the relativeID parameter has changed. Previously this parameter specified the id of another MenuItem. Now it references the id of a Command that exists in the parent menu to specify a location. The net result is that MenuItems no longer have externally exposed ID’s.

These changes simplify the API, but will break existing extensions that add menus, so please update your Brackets extensions with this change.
