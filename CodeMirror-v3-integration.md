We've started a test integration of CodeMirror v3 into Brackets. You can get this by pulling the `nj/cmv3` branch from Brackets, then doing `git submodule update`, which will switch the CodeMirror2 submodule to use the v3 branch (with some additions).

## Known issues with the integration

Issues in **bold** are ones we'd like to investigate further to see where the problem is.

**General bugs:**
* Flickery throw scrolling
    * There's a potential patch for this listed in https://github.com/marijnh/CodeMirror/issues/810, but it breaks horizontal scrolling
* Stuttery throw scrolling
    * This seems worse in brackets than in theme demo in chrome (even when merged with no-flex-box branch), but it's noticeable in vanilla CodeMirror too.
* Fixed gutter moves around while scrolling horizontally (seems to be true only in Brackets, and only for trackpad scrolling; worse when inline editor is open)
    * This _does_ happen in the CodeMirror demos, but it is much harder to reproduce. Should narrow down to specific test case (may require a large CodeMirror area) and file with Marijn.
* delCharLeft isn't implemented, so delete key doesn't work (maybe the function names for some handlers have changed?)

**Missing functionality:**
* No way to turn off fixed gutter

**Inline editor bugs:**
* Cmd-E from within an inline editor (to close it) doesn't work
* Deleting a line doesn't close its attached inline editor
    * I put in code to handle the "delete" event on the line, but it doesn't seem to be working. Not clear if this is our bug or a CodeMirror bug.
* Can get two inline editors on same line (CM doesn't enforce the same rule we did--we'll need to specifically check for this)
* Horizontal scroll area is huge when inline editor is open
    * We were setting the min-width of the inline editor to match the overall width of CodeMirror's linespace, but this doesn't work anymore because the inline editor is actually inside the linespace (and the total width of the editor is wider than the min-width, since we set padding to account for the rule list width). We need to rethink how we're managing the width of the inline editor.
* Scrolling past the right edge of the inline editor with cursor movement doesn't scroll the whole doc (because scrollIntoView() isn't exposed--could we write our own using scrollTo and getScrollInfo?)
* addLineWidget() doesn't have scrollIntoView property
* **Scrolling of rule list lags behind widget**
    * Not sure why this has changed. Scroll events are being sent synchronously from CodeMirror, so it's not clear why this should lag.
    * The lag is less noticeable on a faster machine (Retina MacBook), so it might simply be a performance issue. Perhaps it's related to the extra stuttering in scrolling in v3.
* Motion of rule list when horizontally resizing window lags
* Widget loses focus when scrolled far off the screen
    * The CodeMirror implementation appears to remove widgets from the display list when they scroll off the screen. We'll need to ask Marijn if he'd be willing to add an option to leave certain widgets on the display list (maybe turning off their visibility). If not, we could conceivably live with this since it's kind of an edge case.

**Stuff to test:**
* Middle mouse button

**New things we can take advantage of:**
* setSize() to explicitly set width/height
* getHistory()/setHistory() to get history info
* Multiplexing of event listeners no longer necessary for most events (but still needed for onKeyEvent)

**Other:**
* CM has unused undo()/redo() defs in instance (never called?)

