Brackets has a pretty good automated test suite that you can run from Debug > Run Tests, but that doesn't always cover issues with the overall UI and integrated functionality, or visual/layout issues that are only obvious if you're actually looking at the product. This is a set of manual tests intended to make sure we haven't broken the basic overall workflows of the product. The intention is to keep it quick--if it takes more than 5 minutes on a given platform it's too long.

There are also [Brackets server smoke tests](Brackets-Server-Smoke-Tests).

If you have trouble running through it or something is unclear, please post to the [brackets-dev mailing list](http://groups.google.com/group/brackets-dev).

Setup
=====

1. Make sure your ```git status``` is clean **and** your user extensions folder and `src/extensions/dev` folders are both empty (git status will _not_ tell you about these folders).
2. Quit and relaunch Chrome if it's open (so it's *not* in remote debugger mode).
3. If you've run the smokes previously, revert any changes you might have made in `brackets/test/smokes/citrus completed`.
4. Delete your [cache folder](Cache-Folder) (Mac:  `~/Library/Application Support/Brackets/cef_data`, Win: `%appdata%\Brackets\cef_data`).

Smoke test steps
================

1. Launch Brackets. Verify that the Brackets "Getting Started" folder is visible in the project panel and its index.html file is opened automatically.
2. File > Open Folder and browse to the Brackets source folder.
3. Click on the double-triangle next to the project name. The dropdown should show the "Getting Started" folder and an "Open Folder..." option
4. Switch back to the "Getting Started" folder using the project dropdown, verifying that it switches back to the previous project and shows its index.html.
5. Switch back to the "brackets" folder using the project dropdown.
6. Expand some folders in the brackets project, enough that it has to scroll.
7. Scroll around in the folder area. Verify that the shadows look right (appears at top when not scrolled all the way to the top) and there are no visual glitches.
8. File > Open Folder and browse to the `brackets/test/smokes/citrus completed` folder (note that there's a space in the name; this is intentional) and click OK. In the Project panel, verify that it contains "css" and "images" folders and an "index.html" file.
9. Click on index.html. Verify that the selection in the project panel draws properly.
10. Double-click on index.html. Verify that it's added to the working set and the selection draws properly.
11. Open the File menu and verify the "Live Highlight" menu item is disabled and checked.
12. Resize the window. Verify that the editor resizes properly.
13. Move mouse over the name of the source of an `<img>` tag (e.g. "images/events.jpg" on line 33). Verify that Hover Preview of the image is displayed properly with the width and height.
14. Set the cursor in the `<body>` tag immediately before the `>`.
15. Enter a space. Verify that a list of attribute hints pops up and you can navigate the list with up/down arrow key.
16. Hit Esc key to dismiss the code hints list, then delete the space so the cursor is after the "y" of "body".
17. Hit Cmd/Ctrl-E. Verify that it shows a single body rule and that everything is laid out properly.
18. In the native shell menu, choose View > Increase Font Size. Verify both the host and inline editors font size increases. The inline editor should not show a vertical scrollbar.
19. Click the lightning bolt in the upper right. If you trashed prefs, you'll get an info dialog explaining how live preview works. Hit OK.
20. You should see the page load in Chrome. After a few seconds you should get a dialog saying you need to relaunch Chrome.
21. Click "Relaunch". Chrome should relaunch and open the page.
22. Back in Brackets, edit the background color for the <body> tag in the inline editor (#D90 is a nice color). Verify that the color changes in Chrome as you type. Also verify that the CSS file is added to the working set with the dirty bit set.
23. Hit Cmd/Ctrl-E. Verify that the inline editor closes.
24. Put the cursor immediately after the `<a` in one of the `<a>` tags in the navbar.
25. Hit Cmd/Ctrl-E. Verify that the inline editor opens and that you see a number of rules in the list on the right.
26. Scroll up and down in the outer editor. Verify that the inline editor scrolls properly with the editor.
27. Resize the window. Verify that the rule list moves properly and there are no visual glitches.
28. Click on a rule in the rule list. Verify that the editor shows the correct rule.
29. Quit the app. Verify that you get a "save changes" dialog for any CSS files you edited through the inline editor, and choose to discard the changes.
30. Restart the app. Verify that the "citrus completed" project shows in the sidebar, and that the working set and current editor are showing the same files as when you quit. Also verify that the changes you had previously made were reverted (`git status` in the smokes folder should show clean).