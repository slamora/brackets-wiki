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

* open page with page relative paths
* open page with site-root relative paths without server
* open page with site-root relative paths with server

* make sure base url is clear
* define base url with errors
* define final base url

* edit CSS
* edit HTML, then Save

