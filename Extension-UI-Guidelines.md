This page is an incomplete draft. Ultimately it will have a set of guidelines for all types of Brackets extension UI.

Since the our extensible UI functionality is still incomplete in Brackets, some of these guidelines will only become applicable later on.

## Menus

In general, our menu guidelines follow the standard UI practices for application menus on popular platforms.

### Application menu organization

1. **Avoid adding new top-level menus.** Try to fit your new functionality into an existing menu. If you think it really makes sense as a new top-level menu, please discuss it on the Brackets group first.
2. **If you're adding more than two or three items, add a submenu instead of separate menu items.** (Submenus are not yet implemented.)
3. **If you do add multiple items, surround them with separators.** (In future, we plan to have a "menu group" API that will make this easier to manage.)
4. **Avoid nesting submenus further than two layers deep.**
5. **Enable/disable menus rather than showing/hiding them.** Adding or removing menu items depending on context should only be done in context menus, not in the main application menus. (However, we are considering having a way to have truly mode-specific menu items show/hide, like ones that only apply to certain kinds of files, or only when focus is in a particular kind of inline editor. The trick is to do it in a way that doesn't confuse the user's muscle memory.)

### Context menus

These are not implemented in Brackets yet.

1. **Only show items that are relevant to the current context.** Unlike application menus, you should generally show/hide items in the context menu rather than simply disabling them. However, if an item would ordinarily be applicable to the current context, but the specific selection doesn't allow it, then disable it rather than hiding it. For example, if you want to add an item to the context menu for a particular file type, but it only works for writable files and the file is read-only, then disable it rather than hiding it.
2. **Context menu commands should also be available from the main menu.** Context menus are not always discoverable, so it's important to have an alternative entry point.
3. **Only put the most commonly-used functionality in the context menu.** Use the main application menu for other functions.

### Label style

1. **Use title capitalization for menu commands.** Capitalize the first letter of each word, except for prepositions and conjunctions like "and," "in," or "to" (e.g., "Find in Files"). Always capitalize the last word (e.g., "Save As...").
2. **Add an ellipsis to commands that require further input to complete.** In general, this applies to commands that pop up a modal dialog--e.g., "Save As...". However, commands that merely pop up informational dialogs (e.g. "About") should not have an ellipsis.
3. **Prefer checkmarks to changing menu item names.** For example, if you have a command to show or hide a panel called Results, make the menu item be "View > Results" and check or uncheck it depending on whether it's visible, instead of making the menu item switch between "Show Results" and "Hide Results". 
