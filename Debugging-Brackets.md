## Simple Debugging

To bring up the Chrome Developer Tools on the Brackets window, use _Debug > Show Developer Tools_. This will open a new tab in Chrome containing the developer tools. You can use `console.log()`, breakpoints, etc. just as if you were debugging a normal web page.

_The first time you open Developer Tools, you must [disable caching](https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/E5iqcD8VqD4)_ - otherwise using Reload while dev tools are open will not reflect changes to certain code (such as extensions).

To debug code that runs at startup you can launch Brackets, open the developer tools, set your breakpoints, and then select _Debug > Reload Brackets_. Developer tools will remember your breakpoints as the startup process runs after reload.


## Two-Window Workflow

You can run two instances of Brackets so you still have a working editor if you end up breaking Brackets in the process of making code changes:

* Use "Debug > New Window" to launch a new, separate Brackets instance.
* In window #2, you can open a different folder on disk to access other content for testing your extension (e.g. test.html).
* In window #1, edit your extension code and save.
* Reload window #2 to pick up extension changes, and test it out.
* Don't reload window #1 until you're at a good stable "safe point."

You can open separate copies of developer tools for the two windows if needed.


## Debugging Unit Tests

Some Brackets unit tests run directly in the unit test runner window, while others pop up a separate test instance of Brackets.

To debug tests that run in the unit test window -- click "Show Developer Tools" in the window header.

To debug tests that run in a separate window -- see [[Debugging Test Windows in Unit Tests]].