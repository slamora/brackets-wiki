## General rules

The default behavior for most operations should be:

1. For non-line-oriented operations, act as if each selection/cursor placement were made independently: i.e., pretend that the user made the first selection as a single selection, did the operation, made the second selection, as a single selection, did the operation again, etc.
2. For line-oriented operations (e.g. Duplicate Line, Indent), where the exact location of the selection on the line isn't meaningful, do (1), but only operate on a given line once. (i.e., if there are two selections on one line and you do Duplicate Line, only duplicate the line once.)
3. For operations that really only make sense to operate on one selection, or if one selection needs to be designated as special (e.g. when opening multiple inline editors, which one gets the focus), the last selection made should be considered primary (this matches the default in CodeMirror's API).

In the table below, operations listed as "default" follow the rules above.

## Specific behaviors

Anything not marked "**Not working**" appears to be working as desired in the current CMv4 branch.

**TODO**:
* behavior around hard tabs seems incorrect compared to Sublime
* there seem to be other bugs around hard tabs in the Brackets cmv4 branch (e.g. hitting delete at the end of a line with a hard tab)

Operation | Desired Behavior | Notes
----------|------------------|-------
**Making selections** | |
Multiple cursors (contiguous) | Hold down Alt, then click and drag vertically | **If you drag across lines that don't reach horizontally to your location, Sublime and Ace will add a cursor on those lines, whereas CM won't. [Filed in CM.](https://github.com/marijnh/CodeMirror/issues/2177)<br><br>On Windows, Sublime uses Shift-RMB or MMB, not Alt. Ace is the same as CM. I suspect we shouldn't use MMB (because it means scroll in the browser); do we want to push for Shift-RMB?**<br><br>Note: Sublime and Ace allow Cmd-Alt as well as Alt. Doesn't seem critical to me.
Multiple cursors (discontiguous) | Click first cursor, then Cmd/Ctrl-click to set other cursors |
Multiple selections (contiguous - rectangular) | Hold down Alt, then click and drag a rectangle | **See note for multiple cursors above.** For rectangular selections (as opposed to cursors) Sublime, Ace and CM are consistent - selections are not added on lines that don't reach to the left edge of the rectangle.
Multiple selections (discontiguous) | Make first selection, then Cmd/Ctrl-click and drag to set other selections |
**Cursor navigation** | |
Arrow key (multiple cursors only) | Default |
Arrow key (multiple selections) | Collapse selection in direction of arrow | Note: In the case of up/down arrow, Ace and Sublime also apply the navigation (even in the single selection case), whereas CM just collapses the selection. I think keeping the CM behavior is fine.
Shift-Cmd-arrow key (Mac)/Shift-Alt-arrow key (Win) | Default (extend each selection to end of its line) | Note: on Win, Sublime maps Alt-arrow to move by word, not end of line
Shift-Alt-arrow key (Mac)/Shift-Ctrl-arrow key (Mac) | Default (extend each selection by word; merge selections that collide) |
Shift-arrow key | Extend selection in direction of arrow; merge selections that collide |
Page up/down | Do page up/down and collapse selection to single cursor | Note: on Win, CMv4 currently collapses selection only, but this is true for single select as well. **(Check if this is a regression from CMv3; other page up/down behavior seems wonky on Windows wrt cursor placement.)**
Select Line | Default | **Not working**
**Edits** | |
Typing: multiple cursors/selections | Default (delete selection if any, then insert typed char) |
Typing: combination of cursors and selections | Default |
Delete: multiple cursors | Default (delete char before each cursor) | Edge cases occur when deletions can cause cursors to collide. Should test to make sure there's no bad behavior.
Delete: multiple selections | Default (delete content and collapse each selection to cursor) | Edge cases occur when deletions can cause cursors to collide. Should test to make sure there's no bad behavior.
Delete: combination of cursors and selections | Only delete selections - no operation on cursors. |
Tab/Indent | Default | Note that Sublime doesn't do "smart indent" as we do.
Shift-Tab/Unindent | Default |
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
* Edits on multiple selections/cursors within inline editors: There appear to be bugs with deleting content or adding newlines at the beginning/end of inline editors in cmv4 even in single-selection, so that would need to be investigated first before understanding whether multiple selections are working properly.
* Edits on multiple selections/cursors in live HTML: Seems to work fine for basic cases. I would guess there might be performance issues for lots of changes at once, but that doesn't seem like a big deal. Also, if there's a syntax error we only show the first one, but that seems fine.
* Edits on multiple selections/cursors in live CSS: Seems to work fine for basic cases.

## Work we likely need to do

* Add API functions to Editor
* Implement any behavior above that's not implemented - lots of cases but mostly straightforward
* Create new unit tests for multiple cursor/selection cases for all appropriate functionality
* Fix existing unit tests that are broken with v4
* Scenario testing
* File bugs against popular extensions about multi-selection support
