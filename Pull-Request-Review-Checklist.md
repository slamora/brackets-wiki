This checklist is for Brackets Committers to assist in reviewing pull requests to ensure that they are ready to merge. It has some elements in common with the [Pull Request Checklist](https://github.com/adobe/brackets/wiki/Pull-Request-Checklist) because committers have a chance to double check work before it goes in.

## Proposed Pull Request Review checklist

### Ensuring the change is reasonable ###

* Does this change belong in core? Some features would be better as an extension - could it be done as an extension by separating out a more limited set of core changes (e.g. more generic APIs)?
* Any major architectural or UI changes have been discussed in the forum?
* All new APIs are documented in the [Release Notes] (https://github.com/adobe/brackets/wiki/Release-Notes)?

### Testing the change ###

* Download the code
    * `git remote add (username) (URL of user's GitHub repository)`
    * `git fetch (username) (branchname):(username)/(branchname)`
    * `git checkout (username)/(branchname)`
* Code passes JSLint
* Code is syntactically valid (Brackets launches & no exceptions in the console)
* All unit tests pass
* Smoke tests have been run (nontrivial changes specifically)
* UI is reasonably polished ?

### Other things to look for when reviewing the code ###

* Code follows our JS coding style guidelines (we probably need to clean those up)
* Edge cases not covered by the code
* Code has unit tests
* All UI strings externalized (we should have a how-to page for this).
* If there are string changes, it can't land at the very end of the sprint
* Native: should compile
* Native: Mac AND Win implementations

### Avoid Common pitfalls

(make sure these have been thought about):

* Text manipulation commands: should consider what happens when in an inline editor at boundaries
* Inline editors: does this collide with any other providers?
* Code hinting: does this collide with any other providers?