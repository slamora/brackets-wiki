This set of Smoke Tests exercise Brackets code that requires a server. As with the [main set of Smoke Tests](Brackets-Smoke-Tests), the intention is to keep it quick--if it takes more than 5 minutes to run the tests on a given platform (not including the initial setup time) then it's too long.

If you have trouble running through it or something is unclear, please post to the [brackets-dev mailing list](http://groups.google.com/group/brackets-dev).

Server Setup
============
You will need an HTTP server for these tests. Options are:
* Setup a local wamp or mamp server.
* Use a remote server on intranet with drive mapped to local machine.
* _Future:_ Use Brackets built-in node server with HTTP plugin

Setup
=====

1. Follow Setup steps for [main set of Smoke Tests](Brackets-Smoke-Tests).
2. Copy server-test folder **(TBD)** to server root.

Server smoke test steps
=======================

1. Launch Brackets. Verify that the Brackets "Getting Started" folder is visible in the project panel and its index.html file is opened automatically.
2. File > Open Folder... and browse to server root folder.
3. Verify that you see **(TBD)** pathRel.html, pathRoot.html, server.shtml, css and images folders.
4. Select File > Project Settings... to invoke Project Settings dialog. If a Base URL is specified, delete it.
5. Verify informational text is displayed in empty field. Click OK.
6. Open pathRel.html, start Live Preview using File > Live Preview.
7. If you trashed prefs, you'll get an info dialog explaining how Live Preview works. Click OK.
8. Mac only: after a few seconds you should get a dialog saying you need to relaunch Chrome. Click "Relaunch". Chrome should relaunch and open the page.
9. Verify that page renders correctly in browser.
10. Open File menu, verify Live Preview has a check mark next to it, then click it to toggle off Live Preview.
11. Open pathRoot.html and click Live Preview (lightning bolt) icon on right side of menu bar.
12. Verify that page is opened in browser, but with no CSS or images. Click Live Preview icon to disconnect Live Preview.
13. Open server.shtml and start Live Preview. Verify Project Settings dialog is invoked with warning message indicating a Base URL is required.
14. Enter the URL that maps to the project on your server (e.g. http://localhost/). Click OK. Verify that page renders correctly in browser.
15. In Brackets, set the cursor in the `<body>` tag, hit Cmd/Ctrl-E, and verify that it shows a single body rule.
16. Edit the background color for the <body> tag in the inline editor (#D90 is a nice color). Verify that the color changes in Chrome as you type. Also verify that the CSS file is added to the working set with the dirty bit set.
17. Hit Cmd/Ctrl-E. Verify that the inline editor closes.
18. Make an edit to some text in HTML page that is visible in browser. Verify that text has not yet changed in browser.
19. Use Cmd/Ctrl-S to save changes to HTML file. Verify that changes to HTML file are changed in browser and unsaved CSS changes are still shown in browser.
20. Undo changes in HTML file and save to get back to original state.
21. Close all files and discard changes.
22. Click on recent project dropdown list and use Project Settings... to set Base URL back to blank.

