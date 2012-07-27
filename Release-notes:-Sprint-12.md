This is a draft!
----------------
_This document will not be finalized until the end of Sprint 12 -- August 10._

What's New in Sprint 12
-----------------------
* **Code Hinting**
    * [Code Completion for HTML Attributes](https://trello.com/card/3-code-complete-html-attributes/4f90a6d98f77505d7940ce88/557)
* **Native Shell**
    * [Live Development in CEF 3 Shell](https://trello.com/card/2-enable-live-development-in-cef3-shell/4f90a6d98f77505d7940ce88/539)
    * [CEF 3 Shell Bug Fixes](https://trello.com/card/2-test-cef3-shell/4f90a6d98f77505d7940ce88/540)
    * [Research Native Installer/Packaging](https://trello.com/card/2-research-brackets-native-installer/4f90a6d98f77505d7940ce88/582)
    * [Research HiDPI (Retina) Support](https://trello.com/card/0-research-hidpi-support/4f90a6d98f77505d7940ce88/585)

UI Changes
----------

API Changes
-----------

New/Improved Extensibility APIs
-------------------------------
**LiveDevelopment** - Two new API functions: ```enableAgent(name)``` and ```disableAgent(name)```.

Live Development has a number of more "experimental" agents that are disabled by default. However, some extensions (e.g. [jdiehl/brackets-debugger](https://github.com/jdiehl/brackets-debugger)) would like to use these agents. So, calling these functions allows enabling/disabling of specific experimental agents. The new agents take effect the next time a live development session starts. Current live development sessions are not affected. 

Known Issues
------------

Contributions from Brackets
---------------------------

External contributions
----------------------

Bugs fixed in Sprint 12
-----------------------
For details on the bugs addressed, please refer to [closed sprint 12 bugs](https://github.com/adobe/brackets/issues?labels=sprint+12&page=1&state=closed). A few other bugs might have been fixed that weren't tagged.