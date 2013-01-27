# Testing Pritam's Linux build

There has been a lot of interest in the Brackets Community around getting Brackets running on Linux. The 
[Linux Version thread in the brackets-dev forum](https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/29vOJ6tvl8A[101-125-false]) got so large and hard to follow that I created this wiki page to summarize the info so far, and also as a collecting point for more info. The following info was copied from the post by [Pritam Baral](https://github.com/pritambaral) for his Linux build.

## Setting up Brackets

With reasonable confidence I can say Brackets for Linux is ready for testing. Two builds (32 and 64 bit respectively) have been posted: [pritambaral/brackets-shell/downloads](https://github.com/pritambaral/brackets-shell/downloads). The core brackets code from Sprint 16 has also been packaged and posted on the same page. A tester may chose to build it for himself, or download the brackets-shell-[64,32] and brackets-sprint-16 builds and run it.

Both brackets-shell and brackets are packaged up as brackets-shell-[32,64]-bit.tar.bz2 and brackets-sprint-16.tar.bz2 respectively on the downloads page. Download both and extract to the proper directory. The folders www/ and samples/ being in the same folder as the Brackets binary is preferable, but you extract anywhere and manually choose the location of www/index.html on start-up.

_[Note from Randy Edmunds: it wouldn't work for me on OpenSuse 12.1 when I put these folders side-by-side, but it did work when I put the www/ and samples/ being in the same folder as the Brackets executable]_

Or, if you want to test the building process, the linux branches on both [pritambaral/brackets](https://github.com/pritambaral/brackets) and [pritambaral/brackets-shell](https://github.com/pritambaral/brackets-shell) are the ones to use.

## Testing Brackets

We want to get an idea of how complete and stable this build is.

Open Brackets **Unit Tests** window using: Debug &gt; Run Tests. Note that Unit Tests are disabled if you use the www folder download. Run all Unit and Extension tests.

Also run Brackets **Smoke Tests** for both [Client](https://github.com/adobe/brackets/wiki/Brackets-Smoke-Tests) and [Server, if possible](https://github.com/adobe/brackets/wiki/Brackets-Server-Smoke-Tests).

To help us get an idea of what testing has been done, please update this table after you have completed some testing:

<table cellspacing="0">
<tr>
  <th>Contact info</th>
  <th>OS/version</th>
  <th>Unit Tests pass?</th>
  <th>Smoke tests pass?</th>
  <th>Comments</th>
</tr>
<tr>
  <td><a href="mailto:redmunds@adobe.com">Randy Edmunds</a>, github.com/redmunds</td> 
  <td>OpenSUSE 12.1</td>
  <td></td>
  <td></td>
  <td>In progress...</td>
</tr>
<tr>
  <td><a href="mailto:pabloluisbotta@gmail.com">Pablo Botta</a>, github.com/p4bl1t0</td> 
  <td>LMDE  Up to date</td>
  <td>Unable to run it {1}</td>
  <td>In progress</td>
  <td>Using Pritam 32 bits builds<br />"Show Extension Folder" - Don't open extension folder</td>
</tr>
<tr>
  <td></td> 
  <td></td>
  <td></td>
  <td></td>
  <td></td>
</tr>
</table>

{1} I have copied test folder to the executable's folder and that enabled "Run Tests". But when I click on it and go to the test window nothing happened there. The tests don't run. Maybe I'm missing something.

Use [pritambaral/brackets/issues](https://github.com/pritambaral/brackets/issues) for logging issues.

## Bug Fixing

Grab issues from [pritambaral/brackets/issues](https://github.com/pritambaral/brackets/issues).
