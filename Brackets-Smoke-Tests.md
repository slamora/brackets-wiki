**EARLY DRAFT--not ready for review yet**

Brackets has a pretty good unit test suite that you can run from Debug > Run Unit Tests, but that doesn't always cover issues with the overall UI and integrated functionality, or visual/layout issues that are only obvious if you're actually looking at the product. This is a set of manual tests intended to make sure we haven't broken the basic overall workflows of the product. The intention is to keep it quick--if it takes more than 5 minutes on a given platform it's too long :)

Setup
=====

1. Quit and relaunch Chrome if it's open (so it's *not* in remote debugger mode).
2. If you've run the smokes previously, revert any changes you might have made in `brackets/test/smokes/citrus completed`.

Core functionality
==================

1. Launch Brackets.
2. Open the main brackets source folder if it isn't open already.
3. Expand some folders in the brackets project, enough that it has to scroll.
4. Scroll around in the folder area. Verify that the shadows look right and there are no visual glitches.
5. File > Open Folder and browse to the `brackets/test/smokes/citrus completed` folder (note that there's a space in the name; this is intentional). Verify that its contents look correct, and expand some folders to look inside them.
6. Click on index.html. Verify that the selection in the project panel draws properly.
7. Double-click on index.html. Verify that it's added to the working set and the selection draws properly.
8. Resize the window. Verify that the editor resizes properly and the title bar wraps appropriately.
9. Look through the in-Brackets menus. Verify that they look okay and that they properly pop on top of other UI in the app.
9. Set the cursor in the <body> tag.
10. Hit Cmd-E. Verify that it shows a single body rule and that everything is laid out properly.
11. Click the lightning bolt in the upper right. You should get the dialog saying you need to relaunch Chrome.
12. Click "Relaunch". Chrome should relaunch and open the page.
13. Back in Brackets, edit the background color for the <body> tag in the inline editor (#D90 is a nice color). Verify that the color changes in Chrome as you type. Also verify that the CSS file is added to the working set with the dirty bit set.
14. Hit Cmd-E. Verify that the inline editor closes.
15. Put the cursor immediately after the "<a" in one of the <a> tags in the navbar.
16. Hit Cmd-E. Verify that the inline editor opens and that you see a number of rules in the list on the right.
17. Scroll up and down in the outer editor. Verify that the inline editor scrolls properly with the editor.
18. Resize the window. Verify that the rule list moves properly and there are no visual glitches.
19. Click on a list in the rule list. Verify that the editor shows the correct rule.
20. Quit the app. Verify that you get a "save changes" dialog for any CSS files you edited through the inline editor, and choose to discard the changes.
21. Restart the app. Verify that the "citrus completed" project shows in the sidebar, and that the working set and current editor are showing the same files as when you quit. Also verify that the changes you had previously made were reverted (`git status` in the smokes folder should show clean).

Unofficial features
===================
TODO

1. Edit > Find...
2. Edit > Find in Files...
3. Navigate > Quick Open...

Areas covered
=============
* Project tree - appearance, handling of folders with spaces
* Working set - selection appearance
* File tree - scrolling and selection appearance
* Title bar - resize
* In-browser menu - appearance, Z-order
* Inline editor - open CSS
* Inline editor - resize/scroll behavior
* Inline editor - rule list
* Live development - launch
* Live development - edit CSS
* Find/Replace - appearance/behavior of semimodal dialog
* Find in Files - appearance/behavior of semimodal dialog, panel
* Quick Open - appearance/behavior of semimodal dialog, popup
* Quit the app and restart - project properly restored

**TODO:** Should we add basic file operations (e.g. Save)? Those are obviously key workflows, but we do have unit tests for them.
