We've started a test integration of CodeMirror v3 into Brackets. You can get this by pulling the `nj/cmv3` branch from Brackets, then doing `git submodule update`, which will switch the CodeMirror2 submodule to use the v3 branch (with some additions).

## Known issues with the integration

**General bugs:**
* Flickery throw scrolling - fixed with patch from issue #810Stuttery throw scrolling - this seems much worse in brackets than in theme demo in chrome (even when merged with no-flex-box branch)
* Horizontal position of gutter is different -- possibly due to the way we set padding?
* Can't horizontally scroll all the way to the right (last few characters are cut off) -- ditto?
* Fixed gutter moves around while scrolling horizontally (seems to be true only in Brackets, and only for trackpad scrolling; worse when inline editor is open)
* delCharLeft isn't implemented, so delete key doesn't work (maybe the function names for some handlers have changed?)

**Missing functionality:**
* Don't currently have a way to measure the full visible height of a document (totalHeight()) for inline editor
* No way to turn off fixed gutter

**Inline editor bugs:**
* After opening one editor, opening another one on a different line doesn't open, or opens one on a line other than the one the cursor is in--eventually works after fiddling around
* Cmd-E from within an inline editor (to close it) doesn't work
* Inline editor doesn't align properly with parent editor (line numbers) -- ditto?
* Can get two inline editors on same line (CM doesn't enforce the same rule we did--we'll need to specifically check for this)
* Typing in inline editor is very very slow (not as bad when merged with no-flex-box branch)
* Code in inline editor overlaps on top of rule list
* Horizontal scroll area is huge when inline editor is open
* Scrolling past the right edge of the inline editor with cursor movement doesn't scroll the whole doc (because scrollIntoView() isn't exposed--could we write our own using scrollTo and getScrollInfo?)
* addLineWidget() doesn't have scrollIntoView property
* Scrolling of rule list lags behind widget
* Motion of rule list when horizontally resizing window lags
* Widget loses focus when scrolled off (probably gets removed from display list)

**Stuff that seems to work:**
* Drag auto-scrolling across an inline editor seems to be pretty good

**Stuff to test:**
* Middle mouse button

**New things we can take advantage of:**
* setSize() to explicitly set width/height
* getHistory()/setHistory() to get history info
* Multiplexing of event listeners no longer necessary for most events (but still needed for onKeyEvent)

**Other:**
* CM has unused undo()/redo() defs in instance (never called?)

## Porting changes from our fork of CodeMirror

The brackets fork of CodeMirror has a bunch of changes that were _not_ integrated into the upstream branch. Most of the changes were dealing with inline widgets, but there were several other small features and performance improvements. 

The inline widget changes need to be re-implemented on top of the new line widget API added to CodeMirror 3. Details  Here are notes on the other changes that were made.

### Performance Improvements that do NOT need to be ported

#### Handle hidden lines in coordsChar() - ([commit 11f204](https://github.com/adobe/CodeMirror2/commit/11f2041529d798f9a916be2f146edf165c2b5434))

This fixed performance problems when the contents of an inline editor were near the top of a very large file. For example, the `body` selector in bootstrap.css. The v3 branch of CodeMirror does not have the same performance problems, so this change doesn't need to be ported.

#### Use document fragments in patchDisplay() - ([commit cf24f7](https://github.com/adobe/CodeMirror2/commit/cf24f7240b3bf7edb63cf9f4a625f5c16dff7620))
The v3 branch contains a more comprehensive fix for using document fragments, obviating our changes.


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
7. Over time, we will aim to get everything from the `brackets` branch into CodeMirror upstream, and then just point Brackets at the master branch of adobe/CodeMirror.
