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
2. Copy server-test folder (**TBD**) to server root.

Server smoke test steps
=======================

1. Launch Brackets. Verify that the Brackets "Getting Started" folder is visible in the project panel and its index.html file is opened automatically.
2. File > Open Folder... and browse to server root folder.
3. Verify that you see (**TBD**) pathRel.html, pathRoot.html, server.shtml, css and images folders.
4. Select File > Project Settings... to invoke Project Settings dialog. If a Base URL is specified, delete it.
5. Verify informational text is displayed in empty field. Click OK.
6. Open pathRel.html, start Live Preview using File > Live Preview, and verify that page renders correctly in browser.
7. Open File menu, verify Live Preview has a check mark next to it, then click it to toggle off Live Preview.
8. Open pathRoot.html and click Live Preview (lightning bolt) icon on right side of menu bar.
9. Verify that page is opened in browser, but with no CSS or images. Click Live Preview icon to disconnect Live Preview.
10. Open server.shtml and start Live Preview. Verify Project Settings dialog is invoked with warning message indicating a Base URL is required.
11. Enter the URL that maps to the project on your server (e.g. http://localhost/). Click OK.
12. Verify that page renders correctly in browser. 

* open page with site-root relative paths with server

* edit CSS
* edit HTML, then Save

