Introduction
====

Brackets officially supports Mac and Windows. However, we're working on adding Linux support with the help of our open source community. If you're an end user that just wants to try out Brackets on Linux, please be aware that many features are missing or partially implemented.

To be clear, if you are NOT an extension developer and NOT planning to contribute to brackets-shell, please visit (TBD landing page) to download an experimental build of Brackets. Please review the release notes for known issues.

Development Environment Setup
====

Required Setup for brackets-shell and brackets
----

**IMPORTANT:** This setup script only works for 32-bit Linux. 64-bit support is in progress, but not ready yet. [Developers have already reported issues](https://groups.google.com/d/msg/brackets-dev/n1wo3aDxNls/Ub58QH_1SbUJ) with this script on 64-bit Linux. DO NOT ATTEMPT.

These instructions will download the Git repositories for [brackets-shell](https://github.com/adobe/brackets-shell) and [brackets](https://github.com/adobe/brackets), download required dependencies, compile the native shell, create and install a debian package, then run Brackets ( ``/usr/bin/brackets`` points to ``/usr/lib/brackets/Brackets``).

1. [Fork brackets](https://github.com/adobe/brackets/fork) and [fork brackets-shell](https://github.com/adobe/brackets-shell/fork) 
2. Create a top level folder to contain the Brackets git repositories
3. In a terminal window, ``cd`` to the folder from the previous step and run the following command
```shell
wget https://gist.github.com/jasonsanjose/5514813/raw/e2e688f0e5151b93a2f2c17c93436bac13d32f35/setup.sh; chmod +x setup.sh; bash setup.sh; rm setup.sh
```
4. You'll be prompted for your GitHub user name to clone your fork of the repositories
5. Respond to ``sudo`` password prompts when requested

*ATTENTION* This setup script will point your git ``origin`` to your own fork, then add a remote ``upstream``and point to the ``upstream/master`` branch in the main git repository. Assuming you're here to contribute to Brackets, you will want to point to your fork later on. This setup script automates the fork and upstream [setup instructions here](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#setting-up-your-dev-environment).

Cache Location
----

```
# cache root
~/.Brackets

# browser cache, local storage, etc.
~/.Brackets/cef_data

# extensions
~/.Brackets/extensions/user
```

Extension Development
----

The extension development workflow is the same as Mac and Windows. Please refer to [How to hack on Brackets](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets) and [How to write extensions](https://github.com/adobe/brackets/wiki/How%20to%20write%20extensions). Please note that the extension location on Linux is ``~/.Brackets/extensions/user``.

Tested Distributions
----

Contributors: If you are willing to test other Linux distributions please add your results here

| Distribution | Version | Issues |
| ------------ | ------- | ----- |
| Ubuntu | 12.04 32-bit | None |
| Ubuntu | 12.04 64-bit | Manually replace brackets-shell/deps/cef/Release/libcef.so with the 64-bit binary from http://www.magpcss.net/cef_downloads/ and rebuild |
| Mint 15 | 64-bit | Manually replace brackets-shell/deps/cef/Release/libcef.so with the 64-bit binary from http://www.magpcss.net/cef_downloads/ and rebuild |
|LMDE(*) | Debian Testing 32 bit |None|
| Arch | 64-bit | https://aur.archlinux.org/packages/brackets-git |

(*) Linux Mint Debian Edition

Building brackets-shell
----

The setup script will automatically create ``/path/to/brackets-shell/Makefile``. If you're only making changes to C++ code, just run ``make`` and the binaries will be in ``/path/to/brackets-shell/out/Release``. 

When adding new files or changing the build configuration, you'll need to make modifications to the GYP configuration files (either ``appshell.gyp.txt`` or ``common.gypi``). After making changes, you'll need to generate a new ``Makefile``. To do this, run:

```
gyp --depth .
```

Packaging Brackets
----

On Mac and Windows we would use the ``grunt installer`` task to build an installer. However, we haven't updated all our Grunt tasks for Linux yet. In the meantime, you can do the following to copy the www source and binaries into a debian package

```
cd /path/to/brackets-shell

# copy shell binaries and www files from brackets repo
grunt copy:linux copy:www copy:samples

# creates debian package installer/linux/brackets.deb
./installer/linux/build_installer.sh
```

User Stories
====

There are several user stories (feature work) to complete in brackets-shell before the Linux version reaches feature parity with Mac and Windows. These stories are listed below in priority order

| User Story | Status | Affected Features | Contact |
| ---------- | ------ | ----------------- | ------- |
| [Update CEF](https://trello.com/c/E8N0Q6dE) | In Progress | Everything | [Jason San Jose](http://github.com/jasonsanjose) |
| [Node Integration](https://trello.com/c/9nX06hWa) | Not Started | Live Preview HTML Highlighting, Extension Manager Install/Update/Remove | [Joel Brandt](http://github.com/joelrbrandt) |
| [Ubuntu Installer/Packaging](https://trello.com/c/ZoCPy6mD) | In Progress | Install experience | [Jason San Jose](http://github.com/jasonsanjose) |
| [File API - delete, rename](https://trello.com/c/WMB6vtwO) | Not Started | Project tree and File menu delete and rename commands | |
| [Native Menus](https://trello.com/c/WMB6vtwO) | Not Started | Menus (HTML menus are an interim, but completely functional substitute) | |
| [Show in OS](https://trello.com/c/RF1ddQGK) | Not Started | Project tree command to show the selected file in the native OS file viewer | |
| [Automated Builds](https://trello.com/c/P35As8lf) | Not Started | | [Jason San Jose](http://github.com/jasonsanjose) |

Development Log
====
* 2013-06-22: Updated ``brackets-shell/linux`` branch with SVG app icon [jasonsanjose](http://github.com/jasonsanjose)
* 2013-06-21: Linux branches land in master in ``brackets-shell`` and ``brackets``. Includes: CEF parity with Mac and Win, stubbed methods for incomplete native shell stories (Node, File I/O, etc.), debian packaging. [jasonsanjose](http://github.com/jasonsanjose)