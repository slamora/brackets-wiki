# Brackets Community Testing

Linux is not the main focus of development team, so we need community testing to remain viable.
But we want to get any help testing that we can, so builds are provided on all platforms.


## Builds

Installer builds will be provided as a [Github Release](https://github.com/adobe/brackets/releases)
using prerelease flag. First set of builds are made from release branch.
Additional builds will be made as necessary (i.e. may not need a new build
if only update are string changes).

Testing using cloned version of Brackets on latest release branch is also welcome.


## Testing Instructions

Community will have a week for testing.

Basic testing instructions are:
- [Client Smoke Tests](https://github.com/adobe/brackets/wiki/Brackets-Smoke-Tests)
- [Server Smokes Tests](https://github.com/adobe/brackets/wiki/Brackets-Server-Smoke-Tests)

A list of new features for release will also be provided
with specific testing instructions, as necessary.

Any other testing is also welcome. Be sure to test new features
in conjunction with your usual workflow.

For testing with a cloned version, please run all unit tests.
TODO: hints for running unit tests...


## Issues

Issues should be created in [github](https://github.com/adobe/brackets/issues)
same as any other bug. Be sure to use **Debug > Reload Without Extensions**
to verify issue is not caused by an extension.

Extensions should also be tested. Issues with extensions should be opened
in the repo of the extension. If the cause of the Issue is in core Brackets,
then the Extension Author will open an Issue in Brackets.


## Results

Community should reply to Brackets-Dev Post with their results:

- OS/Version tested
- Build number from About Dialog
- Amount of time spent
- Links to issues opened
- What was tested, what couldn't be tested & why
- Any other comments


## Feedback

Let us know what more we can do to make testing easier, more effective, etc.
