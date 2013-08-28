This set of Smoke Tests exercise Brackets code that requires a server. As with the [main set of Smoke Tests](Brackets-Smoke-Tests), the intention is to keep it quick--if it takes more than 5 minutes to run the tests on a given platform (not including the initial setup time) then it's too long.

If you have trouble running through it or something is unclear, please post to the [brackets-dev mailing list](http://groups.google.com/group/brackets-dev).

Server Setup
============
You will need an HTTP and PHP server for these tests. Options are:
* Setup a local server (e.g. wamp/mamp/lamp).
* Use built-in server on Mac:  
    a. (OSX 10.7 and earlier) Turn on web server: System Preferences > Sharing > Web Sharing: On  
    b. Turn on PHP server: Edit /etc/apache2/httpd.conf, uncomment following line:  
        #LoadModule php5_module libexec/apache2/libphp5.so  
    c. Server root folder: /Library/WebServer/Documents/  
    d. (OSX 10.8 and later) Start (or restart) apache:  run `sudo apachectl start` in a terminal window (use `restart` if already running)
* Use a remote server with drive mapped to local machine.
* Sprint 21: Brackets has node js built-in with an HTTP server plugin. Any project folder can be the server root. Note that this server does not support PHP, so the .php cannot be viewed, but all of the tests in that section can be run using one of the .html pages.

Setup
=====

1. Follow Setup steps for [main set of Smoke Tests](Brackets-Smoke-Tests).
2. Copy the server-tests folder from brackets/test/smokes to server root, or run brackets/tools/setup_server_smokes script to create a symbolic link to the files. The server root folder will be something like c:\wamp (on Win) or /Library/Webserver/Documents (on Mac). On the Mac, you may need to run the script with "sudo" to have proper permissions.

Server smoke test steps
=======================

1. Launch Brackets. Use File > Open Folder... and browse to the smoke tests folder (brackets/test/smokes).
2. Open the server-tests folder in the project tree and verify that you see pathRel.html, pathRoot.html, server.php files, and css &amp; images folders.
3. Select File > Project Settings... to invoke Project Settings dialog. If a Base URL is specified, delete it.
4. Verify informational text is displayed in empty input field. Click OK.
5. Open pathRel.html, start Live Preview using File > Live Preview.
6. If you trashed prefs, you'll get an info dialog explaining how Live Preview works. Click OK.
7. On Mac, you should get a dialog saying you need to relaunch Chrome. Click "Relaunch". Chrome should relaunch and open the page.
8. Verify that the base URL is 127.0.0.1: _nnnnn_ (where _nnnnn_ is an auto-generated port number) and page renders correctly in browser.
9. Open File menu, verify Live Preview has a check mark next to it, then click it to toggle off Live Preview.
10. Open pathRoot.html and click Live Preview (lightning bolt) icon on right side of menu bar.
11. Verify that page is opened in browser and CSS, images (with site-relative paths) are rendered correctly. Click Live Preview icon to disconnect Live Preview.
12. Open server.php and start Live Preview. Verify Project Settings dialog is invoked with warning message indicating a Base URL is required.
13. Enter the URL that maps to the project on your server (e.g. http://localhost/). Click OK.
14. Verify that page is opened in Chrome and the URL protocol is the same as in your URL Prefix (http: or https:) and page renders correctly in browser.
15. In Brackets, set the cursor in the `<body>` tag, hit Cmd/Ctrl-E, and verify that it shows a single body rule.
16. Edit the background color for the <body> tag in the inline editor (#D90 is a nice color). Verify that the color changes in Chrome as you type. Also verify that the CSS file is added to the working set with the dirty bit set.
17. Hit Cmd/Ctrl-E. Verify that the inline editor closes.
18. Make an edit to some text in HTML page that is visible in browser. Note: some text in the page is replaced by logo image via CSS (e.g. first h1 tag and first h2 tag) so edits will not be visible; try updating text in News section.
19. Verify that text has not yet changed in browser. Use Cmd/Ctrl-S to save changes to HTML file. Verify that saved text changes and unsaved CSS changes are shown in browser.
20. Disconnect Live Preview. Undo changes in HTML file and save to get back to original state.
21. Close all files and discard changes.
22. Select File > Project Settings... to set Base URL back to blank.