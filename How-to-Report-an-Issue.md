<h1>How to deal with issues </h1>
<p>&nbsp;</p>
<p>You tested Brackets and found an issue - now you want to file it – that's great!<br /> 
While you help us to improve the quality of Brackets, please keep a few steps in mind to help us manage issues in Brackets.
Bugs are tracked in the <a href="https://github.com/adobe/brackets/issues" target="_blank">GitHub issue tracker.</a></p>
<h2>Existing issues, feature backlog</h2>
<p>Please review the existing issues – GitHub provides a search field to search for Issues and Milestones, found in the top right:</p> 
![search in github](https://github.com/adobe/brackets/wiki/screenshots/issues_search.jpg)
<p>Typing into the serch field will show related issues, this illustrates the importance of choosing keywords within titles, such that others don't file duplicates for your issue. Some of the issues – basically enhancement requests – are moved to our <a href="https://trello.com/b/Slx4ibBw" target="_blank">public backlog</a>, please consider to search through the existing user stories too. There you most likely will find the missing features, enhancements to existing features, etc.</p>
<p>If you find an issue which seems to cover your case you may add comments or scenarios which are specific to your workflow or content. You can also vote for backlog items to help us prioritize.</p>
<h2>How to file an issue or feature request</h2>
<p>Filing an issue is simple, click on the &quot;<strong><em>New Issue</em></strong>&quot; button, add a title and a description. We want to provide some guidance regarding which information should be contained in the description, how we consider to set priority, and how we make use of labels in GitHub.<br />
  Thanks for supporting our project and other contributors - To enhance reproducibility we ask you to provide information necessary to understand and confirm issues.<br />
<ul><li>Brackets version/sprint number (please use sprint labels or commit SHA if you're pulling directly from the repo (see below)).</li>
    <li>Platform/OS version (please use labels for Windows / Mac)</li>
    <li>Reproducible steps, actual and expected results</li>
    <li>Link to test files (you can create a gist on gist.github.com if that's convenient).</li>
</ul>
      <h3>Determine the priority</h3>
      <p>We differentiate three priorities for issues, we use labels on GitHub to set the priority:
    
      </h3>
  <ul>
  <li><em>High</em>: crashes/data loss unless they're very unlikely edge cases</li>
    <li><em>Medium</em>: functional/ui issues that are at least somewhat severe and that a significant number of users will hit</li>
    <li><em>Low</em>: issues that have low severity and/or low frequency</li>
</ul> </p>
<h3>Use of lables for issues found in Brackets</h3>
<table width=90% border="5" cellpadding="1" bordercolor="#000033">
  <tr>
    <th width=15% scope="col">group</th>
    <th width=15% scope="col">labels</th>
    <th width=60% scope="col">usage</th>
  </tr>
  <tr>
    <td rowspan="8">Process </td>
    <td>fix in progress</td>
    <td>Bug has been assigned and assignee has started to make improvements</td>
  </tr>
  <tr>
    <td>fixed but not closed</td>
    <td>issue has been fixed but not merged, may need to be regressed</td>
  </tr>
  <tr>
    <td height="40">last reviewed</td>
    <td>maker for last reviewd bug, see below for explanation of the review process</td>
  </tr>
  <tr>
    <td>move to backlog</td>
    <td>enhancement  marker - to be put  on the <a href="https://trello.com/b/Slx4ibBw" target="_blank">backlog</a></td>
  </tr>
  <tr>
    <td>sprintN</td>
    <td>tag for issues to be adressed in sprint N</td>
  </tr>
  <tr>
    <td>starter bug</td>
    <td>This issue doesn’t require deep knowledge of the architecture and is the recommended level for new contributors</td>
  </tr>
  <tr>
    <td>won't fix</td>
    <td>This is a low priority issue we close without a fix - please refer to the explanation below.</td>
  </tr>
  <tr>
    <td>documentation</td>
    <td>insufficient documentation</td>
  </tr>
  <tr>
    <td rowspan="3">Priority</td>
    <td>high priority</td>
    <td>crashes/data loss unless they're very unlikely edge cases</td>
  </tr>
  <tr>
    <td>low priority</td>
    <td>issues that have low severity and/or low frequency, cosmetic issues</td>
  </tr>
  <tr>
    <td>medium priority</td>
    <td>functional/ui or performance issues that are at least somewhat severe and that a significant number of users will hit</td>
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
    <td>Win only / Mac only</td>
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
    <td>preceived or measurable performance issue</td>
  </tr>
  <tr>
    <td>native shell</td>
    <td>needs code changes in the native shell</td>
  </tr>
</table>

<h2>What happens to the issues I filed?</h2>
<p>We're reviewing the new issues regularily the <strong>'<em>last reviewed</em>'</strong> label is being used to indicate the last bug which has been reviewed. During the review we make sure the appropiate lables are used for the bug. If an issue is related to the work of the current sprint we tag it with the current sprint tag. If it feels rather like an enhancement request we will tag it as <strong>'<em>move to backlog</em>'</strong>. In addition we identify starter issues <strong>'<em>starter bug</em>'</strong> for individuals who want to start to contribute. </p>
<p>Before you fix a bug, post to the <a href="http://groups.google.com/group/brackets-dev">brackets-dev Google group</a> or the <a href="http://freenode.net/">#brackets IRC channel on freenode</a> about what you're thinking of working on, so you can get early feedback. If you start to contribute code to Brackets please also considder reading some of our <a href="https://github.com/adobe/brackets/wiki/">wiki documents</a> like <a href="https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets">How to Hack on Brackets</a> and <a href="https://github.com/adobe/brackets/wiki/Brackets-Coding-Conventions">Coding Conventions</a>.</p>
<p>The Brackets team needs to assign or label bugs, therefore we ask you to document the progress to trigger necessary changes along the bug lifecycle.</p>
<p>Once you start to adress a bug please let us know and add a note to set it to <strong>'<em>fix in progress</em>'</strong>, this helps to avoid multiplied efforts. Once an issue is fixed a the contributor opens a <em><strong>'pull request'</strong></em> to inform the core team to get the bug fix reviewed. After potential adjustments according to the review feedback have been added we will <em><strong>'merge</strong></em><strong>'</strong> the bug fix into master. At this point the status of the bug will be set to <strong>'<em>fixed but not closed</em>'</strong> and the filer will  <strong>'<em>close</em>'</strong> the issue (or notify the team) once he verified the fix.</p>

<h3>Issues we won't fix</h3>
<p>The consensus is that closing some of the low priority issues would only feel appropriate under specific criteria/circumstances, we don't want to end up burying issues which are affecting the quality of Brackets or are otherwise important to the community. We encourage feedback and contributors actually may review and request to re-open issues in case they feel strongly about them. Marking the issue with a label enables us to search for and to review issues at any point in time.</p>
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
    <td rowspan="3">Not worth effort</td>
    <td>High risk / low impact</td>
  </tr>
  <tr>
    <td>High effort / low impact</td>
  </tr>
  <tr>
    <td>Underlaying code or architecture is changing / will change soon</td>
  </tr>
  <tr>
    <td>Intermittent or not reproducible</td>
    <td>basically a flavor of high effort / low impact <br />
( at times we may need additional information to reproduce a bug, those cases don't fall into this category if we're able to gather the information necessary with reasonable effort )</td>
  </tr>
</table>

<h3>Feature requests</h3>
For feature requests, please file them in the issue tracker; they'll be converted
to user stories on the [public Brackets backlog](https://trello.com/board/brackets/4f90a6d98f77505d7940ce88).

## Retrieving SHA for current commit

You can retrieve the SHA hash for the current commit that you are using by running the following command:

    git rev-parse HEAD

<p>Thanks a ton for your support and contributions to Brackets!</p>