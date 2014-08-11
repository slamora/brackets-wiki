None of the Brackets core team currently runs Linux day-to-day, so we need
Linux community testing to ensure that Brackets releases are high quality on Linux.
Linux is not the main focus for Adobe, but if we had some Linux people from the community
step up and become committers at some point and take more ownership of the project,
the future of Linux support would be ensured. I think this community testing work
you’re doing could lay the foundation for that!

All help from community is welcome, so we provide builds on all platforms.


## Builds

Installer builds will be provided as a [Github Release](https://github.com/adobe/brackets/releases)
using prerelease flag. First set of builds are made from release branch.
Additional builds will be made as necessary (i.e. may not need a new build
if only update are string changes).

Testing using cloned version of Brackets on latest release branch is also welcome.


## Testing Instructions

Community will have a week for testing. Brackets Team will post to Brackets-Dev Forum with links to builds, testing dates, and other details.

Basic testing instructions are:
- [Client Smoke Tests](https://github.com/adobe/brackets/wiki/Brackets-Smoke-Tests)
- [Server Smokes Tests](https://github.com/adobe/brackets/wiki/Brackets-Server-Smoke-Tests)

A list of new features for release will also be provided
with specific testing instructions, as necessary.

Any other testing is also welcome. Be sure to test new features
in conjunction with your usual workflow.

For testing with a cloned version, please [run all unit tests](https://github.com/adobe/brackets/wiki/Running-Brackets-Unit-Tests).


## Issues

Issues should be created in [github](https://github.com/adobe/brackets/issues)
same as any other bug. Be sure to use **Debug > Reload Without Extensions**
to verify issue is not caused by an extension.

Extensions should also be tested. Issues with extensions should be opened
in the repo of the extension. If the cause of the Issue is in core Brackets,
then the Extension Author will open an Issue in Brackets.


## Results

Community should reply to Brackets-Dev Post with their results. Please provide info such as:

- OS/Version tested
- Build number from About Dialog
- If any issues were opened, let us know your github id for easy lookup
- What was tested
- What couldn't be tested & why
- Any other comments


## Feedback

Let us know what more we can do to make testing easier, more effective, etc.
