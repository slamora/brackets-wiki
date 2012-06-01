What's new in Sprint 9
----------------------
We didn't add major new user-visible features in Sprint 9. Our main goals for the sprint were similar to the previous sprint - Continue to make progress on the extensibility architecture of Brackets: *research, basic architecture and extensibility API*:
* **Architecture / Research**
     * CEF3 vs. Chromium Content Shell research  (Glenn) 
     * Node vs V8 for bridging into native  (Glenn, Joel, Raymond)
         * A prototype implementation is checked into the *node-proxy* branches in adobe/brackets and adobe/brackets-app repos.

* **Extensibility:**
    * [Menus and Keyboard Shortcuts] (https://groups.google.com/forum/?fromgroups#!searchin/brackets-dev/Menus$20and$20Keyboard$20Shortcuts/brackets-dev/-PfnGtF2npI/_JnDV43y4nMJ) *As an extension developer, I want to be able to add custom menus and keyboard shortcuts when my extension is enabled.* (Ty, Peter, Randy, Raymond, Jason)
        * Add a menu item: Get a top-level menu by calling `Menus.getMenu()` with one of the AppMenuBar constants. Then add a menu item using `theMenu.addMenuItem()`, linking it to your Command id. The menu item's label will be the string name you gave the Command when it was created. Please refer to the [Simple "Hello World" extension] (https://github.com/adobe/brackets/wiki/Simple-%22Hello-World%22-extension) for a code sample.

    * Enable [Unit Tests] (https://github.com/adobe/brackets/wiki/Extension-Experiments#wiki-unittests) in extensions (Randy, Jason, Peter, NJ):
        * We're now able to run unit tests in extensions
        * We implemented Unit Tests in the JS Quick Edit extension
        * We also added an API call to fix [https://github.com/adobe/brackets/issues/831](https://github.com/adobe/brackets/issues/831) *FileUtils.getNativeModuleDirectoryPath()*. Take a look at src/extensions/disabled/JavaScriptInlineEditor/unittests.js for an example.
        * We search all extensions in "default" and "user" folders for unittest.js file, which is optional
        * We made some changes to ExtensionLoader module for loading timing
External contributions:
-----------------------
* [ryanstewart](http://github.com/ryanstewart) made improvements to the Performance of Sidebar
* [conradz](http://github.com/conradz) improved Line and Block Commenting 


Bugs fixed in Sprint 9
----------------------
See [closed sprint 9 bugs](https://github.com/adobe/brackets/issues?labels=sprint+9&page=1&state=closed). A few other bugs might have been fixed that weren't tagged.

Known issues in Sprint 9
------------------------

* 

Upcoming features
-----------------

Here are some things we're planning to do over the next few sprints:

* Commit Scrolling CodeMirror Enhancements
* Increase / Decrease Code Font Size
* Editor HTML Context Menus and Submenues
* Specify css stylesheets
* Continue performance research and enhancements