Brackets has a pretty good unit test suite that you can run from Debug > Run Unit Tests, but that doesn't always cover issues with the overall UI and integrated functionality, or visual/layout issues that are only obvious if you're actually looking at the product. This is a set of manual tests intended to make sure we haven't broken the basic overall workflows of the product. The intention is to keep it quick--if it takes more than 5 minutes on a given platform it's too long :)

Setup
=====

1. Quit and relaunch Chrome if it's open (so it's *not* in remote debugger mode).
2. If you've run the smokes previously, revert any changes you might have made in `brackets/test/smokes/citrus completed`.
3. Delete your cache folder (~/Library/ApplicationSupport/com.adobe.Brackets.cefCache on Mac, ...\AppData\Roaming\Brackets on Win).

Core functionality
==================

1. Launch Brackets. Verify that the brackets "src" folder is visible in the project panel.
2. Expand some folders in the brackets project, enough that it has to scroll.
3. Scroll around in the folder area. Verify that the shadows look right and there are no visual glitches.
4. File > Open Folder and browse to the `brackets/test/smokes/citrus completed` folder (note that there's a space in the name; this is intentional). Verify that it contains "css" and "images" folders and an "index.html" file.
5. Click on index.html. Verify that the selection in the project panel draws properly.
6. Double-click on index.html. Verify that it's added to the working set and the selection draws properly.
7. Resize the window. Verify that the editor resizes properly and the title bar wraps appropriately.
8. Look through the in-Brackets menus. Verify that they look okay and that they properly pop on top of other UI in the app.
9. Set the cursor in the `<body>` tag.
10. Hit Cmd/Ctrl-E. Verify that it shows a single body rule and that everything is laid out properly.
11. Click the lightning bolt in the upper right. You should see the page load in Chrome. On the Mac, after a few seconds you should get a dialog saying you need to relaunch Chrome.
12. (Mac only) Click "Relaunch". Chrome should relaunch and open the page.
13. Back in Brackets, edit the background color for the <body> tag in the inline editor (#D90 is a nice color). Verify that the color changes in Chrome as you type. Also verify that the CSS file is added to the working set with the dirty bit set.
14. Hit Cmd/Ctrl-E. Verify that the inline editor closes.
15. Put the cursor immediately after the `<a` in one of the `<a>` tags in the navbar.
16. Hit Cmd/Ctrl-E. Verify that the inline editor opens and that you see a number of rules in the list on the right.
17. Scroll up and down in the outer editor. Verify that the inline editor scrolls properly with the editor.
18. Resize the window. Verify that the rule list moves properly and there are no visual glitches.
19. Click on a list in the rule list. Verify that the editor shows the correct rule.
20. Quit the app. Verify that you get a "save changes" dialog for any CSS files you edited through the inline editor, and choose to discard the changes.
21. Restart the app. Verify that the "citrus completed" project shows in the sidebar, and that the working set and current editor are showing the same files as when you quit. Also verify that the changes you had previously made were reverted (`git status` in the smokes folder should show clean).

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

Not yet covered
===============

* Find in Files - appearance/behavior of semimodal dialog, panel
* Quick Open - appearance/behavior of semimodal dialog, popup
* Quit the app and restart - project properly restored

**TODO:** Should we add basic file operations (e.g. Save)? Those are obviously key workflows, but we do have unit tests for them.

**TODO:** Should we add smoke tests for unofficial features (find/replace, find in files, quick open), or wait till they become official?
