Download Brackets
====

* Contributors - See [Linux Development For Contributors](https://github.com/adobe/brackets/wiki/Linux-Development-for-Contributors). This is your guide to setting up a development environment, navigating your way around the code base, finding Linux specific user stories to contribute to, and updates on our progress overall.
* Users - Coming soon

Feature Status
====

The latest Linux builds (as of Sprint 27), are *near* feature parity with the the Mac and Windows builds. However, there are notable features (e.g. Node.js integration and extension install UI) that are in various stages of planning and development. To be clear though, Brackets features going forward that only rely on features in the browser can be developed in lock-step on all 3 platforms.

This table lists features that appear in the Linux build but do not function as expected. Please see the [Linux Development For Contributors](https://github.com/adobe/brackets/wiki/Linux-Development-for-Contributors) for progress on these features.

| Feature | Description | Workaround |
| ------------ | ------- | ----- |
| | | |

FAQ
====

1. I found a bug, where do I file it? Here [Brackets issues](https://github.com/adobe/brackets/issues)
1. I want to help, but I don't have any C++ or GTK experience. How can I help? Start learning C++ and GTK! Otherwise, see the next question.
1. I can't help on the native shell, but how else can I help? You can help fix issues (Linux-specific or not) in the www code located in the brackets repository.
1. Why isn't Linux distribution X supported?
We're currently focused on Ubuntu since [Chromium Embedded Framework (CEF)](https://code.google.com/p/chromiumembedded/) is well tested there both in a development environment and in deployment. We're open to contributors helping us *test* and support more distributions.
1. How do I debug the native shell? As of June 12, 2013, CEF does not have a 32-bit Debug build. Once this is available, we'll post a new answer to this question.
1. Why aren't the Grunt scripts updated for Linux? The CEF binaries for Linux use 7zip instead of the standard ZIP file format. Some work needs to be done in the Grunt scripts to support this.
1. Why is NodeJS required for brackets-shell? 
1. How do I use a browser other than Google Chrome?
1. Live Development doesn't work with the Chromium browser.  How can I fix it?  If you want to use the Chromium browser for Live Development, you will need to add symlink the file `/usr/bin/google-chrome` to the chromium executable which is normally located at `/usr/bin/chromium-browser`. All it should take is `sudo ln -s /usr/bin/chromium-browser /usr/bin/google-chrome`.
1. What IDE or tools should I use to work on brackets-shell?
1. I really want to use Brackets in my browser instead of the native shell. How do I do that?

Log
----
2013-06-22: Created. Pointer to dev wiki page. Placeholder for end users until download and documentation plans are in place. [jasonsanjose](http://github.com/jasonsanjose)