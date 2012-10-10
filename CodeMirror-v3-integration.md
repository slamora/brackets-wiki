We've started a test integration of CodeMirror v3 into Brackets. You can get this by pulling the `nj/cmv3` branch from Brackets, then doing `git submodule update`, which will switch the CodeMirror2 submodule to use the v3 branch (with some additions).

This page captures the known issues with the integration.

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
