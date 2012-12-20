###Proposed Pull Request checklist

* Does this change belong in core? Some features would be better as an extension - could it be done as an extension by separating out a more limited set of core changes (e.g. more generic APIs)?
* Some pull requests require the core team to implement additional supporting code in order to work. These pull requests may be delayed until the core team has time to do that work.
* Any major architectural or UI changes have been discussed in the forum?
* All new APIs are documented in the [Release Notes] (https://github.com/adobe/brackets/wiki/Release-Notes)?
* Code follows our JS coding style guidelines (we probably need to clean those up)
* Code passes JSLint
* Code is syntactically valid (Brackets launches & no exceptions in the console)
* Testing
    * Code has been tested -- describe the cases that were tested
    * All unit tests pass
    * No known bugs
    * Smoke tests have been run (nontrivial changes specifically)
    * Please write Unit Tests for all new functionality
* All UI strings externalized (we should have a how-to page for this).
* If there are string changes, it can't land at the very end of the sprint
* Native: should compile
* Native: Mac AND Win implementations
* UI is reasonably polished ?

###Avoid Common pitfalls
(make sure these have been thought about):
* Text manipulation commands: should consider what happens when in an inline editor at boundaries
* Inline editors: does this collide with any other providers?
* Code hinting: does this collide with any other providers?