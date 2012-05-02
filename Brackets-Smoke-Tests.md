**EARLY DRAFT--not ready for review yet**

Brackets has a pretty good unit test suite that you can run from Debug > Run Unit Tests, but that doesn't always cover issues with the overall UI and integrated functionality, or visual/layout issues that are only obvious if you're actually looking at the product. This is a set of manual tests intended to make sure we haven't broken the basic overall workflows of the product. The intention is to keep it quick--if it takes more than 5 minutes on a given platform it's too long :)

(TODO: setup step--get the sample app)

Core functionality
==================

0. Quit and relaunch Chrome (so it's *not* in debug mode).
1. Launch Brackets.
2. File > Open Folder and browse to the sample app folder. Verify that its contents look correct, and expand some folders to look inside them.
3. Click on index.html. Verify that it opens in the editor but is not in the working set.
4. Resize the window. Verify that the title bar wraps appropriately.
5. Set the cursor in the <body> tag.
6. Hit Cmd-E. Verify that it shows a single body rule.
7. Click the lightning bolt in the upper right. You should get the dialog saying you need to relaunch Chrome.
8. Click "Relaunch". Chrome should relaunch and open the page.
9. Back in Brackets, edit the background color for the <body> tag in the inline editor (#D90 is a nice color). Verify that the color changes in Chrome as you type.
10. Hit Cmd-E. Verify that the inline editor closes.
11. Put the cursor immediately after the "<a" in one of the <a> tags in the navbar.
12. Hit Cmd-E. Verify that the inline editor opens and that you see a number of rules in the list on the right.
13. Scroll up and down in the outer editor. Verify that the inline editor scrolls properly with the editor.
14. Resize the window. Verify that the rule list moves properly and there are no visual glitches.
15. Click on a list in the rule list. Verify that the editor shows the correct rule.

Unofficial features
===================
TODO
1. Edit > Find...
2. Edit > Find in Files...
3. Navigate > Quick Open...
