The core text editor in Brackets is [CodeMirror](http://github.com/marijnh/CodeMirror). The Brackets
team has submitted or contributed to a number of features and bug fixes in CodeMirror, and has also
given input into the design of features like line widgets and document/view separation.

Currently, Brackets uses a [fork of CodeMirror](http://github.com/adobe/CodeMirror2) as a submodule. 
Originally, this was because of a number of experimental features that Brackets implemented (including 
a prototype implementation of inline widgets) that we hadn't yet contributed upstream. Now that the
upstream CodeMirror repo has versions of these features, we've ported Brackets to use the upstream 
version.

As of Sprint 20, where we merged with the latest CodeMirror v3, the 
[adobe/CodeMirror2](http://github.com/adobe/CodeMirror2) fork's master has been reset to upstream 
master. Going forward, we will keep our master completely in sync with upstream; we will never add 
our own commits directly to master, and we'll plan to pull from upstream as often as we can, keeping 
the Brackets submodule pointing at master. However, in cases where there are last-minute upstream 
changes that we don't want to pull into a given sprint, and we need other fixes, we might make 
short-lived sprint branches.

Current status
==============

There are currently two branches of note in the [adobe/CodeMirror2](http://github.com/adobe/CodeMirror2) fork:

* `master` - this has now been reset to upstream master, and will always reflect the state of upstream.
  As of sprint 21, Brackets is now pointing directly to this branch.
* `v2-master` - this is the last commit we made in our old (diverged) master. We won't be using this branch
  going forward, but older versions of Brackets refer to it in their submodule SHAs, so we need to keep
  it around for archival purposes.

Going forward, we'll generally submit pull requests directly to the 
[main CodeMirror repo](http://github.com/marijnh/CodeMirror) rather than making fixes in branches in our fork,
then pull them down once they've been merged upstream. Similarly, if you have fixes you need in CodeMirror,
please submit them upstream (you can file an issue with us to let us know that it's important for us to merge
it soon, or refer to your upstream fix from a dependent pull request). We generally won't accept pull requests
directly into our CodeMirror fork.

If you have an existing fork or clone of adobe/CodeMirror2 from before Sprint 20, you shouldn't try to pull
into your master from our master--instead, you'll want to do a direct reset of your master branch to our master 
(e.g. if you have your `upstream` remote set to adobe/CodeMirror2, then do `git checkout master` and 
`git reset upstream/master`).

Note that we have not yet renamed the submodule or the repo to reflect the current naming of the upstream
repo--it's still "CodeMirror2". This renaming is a bit tricky and isn't urgent, so we've put it off until
later. See [this Google Group thread](https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/D_rezwjyXM0)
for more info.
