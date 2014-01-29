## General rules

In the following rules, "selection" refers to both cursors and range selections.

The default behavior for most operations should be:

1. For non-line-oriented operations:
    * Do a batch operation that contains the result of acting on each selection independently.
    * Track each selection through the edit (so that if the edit to one of the selections would change the selection, that new selection is substituted for the original in the resulting multiple selection).
    * If some of the resulting selections overlap, merge them.
2. For line-oriented operations (e.g. Duplicate Line, Indent), where the exact location of the selection on the line isn't meaningful:
    * Operate once on each line that intersects any of the selections (i.e., don't operate on a line twice even if there are two selections within the line).
    * But still track each selection through the edit as if the selections were made independently (i.e., if there are two selections within the line, the resulting selection should contain updated locations for both of them). 
    * As in (1), if some of the resulting selections overlap, merge them.
3. For operations that really only make sense to operate on one selection, or if one selection needs to be designated as special (e.g. when opening multiple inline editors, which one gets the focus), the last selection made should be considered primary (this matches the default in CodeMirror's API).

In the table below, operations listed as "default" follow the rules above.

## Specific behaviors

Anything not marked "**Not working**" appears to be working as desired in the current CMv4 branch.

Operation | Desired Behavior | Notes
----------|------------------|-------
**Making selections** | |
Multiple cursors (contiguous) | Hold down Alt, then click and drag vertically | **On Windows, Sublime uses Shift-RMB or MMB, not Alt. Ace is the same as CM. I suspect we shouldn't use MMB (because it means scroll in the browser); do we want to push for Shift-RMB?**<br><br>Note: Sublime and Ace allow Cmd-Alt as well as Alt. Doesn't seem critical to me.
Multiple cursors (discontiguous) | Click first cursor, then Cmd/Ctrl-click to set other cursors |
Multiple selections (contiguous - rectangular) | Hold down Alt, then click and drag a rectangle | **See note for multiple cursors above.** For rectangular selections (as opposed to cursors) Sublime, Ace and CM are consistent - selections are not added on lines that don't reach to the left edge of the rectangle.
Multiple selections (discontiguous) | Make first selection, then Cmd/Ctrl-click and drag to set other selections |
? | Add Line to Selection - adds the line above or below as another selection | **Not working** - **need to pick keyboard shortcuts for this**; Sublime's shortcuts seem to conflict with OS X Expose shortcuts - **this might be lower priority**
Ctrl/Cmd-Shift-L | Split into Lines - takes a block of lines and splits it into individual selections, one per line | **Not working** - **this might be lower priority**
? | Quick Add Next - if word not selected, expands selection to word; if word selected, adds next occurrence of word as another selection | **Not working** - seems popular. - **need to pick shortcut**; Sublime uses Cmd-D, which conflicts with our Duplicate
Alt-F3 / Cmd-Ctrl-G | Find All - selects current word, then adds selections for each match in the current document | **Not working**
Ctrl/Cmd-U | Undo Selection - undoes the last selection change (for both single and multiple selections) | **Not working**, probably important for the workflow with Cmd-D. Note that it appears to be a superset of ordinary undo (i.e., if there was a doc edit in between selection changes, it will undo that edit).
Esc | Switch to single selection | **Not working** - appears to be implemented in CM but we intercept Esc in Brackets. Also, need to double-check which selection it collapses to - Sublime seems to collapse to the first selection, not the last one.
**Cursor navigation** | |
Arrow key (multiple cursors only) | Default |
Arrow key (multiple selections) | Collapse selection in direction of arrow | Note: In the case of up/down arrow, Ace and Sublime also apply the navigation (even in the single selection case), whereas CM just collapses the selection. I think keeping the CM behavior is fine.
Shift-arrow key | Default (extend each selection in direction of arrow; merge selections that collide) |
Shift-Cmd-arrow key (Mac)/Shift-Alt-arrow key, Shift-Home/End (Win) | Default (extend each selection to end of its line) | Note: on Win, Sublime maps Alt-arrow to move by word, not end of line
Shift-Alt-arrow key (Mac)/Shift-Ctrl-arrow key (Win) | Default (extend each selection by word; merge selections that collide) |
Page up/down | Do page up/down and collapse selection to single cursor | Note: on Win, CMv4 currently collapses selection only, but this is true for single select as well. **(Check if this is a regression from CMv3; other page up/down behavior seems wonky on Windows wrt cursor placement.)**
Select Line | Default | **Not working**
**Edits** | |
Typing: multiple cursors/selections | Default (delete selection if any, then insert typed char) |
Typing: combination of cursors and selections | Default |
Delete: multiple cursors | Default (delete char before each cursor) | Edge cases occur when deletions can cause cursors to collide. Should test to make sure there's no bad behavior.
Delete: multiple selections | Default (delete content and collapse each selection to cursor) | Edge cases occur when deletions can cause cursors to collide. Should test to make sure there's no bad behavior.
Delete: combination of cursors and selections | Only delete selections - no operation on cursors. |
Tab/Shift-Tab | Default | Note that Sublime doesn't do "smart indent" as we do.
Indent/Unindent | Default |
Copy | Concatenate each selection, separated by newlines, and copy to clipboard. Cursors count as empty selections, so add an extra newline. | This is the current cmv4 behavior. **Sublime ignores cursors in this case (extra newlines are not generated).** Ace's behavior is like CM's.
Cut | Like copy, but delete afterwards. | Note that in Sublime, if there are cursors in the selection, the lines containing those cursors are deleted, because that's the behavior in the single selection case. Brackets doesn't do that, so it makes sense to stay consistent with ourselves. Ace's behavior is like CM's.
Paste | Default (paste the clipboard into each selection/cursor, deleting selections first, and leaving cursor after pasted content) |
Duplicate Line | Default | **Not working**
Delete Line | Default | **Not working**
Move Line Up/Down | Default (except that contiguous lines should move together) | **Not working**
Toggle Line Comment | Default | **Not working**
Toggle Block Comment | Default | **Not working**
**Other gestures** | |
Quick Edit/Quick Docs | Default (i.e., open on each selection); set focus to inline editor for last selection | **Not working**
Find (what should be in the query field) | Contents of the last (primary) selection | **Not working**. Ace's behavior matches this. Note: Sublime doesn't copy the selection to the query string on Cmd-F, but it does do so on Cmd-E, and it uses the contents of the last selection.
Find Next/Find Previous (where should they start from) | Start from the last (primary) selection | **Not working**. Ace's behavior matches this. Note: Sublime seems to start from the selection farthest along in the document, but it seems more consistent to start from the last selection made.
Live Preview Highlight in HTML/CSS (do we highlight all selections?) | Default (highlight all items containing any selection) | **Not working**.
Highlight Active Line (for multiple cursors) | Default (highlight any line that contains a cursor) |

## Other functionality to test

* Unit tests: lots of breakage :)
* Selections around hard tabs - CM currently doesn't match Sublime's behavior; it treats a hard tab as if it were a single character, so selections are no longer visually rectangular to the right of a hard tab. Filed as https://github.com/marijnh/CodeMirror/issues/2189.
* Edits on multiple selections/cursors within inline editors: There appear to be bugs with deleting content or adding newlines at the beginning/end of inline editors in cmv4 even in single-selection, so that would need to be investigated first before understanding whether multiple selections are working properly.
* Edits on multiple selections/cursors in live HTML: Seems to work fine for basic cases. I would guess there might be performance issues for lots of changes at once, but that doesn't seem like a big deal. Also, if there's a syntax error we only show the first one, but that seems fine.
* Edits on multiple selections/cursors in live CSS: Seems to work fine for basic cases.
* Need to finish and merge https://github.com/adobe/brackets/pull/6053 which fixes a number of issues where we were modifying the cursor returned by CM.

## Work we likely need to do

* Add API functions to Editor
* Implement any behavior above that's not implemented - lots of cases but mostly straightforward
* Create new unit tests for multiple cursor/selection cases for all appropriate functionality
* Fix existing unit tests that are broken with v4
* Scenario testing
* File bugs against popular extensions about multi-selection support
