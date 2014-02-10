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
* The non-focused view has a small box in the upper right corner showing the name of the file in that view. 
    * **Issue:** Maybe we should only do this for the secondary view, since the main view will always have the pointer from the working set.
* There is a status bar in each view.
* When the view is split vertically, the modal bar and bottom panel always stretch across both views, even in cases where they would only affect the focused view.

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