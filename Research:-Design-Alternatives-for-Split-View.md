This is an early draft of a description of several alternatives for split view that are being considered.

## New Alternative Split View

![New Alternative Split View mockup](https://trello-attachments.s3.amazonaws.com/4f90a6d98f77505d7940ce88/4fb6c9c75b54696e60026473/1540x3044/980d341b509e0a20f1346254933d4f1e/SplitView_Global_001.png)
This is a more intuitive model:

* The split view icon next to the Working Files label can be toggled to split the editor into two columns
* When the editor is split into two columns the columns will have column headers that contain filenames, or an instruction if there's no file in the column. Focused column has black header text whereas the unfocused column has gray header text. Also, there will be two Working Files sections called "Left" and "Right" which are self-explanatory.
* Blue filenames under Working Files are the files being show in the columns. There can only be one blue filename per working files section. The file that's currently in focus will have the dark selection background; we should consider removing the triangle that sticks out to avoid confusion.

[Comments from nj]
* I think this is more or less the same as the "Split Icons Separate from Working Set" idea at the bottom, but with the icon in the working set area instead of in the toolbar. In particular, opening a file opens it in the currently focused pane.
* I think we were planning on doing both horizontal and vertical splitting in the initial implementation, so the icon will probably need to be a dropdown that lets you pick which way you want to split the window (and also gives you the option to unsplit). If the window is already split one way and you choose to split it the other way, the panes should just move around and the labels of the working set sections should change to match.
* I think it would be good to consider adding some other subtle header decoration to emphasize which is the focused pane in addition to just the black/gray text.
* We should note that we have discussed adding tabs for documents, but are not planning to do that right now - we might add them later.
* One question is what happens if the user single-clicks in the file tree. Currently, that doesn't add the file to the working set, but presumably the user would expect it to open in the focused pane. I'm assuming that the visual treatment will be the same as in the working set - i.e., if the focused pane contains a file from the file tree that's not in the working set, then it will get the black selection highlight in the file tree; if the unfocused pane contains a file from the file tree, then it will get the blue text highlight.
* We also need to be clear about what Ctrl-Tab does, since it cycles in MRU order. I'm assuming it would do the same in the new world, and that MRU is simply tracked across both working sets.
* I also wonder if we should add another shortcut for just cycling focus between the panes.
* Also need to figure out how the working set gear menu and context menu commands work with multiple working sets. "Sort" probably just sorts within each set; the "Close Others" commands are less clear (do they mean just in the working set containing the file you opened the context menu on?)
* Also need to figure out how drag and drop will work with multiple working sets (I think the intention is that you could drag and drop between the working sets in addition to reordering within the same set).

## Split Icons in Working Set

![Split View mockup with icons in working set](https://trello-attachments.s3.amazonaws.com/4f90a6d98f77505d7940ce88/4fb6c9c75b54696e60026473/f5ab882a6ee322d3bf664c7de98c00d5/SplitView_001.png)

In this model, there's a "main" view (the one on the left or top) and a "secondary" view (on the right or bottom) that is created by clicking on a split icon in the working set.
* On hover of any item in the working set, two icons appear. Clicking on the icon on the left opens that item in a bottom view. Clicking on the one on the right opens the item in a right-side view.
    * This show up in the file tree as well. If the icons too annoying on hover then we should turn them into context menu items.
    * This works for the currently-selected file too, so you can have the same file open in both views.
    * The file that's open in the secondary view gets the same selection background as the main file, but without the pointer. 
        * It also shows both split icons persistently (instead of on hover), so you can unclick the selected icon to close the split view, or click on the other icon to switch the orientation of the split view.
        * When the split is at the bottom, the pointer could be misleading in edge cases where you increase the bottom splitter height to the point where the top of the bottom view is next to the working set so we should remove the arrow. The bottom panel icon will be blue and persistent beside the file name so hopefully it won’t be that misleading.
        * The split icons show up on hover of other items in the working set while a split is already open so you can replace the current split panel with a new one. Basically there can only be one split icon that is active (blue).
    * You can also close the split view by doing File > Close while focus is in one of the views. If you do this in the main view, the secondary view becomes the main view.
* When clicking on another file in the working set or file tree, the file is opened in left or right pane depending on where the user is clicking, file name or split icon. A "file click" should always open the file in the main view (e.g. left side) and split icon click should always open the file in the secondary view (e.g. right side).
    * Exception: before we make it possible to view the same doc in two views, selecting a file already open just switches focus to that view.
        * **Issue:** would we always want to do this? Might want to at least keep this behavior for non-sidebar opens. For sidebar opens, in this model (sidebar icons), it might work to keep this behavior for the working set as well.
    * Same applies to any other UI action that opens a file (e.g. click on a result from Find in Files) - goes to last active view (need to ensure that per-doc bottom panels always properly hide/switch if you switch views)
    * We could use good panel titles to let people know what’s what so that it won't be confusing that some bottom panels are global vs. per doc.
* The non-focused view has a small box in the upper right corner showing the name of the file in that view. 
    * Persistent active split icon beside the file should make it clear what the right pane contains.
* Status bar is full width (not split as in mockup above), to avoid issues with bottom panels having to go above them, and with some things in the status bar not being file-specific.
* **Issue:** Other questions
    * How does Ctrl-Tab work? This is tricky, but since people ctrl-tab in order to cycle through working files then I guess we can keep the current behavior because primary view is the lowest common denominator. 
    * Multiple working sets?
    * How does resizing the window affect the sizes of the split views? Ideally the panes should be resizable, with a fixed primary-view width. If not then split it 50/50.

### Pros

* Opening a split view on a particular file is a single click.
* Immediately discoverable.

### Cons

* Icons overlap filenames, reducing the hit area for selecting a file.
* The behavior for the icons seems a bit complicated/confusing.

## Split Icons Separate from Working Set

No mockup yet, but the idea is that we would have a separate "split view" affordance in either the status bar area or the toolbar. This could be a button bar with three toggles (no split, split vertically, split horizontally) or a single button with a dropdown affordance.

* Clicking on the split view affordance splits the view and shows the current document in both views (or closes the secondary view if you pick the "no split" icon).
    * **Issue:** Or it might be clearer to have the secondary view be blank, but focused, until you choose another file from the working set. The view could have a message like "Click on a file to open it in this view".
* All the other rules (file selection, closing a file, etc.) not related to the split icon state are as in the previous proposal.

## Pros

* Seems cleaner to separate window splitting from file selection.
* Icons don't overlap filenames.

## Cons

* "Blank view" state might be odd.
* Might be less immediately discoverable.

# Rough task breakdown

* (M) editor area management
    * creating another editor within editor area
    * sizing/layout
    * allowing user to resize
    * persist view state in prefs
* (M) creating split view
    * either: handle icons in working set *or* create split UI in toolbar/status bar
    * menu commands
* (S) managing split view documents
    * handling working set/file tree selections while view is split
    * handling File > Close while view is split
* (M) slop time
    * changing UI if there are usability issues with whichever way we go initially
    * possible performance issues with two large CMs open at once
* (M) handle image preview in split views
* (M) allow same doc in two views
    * doc-linking (enables showing same doc in two views)
    * manage Document._masterEditor (might have to juggle this, might go away with doc linking)
    * might have to do work to make scroll positions of both views feel independent when edits are made in one (might also go away with doc linking)