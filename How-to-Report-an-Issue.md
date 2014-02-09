# How to Report a Brackets Issue

Found a bug you'd like to tell us about? Great!

### How to file a bug

Brackets bugs are tracked in [the issue list on GitHub](https://github.com/adobe/brackets/issues). Always search for existing issues before filing a new one (use the search field at the top of the page).

When filing a new bug, please remember to include:

* A helpful title - use descriptive keywords in the title and body so others can find your bug (avoiding duplicates).
* Steps to reproduce the problem, with actual vs. expected results
* Brackets sprint number (or if you're pulling directly from the Git repo, your current commit SHA - use `git rev-parse HEAD`)
* OS version
* If the problem happens with specific code, link to test files ([gist.github.com](https://gist.github.com/) is a great place to upload code).
* Screenshots are very helpful if you're seeing an error message or a UI display problem. (Just drag an image into the issue description field to include it).

### If you've installed extensions

Bugs can be caused by Brackets extensions you've added. Before you file an issue on Brackets core, first rule out extensions by disabling them. [See "Extension bugs" below](#extension-bugs) for details.

### What about requesting a feature?

Please first check our [feature backlog on Trello](http://bit.ly/BracketsBacklog) to see if it's already there. You can vote on features in the backlog to help us prioritize them.

Feel free to file feature requests as an issue on GitHub, just like a bug. We tag these issues "move to backlog" and periodically migrate them onto feature backlog for you.


# What happens after a bug is filed?

### Bug review

We review _all_ new issues on a regular basis (the issue tagged _'last reviewed'_ is the most recent one reviewed). Several things typically happen as part of review:

* Is the issue clearly understandable? If not, we'll ask for clarification (tagging the filer with "@").
* Is the issue more of a feature request? If so, we'll tag it [_'move to backlog'_](https://github.com/adobe/brackets/issues?labels=move+to+backlog&state=open); these items are periodically migrated to our [feature backlog](http://bit.ly/BracketsBacklog).
* Labels are added to indicate priority - high, medium, low, or none ([read more below](#wiki-bug-priority)). Bugs may also be assigned a sprint milestone - if they are urgent or related to features under development in the current sprint.
* Labels may be added to categorize the bug. See ["Understanding issue labels" below](#understanding-issue-labels).
* Would the fix be a good intro to the Brackets source code? If so we'll tag it [_'starter bug'_](https://github.com/adobe/brackets/issues?labels=starter+bug&state=open) to encourage Brackets newcomers to work on it.
* Most bugs are assigned to a Brackets core committer right away. Depending on priority, milestone, and other workload, the developer may or may not begin working on the bug soon.

Some bugs may be closed without fixing - see ["Hey! My bug wasn't fixed!" below](#hey-my-bug-wasnt-fixed).

### Bug lifecycle

The process above covers the first two steps. Here are the rest:

1. New bug is filed; awaiting review -- bug is newer than _'last reviewed'_ tag
2. Triaged in bug review -- bug is assigned to a developer
3. Developer begins working on it -- bug is tagged _'fix in progress'_
4. Developer opens a pull request with a fix, which must be reviewed -- a link to the pull request appears in the bug's activity stream
5. Pull request is merged, and original filer is requested to verify that it's fixed -- bug is tagged _'fixed but not closed'_ (FBNC)
6. Filer agrees that it's fixed -- bug is closed, and its milestone is set to the sprint the fix landed

### Can I help fix a bug?

Yes please! But first...

* Check to make sure no one else is already working on it. If the bug has a sprint milestone assigned or is tagged _'fix in progress'_ then it's already under way. Otherwise, post to the [brackets-dev newsgroup](http://groups.google.com/group/brackets-dev), the [#brackets IRC channel on freenode](http://webchat.freenode.net/?channels=brackets), or the bug comment thread itself (pinging the assigned developer via "@") to check.
* Read the [guidelines for contributing code](https://github.com/adobe/brackets/blob/master/CONTRIBUTING.md#contributing-code) - especially, make sure you're following our [pull request guidelines](Pull Request Checklist) and [coding conventions](Brackets Coding Conventions).


# Extension bugs

Bugs can be caused by Brackets extensions you've installed. Before you file an issue on the core product, first rule out extensions by disabling them.  This can be done by selecting _Brackets > Debug > Reload Without Extensions_ and then trying to reproduce the problem with your extensions disabled.  If you can still reproduce the problem, file an issue on Brackets.

If you can't reproduce the problem, there is a problem with one of your extensions.  You will need to
manually search for the extension that is causing the problem.

For now, testing extensions must be done manually:

1. Select _Brackets > Help > Show Extensions Folder_.
2. Move all of the extensions from the `user` directory to the `disabled`
directory, then restart Brackets.
3. Move extensions back to the `user` directory one at a time (restarting Brackets each time) until you find the cause. If you find an extension bug, please file an issue in that extension's repo so it can be addressed by the extension author.
4. When finished, move all your extensions back into the `user` directory, then restart Brackets again to re-enable them all.

You can also try selecting _Brackets > Help > Show Developer Tools_ so you can look at the console.  Sometimes the console has error messages that can help you locate the file that is failing, and knowing the location of the file can help you track down which extension is failing.

# Understanding issue labels

We use labels/tags for a number of purposes:

<table width=90% border="5" cellpadding="1" bordercolor="#000033">
  <tr>
    <th width=15% scope="col"></th>
    <th width=15% scope="col">label</th>
    <th width=60% scope="col">meaning</th>
  </tr>
  <tr>
    <td rowspan="4">Process </td>
    <td>fix in progress</td>
    <td>Someone has started work on a fix (or the fix is ready but still undergoing code review - not merged yet).</td>
  </tr>
  <tr>
    <td>fixed but not closed</td>
    <td>Fix has been merged. Waiting for the original bug reporter to verify that it's fixed.</td>
  </tr>
  <tr>
    <td>last reviewed</td>
    <td>Last reviewed/triaged bug. See <a href="#bug-review">"Bug review" above</a>.</td>
  </tr>
  <tr>
    <td>move to backlog</td>
    <td>Feature/enhancement request rather than a bug. Will be moved to the <a href="http://bit.ly/BracketsBacklog">feature backlog</a>.</td>
  </tr>
  <tr>
    <td rowspan="4">Priority <a name="bug-priority"></a></td>
    <td>high priority</td>
    <td>High impact bug many users will hit (e.g. crash/data loss). We try to have zero high-priority bugs before shipping each sprint.</td>
  </tr>
  <tr>
    <td>medium priority</td>
    <td>Bug that's at least somewhat severe and a significant number of users will hit. We closely track the number of medium-priority bugs.</td>
  </tr>
  <tr>
    <td>low priority</td>
    <td>Low severity (e.g. small cosmetic issue) and/or few users will hit. May not be fixed for a while.</td>
  </tr>
  <tr>
    <td>no priority</td>
    <td>Impact is so low that we don't plan to ever spend time fixing the bug - but we would accept a fix if someone offers a pull request. May be an issue we anticipate will become obsolete due to upcoming UI or feature changes.</td>
  </tr>
  <tr>
    <td rowspan="3">Suggestions </td>
    <td>starter bug</td>
    <td>Fixing this bug would be a good intro to the Brackets source code. Recommended for new contributors.</td>
  </tr>
  <tr>
    <td>Extension Idea</td>
    <td>Feature/enhancement request that's a great idea for a Brackets extension. May be out of scope for Brackets core.</td>
  </tr>
  <tr>
    <td>documentation</td>
    <td>Fix involves improving documentation rather than writing code.</td>
  </tr>
  <tr>
    <td rowspan="3">External tracking </td>
    <td>codemirror</td>
    <td>Needs <a href="https://github.com/marijnh/CodeMirror">CodeMirror</a> changes.</td>
  </tr>
  <tr>
    <td>cef<br>webkit</td>
    <td>Needs code changes in <a href="https://code.google.com/p/chromiumembedded/">Chromium Embedded Framework</a> (which brackets-shell is built on) or in WebKit/Chromium itself.</td>
  </tr>
  <tr>
    <td>tracking</td>
    <td>Specific bug has been filed in external project - waiting on a fix.</td>
  </tr>
  <tr>
    <td rowspan="5">Architecturally-focused</td>
    <td>architecture</td>
    <td>Requires significant architectural changes - to be discussed in meeting or newsgroup.</td>
  </tr>
  <tr>
    <td>performance</td>
    <td>Perceived or measurable performance issue.</td>
  </tr>
  <tr>
    <td>code cleanup</td>
    <td>Fix involves cleaning up code, improving maintainability without making major architectural changes. (May be the main benefit of fixing the bug).</td>
  </tr>
  <tr>
    <td>async</td>
    <td>Bug caused by asynchronous execution / race condition</td>
  </tr>
  <tr>
    <td>native shell</td>
    <td>Needs code changes in <a href="https://github.com/adobe/brackets-shell">brackets-shell</a>, our desktop native wrapper</td>
  </tr>
</table>

### Other bug-related terminology

Acronyms we use frequently in bug comments:

* _FBNC_ = "fixed but not closed": See the bug-process label above.
* _UTR_ = "unable to reproduce": Someone tried to follow the steps in the bug, but everything seemed to work fine.
* _NAB_ = "not a bug": The behavior described in the bug is actually how it's supposed to work. This may indicate a usability or documentation problem.
* _FOL_ = "fact of life": The bug is virtually impossible to fix due to constraints of the OS, browser runtime, etc. Suggests the bug should be closed or tagged "no priority."

# Hey! My bug wasn't fixed!

Yeah, what's up with that? There are a number of reasons an issue might get closed without being fixed:

* Tagged "move to backlog" - the issue is a larger feature/enhancement request. It's not forgotten! Look for a link in the comment thread to a user story on our [feature backlog](http://bit.ly/BracketsBacklog). Please vote on the story to help us prioritize it!
* Tagged "fixed but not closed" - _we_ think it's fixed. Make sure you have a Brackets build containing the fix (check the sprint milestone assigned to the bug). If you're still seeing it, let us know.
* Duplicate - there's already a bug for this (may be part of a larger issue).
* Unable to reproduce - we're unable to reproduce the result described in the bug report. If you're still seeing it, please reply with more detailed steps to trigger the bug.
* Out of scope - this change probably doesn't belong in Brackets core... but it could still make for a great extension!
* Not a bug / fact of life - this is the intended behavior. If it feels wrong, we should discuss how to improve the usability of the feature.
* Tagged "no priority" (usually left open) - impact is so low that we don't plan to spend time fixing the bug. But we would accept a fix if someone else is willing to work on it.

If you disagree with a bug being closed, feel free to post a comment asking for clarification or re-evaluation. The more new/updated info you can provide, the better.

<br>
Thanks a ton for your support and contributions to Brackets!