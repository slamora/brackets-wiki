This is a draft!
----------------
This document will not be finalized until the end of Sprint 12.

New Extensibility APIs / API Enhancements
-----------------------------------------
**LiveDevelopment** - Two new API functions: ```enableAgent(name)``` and ```disableAgent(name)```.

Live Development has a number of more "experimental" agents that are disabled by default. However, some extensions (e.g. [jdiehl/brackets-debugger](https://github.com/jdiehl/brackets-debugger)) would like to use these agents. So, calling these functions allows enabling/disabling of specific experimental agents. The new agents take effect the next time a live development session starts. Current live development sessions are not affected. 