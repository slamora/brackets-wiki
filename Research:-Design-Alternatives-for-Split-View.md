This is an early draft of a description of several alternatives for split view that are being considered.

## New Alternative Split View

![New Alternative Split View mockup](https://trello-attachments.s3.amazonaws.com/4f90a6d98f77505d7940ce88/4f99afca0c8915c46e1bd5b1/1540x3044/449a434ce6e0734d24003e4675531a55/SplitView_Global_001.png)

This is a more intuitive model:

* The split view icon next to the Working Files label can be toggled to split the editor into two columns
* When the editor is split into two columns the columns will have column headers that contain filenames, or an instruction if there's no file in the column. Focused column has black header text whereas the unfocused column has gray header text. Also, an extra section will appear below Working Files called Split Pane for now (name TBD), this section is for the working files designated for the second column.
* Blue filenames under Working Files are the files being show in the columns. There can't be more than two blue filenames. The file that's currently in focus will have the dark selection background; we should consider removing the triangle that sticks out to avoid confusion.




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