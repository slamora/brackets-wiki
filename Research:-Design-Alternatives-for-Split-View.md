This is an early draft of a description of several alternatives for split view that are being considered.

## Split Icons in Working Set

![Split View mockup with icons in working set](https://trello-attachments.s3.amazonaws.com/4f90a6d98f77505d7940ce88/4fb6c9c75b54696e60026473/f5ab882a6ee322d3bf664c7de98c00d5/SplitView_001.png)

In this model, there's a "main" view (the one on the left or top) and a "secondary" view (on the right or bottom) that is created by clicking on a split icon in the working set.
* On hover of any item in the working set, two icons appear. Clicking on the icon on the left opens that item in a bottom view. Clicking on the one on the right opens the item in a right-side view.
    * **Issue:** Does this show up in the file tree as well?
    * This works for the currently-selected file too, so you can have the same file open in both views.
    * The file that's open in the secondary view gets the same selection background as the main file, but without the pointer. 
        * It also shows both split icons persistently (instead of on hover), so you can unclick the selected icon to close the split view, or click on the other icon to switch the orientation of the split view.
        * **Issue:** When the split is at the bottom, the pointer could be misleading in edge cases where you increase the bottom splitter height to the point where the top of the bottom view is next to the working set.
        * **Issue:** Does this mean that the split icons no longer show up on hover of other items int he working set while a split is already open?
    * You can also close the split view by doing File > Close while focus is in one of the views. If you do this in the main view, the secondary view becomes the main view.
* When clicking on another file in the working set or file tree, the file is opened in whichever view is currently focused, and the selection highlight is updated accordingly.
    * Exception: before we make it possible to view the same doc in two views, selecting a file already open just switches focus to that view.
        * **Issue:** would we always want to do this? Might want to at least keep this behavior for non-sidebar opens. For sidebar opens, in this model (sidebar icons), it might work to keep this behavior for the working set as well.
    * Same applies to any other UI action that opens a file (e.g. click on a result from Find in Files) - goes to last active view (need to ensure that per-doc bottom panels always properly hide/switch if you switch views)
    * **Issue:** Will it be confusing that some bottom panels are global vs. per doc?
* The non-focused view has a small box in the upper right corner showing the name of the file in that view. 
    * **Issue:** Maybe we should only do this for the secondary view, since the main view will always have the pointer from the working set.
* Status bar is full width (not split as in mockup above), to avoid issues with bottom panels having to go above them, and with some things in the status bar not being file-specific.
* **Issue:** Other questions
    * How does Ctrl-Tab work? 
    * Multiple working sets?
    * How does resizing the window affect the sizes of the split views

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
