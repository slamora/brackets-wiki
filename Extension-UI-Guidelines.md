This page is an incomplete draft. Ultimately it will have a set of guidelines for Brackets extension developers.

## Menus

Since the menu extension functionality is still incomplete in Brackets, some of these guidelines will only become applicable later on.

In general, our menu guidelines follow the standard UI practices for application menus on popular platforms.

### Application menu organization

1. **Avoid adding new top-level menus.** Try to fit your new functionality into an existing menu. If you think it really makes sense as a new top-level menu, please discuss it on the Brackets group first.
2. **If you're adding more than two or three items, add a submenu instead of separate menu items.** (Submenus are not yet implemented.)
3. **If you do add multiple items, surround them with separators.** (In future, we plan to have a "menu group" API that will make this easier to manage.)
4. **Avoid nesting submenus further than two layers deep.**
5. **Enable/disable menus rather than showing/hiding them.** Adding or removing menu items depending on context should only be done in context menus, not in the main application menus. (However, we are considering having a way to have truly mode-specific menu items show/hide, like ones that only apply to certain kinds of files. The trick is to do it in a way that doesn't confuse the user's muscle memory.)

### Label style

1. **Use title capitalization for menu commands.** Capitalize the first letter of each word, except for prepositions and conjunctions like "and," "in," or "to" (e.g., "Find in Files"). Always capitalize the last word (e.g., "Save As...").
2. **Add an ellipsis to commands that require further input to complete.** In general, this applies to commands that pop up a modal dialog--e.g., "Save As...". However, commands that merely pop up informational dialogs (e.g. "About") should not have an ellipsis.


