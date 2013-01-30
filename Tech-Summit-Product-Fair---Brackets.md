## Walk-through Steps

1. Launch Brackets if it's not already running. You should be in the "citrus completed" project, so the "citrus completed" folder is visible in the Project panel on the left, and you see "css" and "images" folders and an "index.html" file in the project tree.
2. If the "citrus completed" is not the current project, click on the triangle next to the project name. If the dropdown show the "citrus completed" project, then select it.
3. If the project is not in the recent projects list, use File > Open Folder and browse to the "brackets/test/smokes/citrus completed" folder. 
4. Click on index.html. The contents of the file should be displayed in the editor.
5. Double-click on index.html. Notice that it's added to the working set above the project tree, and the selection changes to the working set.
6. In the document, set the cursor in the "<body>" tag immediately before the ">".
7. Enter a space to pop up a list of attribute hints. You can navigate the list with up/down arrow the mouse or keyboard.
8. Hit Esc key to dismiss the code hints list, then delete the space so the cursor is after the "y" of "body".
9. Hit Cmd/Ctrl-E to invoke Quick Edit. It should list a single CSS body rule on the right and show the contents of the rule on the left.
10. Click the lightning bolt in the upper right. You should see the page load in the Chrome browser.
11. Mac only: after a few seconds you should get a dialog saying you need to relaunch Chrome. Click "Relaunch". Chrome should relaunch and open the page.
12. Back in Brackets, edit the background color for the <body> tag in the inline editor (#D90 is a nice color). Notice that the color changes in Chrome as you type. Also verify that the CSS file is added to the working set with the dirty bit set.
13. Hit Cmd/Ctrl-E. Verify that the inline editor closes.
14. Put the cursor immediately after the "<a" in one of the "<a>" tags in the navbar.
15. Hit Cmd/Ctrl-E. In the inline editor, you should see a number of rules in the list on the right. Select different rules on the right to change rules.
16. Make an edit to some text in HTML page that is visible in browser. Note: some text in the page is replaced by logo image via CSS (e.g. first h1 tag and first h2 tag) so edits will not be visible; try updating text in News section.
17. Verify that text has not yet changed in browser. Use Cmd/Ctrl-S to save changes to HTML file. Verify that saved text changes and unsaved CSS changes are shown in browser.
18. Disconnect Live Preview. Undo changes in HTML file and save to get back to original state.
19. Close all files and discard changes.
