Introduction
====

Brackets officially supports Mac and Windows. However, we're working on adding Linux support with the help of our open source community. If you're an end user that just wants to try out Brackets on Linux, please be aware that many features are missing or partially implemented.

To be clear, if you are NOT an extension developer and NOT planning to contribute to brackets-shell, please visit (TBD landing page) to download an experimental build of Brackets. Please review the release notes for known issues.

Development Environment Setup
====

These instructions will download the Git repositories for [brackets-shell](https://github.com/adobe/brackets-shell) and [brackets](https://github.com/adobe/brackets), download required dependencies, compile the native shell, stage all the runtime files, then run Brackets.

1. Create a top level folder to contain the Brackets git repositories
2. In a terminal window, ``cd`` to the folder from the previous step and run the following command
```shell
wget -O - https://gist.github.com/jasonsanjose/5514813/raw/d4ed00adfaf5613a629e97a4f9123532e9938928/setup.sh | bash
```
3. Respond to ``sudo`` password prompts when requested
4. When complete, Brackets will launch from ``/path/to/brackets-shell/installer/linux/staging/Brackets`` with ``www`` source (brackets/src) copied to ``/path/to/brackets-shell/installer/linux/staging/www``
5. To setup for Brackets development, we would normally use ``/path/to/brackets/tools/setup_for_hacking.sh`` to redirect the Brackets binary at the brackets git repository. There's still some open issues there, so instead, just manually run the following
```shell
ln -s /path/to/brackets /path/to/brackets-shell/installer/linux/staging/dev
```

Tested Distributions
----

Contributors: If you are willing to test other Linux distributions please add your results here

| Distribution | Version | Issues |
| ------------ | ------- | ----- |
| Ubuntu | 12.04 32-bit | None |
| Ubuntu | 12.04 64-bit | Manually replace brackets-shell/deps/cef/Release/libcef.so with the 64-bit binary from http://www.magpcss.net/cef_downloads/ and rebuild |

User Stories
====

There are several user stories (feature work) to complete in brackets-shell before the Linux version reaches feature parity with Mac and Windows. These stories are listed below in priority order

| User Story | Status | Affected Features | Contact |
| ---------- | ------ | ----------------- | ------- |
| [Update CEF](https://trello.com/c/E8N0Q6dE) | In Progress | Everything | [Jason San Jose](http://github.com/jasonsanjose) |
| [Node Integration](https://trello.com/c/9nX06hWa) | Not Started | Live Preview HTML Highlighting, Extension Manager Install/Update/Remove | [Joel Brandt](http://github.com/joelrbrandt) |
| [Ubuntu Installer/Packaging](https://trello.com/c/ZoCPy6mD) | Not Started | Install experience | |
| [File API - delete, rename](https://trello.com/c/WMB6vtwO) | Not Started | Project tree and File menu delete and rename commands | |
| [Native Menus](https://trello.com/c/WMB6vtwO) | Not Started | Menus (HTML menus are an interim, but completely functional substitute) | |
| [Show in OS](https://trello.com/c/RF1ddQGK) | Not Started | Project tree command to show the selected file in the native OS file viewer | |
| [Automated Builds](https://trello.com/c/P35As8lf) | Not Started | | [Jason San Jose](http://github.com/jasonsanjose) |

Building brackets-shell
====

The setup script will automatically create ``/path/to/brackets-shell/Makefile``. If you're only making changes to C++ code, just run ``make`` and the binaries will be in ``/path/to/brackets-shell/out/Release``. 

When adding new files or changing the build configuration, you'll need to make modifications to the GYP configuration files (either ``appshell.gyp.txt`` or ``common.gypi``). After making changes, you'll need to generate a new ``Makefile``. To do this, run:

```
gyp --depth .
```

Packaging Brackets
====

Currently, we do not have a way to create an Ubuntu package suitable for end users to install. In the meantime, we can assemble all the required files necessary to run Brackets with the following script:

```
# copy required brackets-shell and brackets files to /path/to/brackets-shell/installer/linux/staging
/path/to/brackets-shell > ./installer/linux/stage_for_packaging.sh
```

This staging folder can be zipped and distributed as a "portable" install that the runtime dependencies are addressed separately.

FAQ
====

1. Why isn't Linux distribution X supported?
We're currently focused on Ubuntu since [Chromium Embedded Framework (CEF)](https://code.google.com/p/chromiumembedded/) is well tested there both in a development environment and in deployment. We're open to contributors helping us support more distributions.