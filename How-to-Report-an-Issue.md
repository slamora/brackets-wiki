# How to deal with issues

You tested Brackets and found an issue - now you want to file it – that's great!

While you help us to improve the quality of Brackets, please keep a few steps in mind to help us manage issues in Brackets.
Bugs are tracked in the [GitHub issue tracker](https://github.com/adobe/brackets/issues).

## Existing issues, feature backlog
Please review the existing issues – GitHub provides a search field to search for Issues and Milestones, found in the top right:
![search in github](https://github.com/adobe/brackets/wiki/screenshots/issues_search.jpg)

Typing into the search field will show related issues, this illustrates the importance of choosing keywords within titles, such that others don't file duplicates for your issue. Some of the issues – basically enhancement requests – are moved to our [public feature backlog](http://bit.ly/BracketsBacklog), please consider to search through the existing user stories too. There you most likely will find the missing features, enhancements to existing features, etc.

If you find an issue which seems to cover your case you may add comments or scenarios which are specific to your workflow or content. You can also vote for backlog items to help us prioritize.

## How to file an issue or feature request
Filing an issue is simple, click on the _"New Issue"_ button, add a title and a description. We want to provide some guidance regarding which information should be contained in the description, how we consider to set priority, and how we make use of labels in GitHub.

 Thanks for supporting our project and other contributors - To enhance reproducibility we ask you to provide information necessary to understand and confirm issues.

* Brackets version/sprint number (please use sprint labels or commit SHA if you're pulling directly from the repo (see below)).
* Platform/OS version (please use labels for Windows / Mac)
* Reproducible steps, actual and expected results
* Link to test files (you can create a gist on gist.github.com if that's convenient).

### Determine the priority
We differentiate three priorities for issues, we use labels on GitHub to set the priority:

* _High_: crashes/data loss unless they're very unlikely edge cases
* _Medium_: functional/ui issues that are at least somewhat severe and that a significant number of users will hit
* _Low_: issues that have low severity and/or low frequency
* _No Priority_: issues will remain open but the Brackets development team is not planning to fix (see more about No Priority issues below)

### Use of labels for issues found in Brackets

<table width=90% border="5" cellpadding="1" bordercolor="#000033">
  <tr>
    <th width=15% scope="col">group</th>
    <th width=15% scope="col">labels</th>
    <th width=60% scope="col">usage</th>
  </tr>
  <tr>
    <td rowspan="6">Process </td>
    <td>fix in progress</td>
    <td>Bug has been assigned and assignee has started to make improvements</td>
  </tr>
  <tr>
    <td>fixed but not closed</td>
    <td>issue has been fixed but not merged, may need to be regressed</td>
  </tr>
  <tr>
    <td height="40">last reviewed</td>
    <td>maker for last reviewed bug, see below for explanation of the review process</td>
  </tr>
  <tr>
    <td>move to backlog</td>
    <td>enhancement marker - to be put  on the [backlog](http://bit.ly/BracketsBacklog)</td>
  </tr>
  <tr>
    <td>starter bug</td>
    <td>This issue doesn’t require deep knowledge of the architecture and is the recommended level for new contributors</td>
  </tr>
  <tr>
    <td>documentation</td>
    <td>insufficient documentation</td>
  </tr>
  <tr>
    <td rowspan="4">Priority</td>
    <td>high priority</td>
    <td>crashes/data loss unless they're very unlikely edge cases</td>
  </tr>
  <tr>
    <td>medium priority</td>
    <td>functional/ui or performance issues that are at least somewhat severe and that a significant number of users will hit</td>
  </tr>
  <tr>
    <td>low priority</td>
    <td>issues that have low severity and/or low frequency, cosmetic issues</td>
  </tr>
  <tr>
    <td>no priority</td>
    <td>This is a low priority issue we will leave open but do not intent to fix - please refer to the explanation below.</td>
  </tr>
  <tr>
    <td rowspan="5">External tracking</td>
    <td>codemirror</td>
    <td>needs CM code changes</td>
  </tr>
  <tr>
    <td>webkit</td>
    <td>needs code changes within webkit</td>
  </tr>
  <tr>
    <td>LESS</td>
    <td>needs code changes within LESS module</td>
  </tr>
  <tr>
    <td>cef</td>
    <td>needs code changes in CEF sources</td>
  </tr>
  <tr>
    <td>tracking</td>
    <td> issue is being tracked for example due to dependencies not yet resolved</td>
  </tr>
  <tr>
    <td rowspan="6">Architecturally-focused</td>
    <td>Win only<br/>Mac only<br/>Linux only</td>
    <td>platform specific issue</td>
  </tr>
  <tr>
    <td>architecture</td>
    <td>To fix this issue significant architectural change is required or desired</td>
  </tr>
  <tr>
    <td>async</td>
    <td>asynchronous processing / runtime issue</td>
  </tr>
  <tr>
    <td height="35">code cleanup</td>
    <td>provides an opportunity to do code cleanup, basically, it's a tag that means small, opportunistic refactorings that don't require major cross-cutting changes</td>
  </tr>
  <tr>
    <td>performance</td>
    <td>perceived or measurable performance issue</td>
  </tr>
  <tr>
    <td>native shell</td>
    <td>needs code changes in the native shell</td>
  </tr>
</table>

### Feature requests

For feature requests, please file them in the issue tracker; they'll be converted
to user stories on the [Brackets feature backlog](http://bit.ly/BracketsBacklog).

### Extension bugs
Sometimes a bug is caused by a Brackets extension. Before you file an issue on
the core product you should first rule out all of your installed extensions.
Eventually, Brackets will have an automatic way to enable and disable extensions.
For now it must be done manually:

1. Select _Brackets > Help > Show Extensions Folder_.
2. Move all of the extensions from the `user` directory to the `disabled`
directory, then reload or restart Brackets.
3. If you can still reproduce the issue, file the issue on Brackets.
4. If you can't reproduce the issue, you can test each extension individually
by moving them back to the `user` directory one at a time. You will
have to reload or restart Brackets each time. If you find an extension bug,
please file an issue for the extension so it can be addressed by the extension
author.
4. Make sure all of your extensions are moved back into the `user` directory,
then reload or restart Brackets.


## Retrieving SHA for current commit

You can retrieve the SHA hash for the current commit that you are using by running the following command:

    git rev-parse HEAD


## What happens to the issues I filed?

We're reviewing the new issues regularly the _'last reviewed'_ label is being used to indicate the last bug which has been reviewed. During the review we make sure the appropriate labels are used for the bug. If an issue is related to the work of the current sprint we tag it with the current sprint tag. If it feels rather like an enhancement request we will tag it as _'move to backlog'_. In addition we identify starter issues _'starter bug'_ for individuals who want to start to contribute.

Before you fix a bug, post to the [brackets-dev newsgroup](http://groups.google.com/group/brackets-dev) or the [#brackets IRC channel on freenode](http://webchat.freenode.net/?channels=brackets) about what you're thinking of working on, so you can get early feedback. If you start to contribute code to Brackets please also consider reading some of our wiki documents like [[How to Hack on Brackets]] and [Coding Conventions](Brackets Coding Conventions).

The Brackets team needs to assign or label bugs, therefore we ask you to document the progress to trigger necessary changes along the bug lifecycle.

Once you start to address a bug please let us know and add a note to set it to _'fix in progress'_, this helps to avoid multiplied efforts. Once an issue is fixed a the contributor opens a pull request to inform the core team to get the bug fix reviewed. After potential adjustments according to the review feedback have been added we will merge the bug fix into master. At this point the status of the bug will be set to _'fixed but not closed'_ and the filer will close the issue (or notify the team) once he verified the fix.

### Issues we won't fix
The consensus is that closing some of the low priority issues would only feel appropriate under specific criteria/circumstances, we don't want to end up burying issues which are affecting the quality of Brackets or are otherwise important to the community. We encourage feedback and contributors actually may review and request to re-open issues in case they feel strongly about them.

#### No priority

Items we keep open but will not work on soon. 
<table width="90%" border="1">
<tr>
    <td rowspan="3">No Priority</td>
    <td>High risk / low impact</td>
  </tr>
  <tr>
    <td>High effort / low impact</td>
  </tr>
  <tr>
    <td>Underlying code or architecture is changing / will change soon</td>
  </tr>
</table>

### Issues we close

<table width="90%" border="1">
  <tr>
    <th scope="col">Category</th>
    <th scope="col">Description</th>
  </tr>
  <tr>
    <td rowspan="2">Feature not supported</td>
    <td>Feature is 'as designed' and the request is not an enhancement.</td>
  </tr>
  <tr>
    <td>Feature removed or beyond scope of Brackets</td>
  </tr>
  <tr>
    <td>Not this product</td>
    <td>Feature or issue is not in our product and we have no reasonable way to influence the fix being made</td>
  </tr>
  <tr>
    <td>Intermittent or not reproducible</td>
    <td>basically a flavor of high effort / low impact <br />
( at times we may need additional information to reproduce a bug, those cases don't fall into this category if we're able to gather the information necessary with reasonable effort )</td>
  </tr>
</table>

Thanks a ton for your support and contributions to Brackets!