**Fixed bugs**
* Horizontal position of gutter is different. Fixed in [add307e5](https://github.com/adobe/brackets/commit/add307e5f9bda545e1863ac50e52711aa897b7f6)
* Can't horizontally scroll all the way to the right (last few characters are cut off). Fixed in [add307e5](https://github.com/adobe/brackets/commit/add307e5f9bda545e1863ac50e52711aa897b7f6)
* Inline editor doesn't align properly with parent editor (line numbers). Fixed in [add307e5](https://github.com/adobe/brackets/commit/add307e5f9bda545e1863ac50e52711aa897b7f6)
* After opening one editor, opening another one on a different line doesn't open, or opens one on a line other than the one the cursor is in. Fixed in [05e8aa](https://github.com/adobe/brackets/commit/05e8aa684fb54947fbba74ffe8918dde6444662d)
* Code in inline editor overlaps on top of rule list. Fixed in [3d908d](https://github.com/adobe/brackets/commit/3d908d644d1ba56e8bb492ea4a54bd6303ed9b9b)

## Porting changes from our fork of CodeMirror

The brackets fork of CodeMirror has a bunch of changes that were _not_ integrated into the upstream branch. Most of the changes were dealing with inline widgets, but there were several other small features and performance improvements. 

The inline widget changes need to be re-implemented on top of the new line widget API added to CodeMirror 3. A first pass at this has been done in the `nj/cmv3` branch, but it has the issues noted above.

Here are notes on the other changes that were made.

### Functionality that does NOT need to be ported

#### Handle hidden lines in coordsChar() - ([commit 11f204](https://github.com/adobe/CodeMirror2/commit/11f2041529d798f9a916be2f146edf165c2b5434))

This fixed performance problems when the contents of an inline editor were near the top of a very large file. For example, the `body` selector in bootstrap.css. The v3 branch of CodeMirror does not have the same performance problems, so this change doesn't need to be ported.

#### Use document fragments in patchDisplay() - ([commit cf24f7](https://github.com/adobe/CodeMirror2/commit/cf24f7240b3bf7edb63cf9f4a625f5c16dff7620))
The v3 branch contains a more comprehensive fix for using document fragments, obviating our changes.

#### Mouse handling for drag selecting and autoscrolling across inline editors
The v3 branch seems to handle this pretty well.

#### Changes to updateGutter()
These are no longer necessary with the new inline editor implementation. They were only required because we were injecting dummy nodes into the gutter.

### Functionality that needs to be ported

Most of these changes should be trivial to port, as long as Marijn is okay accepting them upstream.

#### Dirty bit handling (`isDirty()`, `markClean()`)
This has already been ported in our branch of v3 ([commit b0df09](https://github.com/adobe/CodeMirror2/commit/b0df0929bb0e150390012e1de346730af9a5d9bb)). This will need to be submitted upstream.

#### Exposing CodeMirror methods
We exposed the CodeMirror methods `selectWordAt()` and `scrollIntoView()` for use in Brackets. We'll need to check with Marijn to see if he's okay with exposing these in master.

#### `totalHeight()`
We added this function in order to determine how tall an inline editor should be to show all of its content. We might be able to just get this by letting CodeMirror lay itself out and then measuring its height, but I believe there were reasons why that didn't work before. If we can't get that to work, then we'll need to port this, which should be easy.

#### `preserveScrollPosOnSelChange`
This makes it so that when you do Cmd/Ctrl-A to select all, the editor doesn't scroll. This should be easy to port.

#### Right-click fix to `onMouseDown()`
This needs to be ported, as it prevents our context menus from working.

#### Checking `isInWidget()` in various cases and returning early
We will likely need to port these so that CodeMirror doesn't handle things like context menus or double-click in widgets.

### Changes to htmlmixed mode
If we encounter a `<script>` tag in HTML that seems to be a Mustache template, we treat it as HTML, and if we don't recognize it, we leave it uncolored instead of attempting to color it as javascript. We should submit this upstream.

## Forking strategy

Currently, the `master` branch in adobe/CodeMirror2 is completely out of sync with the upstream CodeMirror master, because we actually checked all of our changes directly into our master. In hindsight, that wasn't a good idea :)

For now, since all the v3 work is in a separate branch in the CodeMirror repo, we don't have to worry about our master; we can simply point the submodule SHA in Brackets to the v3 branch. However, once Marijn merges v3 into CodeMirror master, we'll need to bring our master back into sync.

Our proposal is to hard reset our master to the upstream CodeMirror master at that point. Our assumption is that there are no (or very few) people working on the adobe/CodeMirror2 fork, so this shouldn't disrupt anything. Once we've done that, we will never make commits directly to master in our fork; we'll either submit patches directly to Marijn and wait for them to be merged upstream, or we'll create a separate Brackets-specific branch and point our submodule SHA to that branch.

Here's the proposal in more detail:

### Before v3 is merged into upstream master

1. We will pull the `v3` branch from upstream into adobe/CodeMirror2 and keep it in sync with upstream.
2. We will have a separate branch in adobe/CodeMirror2 that contains any local changes we need for now as we're investigating the integration. (Currently, this branch is `nj/cmv3-brackets`).
3. If we finish our integration before v3 officially gets merged into upstream master, we'll keep this separate branch in adobe/CodeMirror2, and point the main Brackets submodule SHA at this branch.

### After v3 is merged into upstream master

1. We will rename adobe/CodeMirror2 to adobe/CodeMirror (and update the submodule info in Brackets) to stay in sync with Marijn's repo name.
2. On our master, we'll create a `brackets-old` branch based on the current state of our master.
3. We'll then do `git reset --hard upstream/master` in order to point our master back at upstream. We will never commit directly to our master after this.
4. Anyone else who has actually made unsubmitted changes in their fork of adobe/CodeMirror2 will similarly need to reset their master branch to ours, and deal with merging any changes they have into a branch based on the new master.
5. Since there will be some functionality in our old CodeMirror branch that will need to be ported over to v3, we'll create a separate `brackets` branch in adobe/CodeMirror to hold this ported functionality, and point the Brackets submodule at that branch.
6. We'll keep the `brackets` branch as clean as possible, rebasing as necessary to make it so each commit can be submitted directly to CodeMirror upstream as a pull request.
7. Over time, we will aim to get everything from the `brackets` branch into CodeMirror upstream, and then just point Brackets at the master branch of adobe/CodeMirror. We'll only revive the separate branch if we need to implement functionality that will take awhile to get accepted upstream, but we'll always try to get it pushed upstream as soon as possible.