Download Brackets
====

* Contributors - See [Linux Development For Contributors](https://github.com/adobe/brackets/wiki/Linux-Development-for-Contributors). This is your guide to setting up a development environment, navigating your way around the code base, finding Linux specific user stories to contribute to, and updates on our progress overall.
* Users - Coming soon

Feature Status
====

The latest Linux builds (as of Sprint 27), are *near* feature parity with the the Mac and Windows builds. However, there are notable features (e.g. Node.js integration and extension install UI) that are in various stages of planning and development. To be clear though, Brackets features going forward that only rely on features in the browser can be developed in lock-step on all 3 platforms.

This table lists features that appear in the Linux build but do not function as expected. Please see the [Linux Development For Contributors](https://github.com/adobe/brackets/wiki/Linux-Development-for-Contributors) for progress on these features.

Known Issues
----

| Feature | Description | Workaround |
| ------------ | ------- | ----- |
| Extension Manager | Install, update and remove extensions via File > Extension Manager... UI is enabled but all functionality fails. | Unzip extensions to ``~/.Brackets/extensions/user``. |
| HTML Highlighting | When in Live Preview mode, place the cursor on an HTML element to highlight that element in the browser. | None |
| Native Menus | Native OS menus | N/A. Brackets already fallback to HTML menus on Linux |
| Show in OS | Right click on a file or folder and choose Show in OS to open the location in Mac Finder or Windows Explorer | None |
| File rename and delete | Rename and delete files from the project tree | None |

FAQ
====

1. **I found a bug, where do I file it?** Here [Brackets issues](https://github.com/adobe/brackets/issues)
1. **I want to help, but I don't have any C++ or GTK experience. How can I help?** Start learning C++ and GTK! Otherwise, see the next question.
1. **I can't help on the native shell, but how else can I help?** You can help fix issues (Linux-specific or not) in the www code located in the brackets repository.
1. **Why isn't Linux distribution X supported?**
We're currently focused on Ubuntu since [Chromium Embedded Framework (CEF)](https://code.google.com/p/chromiumembedded/) is well tested there both in a development environment and in deployment. We're open to contributors helping us *test* and support more distributions.
1. **How do I debug the native shell?** As of June 12, 2013, CEF does not have a 32-bit Debug build. Once this is available, we'll post a new answer to this question.
1. **Why aren't the Grunt scripts updated for Linux?** Haven't gotten to it yet.
1. **Why is Node.js required for brackets-shell?** With Node.js, we're able to leverage other open source packages to help us build features in Brackets like the HTTP server that we use for Live Preview to instrument HTML files.
1. **How do I use a browser other than Google Chrome?** You can't, for now. For all platforms, Brackets only supports Chrome and Chromium.
1. **Live Development doesn't work with the Chromium browser.  How can I fix it?**  If you want to use the Chromium browser for Live Development, you will need to add symlink the file `/usr/bin/google-chrome` to the chromium executable which is normally located at `/usr/bin/chromium-browser`. All it should take is `sudo ln -s /usr/bin/chromium-browser /usr/bin/google-chrome`.
1. **What IDE or tools should I use to work on brackets-shell?** Anything you want.
1. **I really want to use Brackets in my browser instead of the native shell. How do I do that?** This use case is unsupported currently. However, there is a (somewhat stale) [in-browser branch](https://github.com/adobe/brackets/tree/in-browser/) that implements required file system functionality. See this [Google Group thread](https://groups.google.com/d/msg/brackets-dev/HR4lwxEKt6M/WHj4fcstLwMJ) for an introduction.

Log
----
* 2013-06-24: Updated known issues and FAQ. [jasonsanjose](http://github.com/jasonsanjose)
* 2013-06-22: Created. Pointer to dev wiki page. Placeholder for end users until download and documentation plans are in place. [jasonsanjose](http://github.com/jasonsanjose)