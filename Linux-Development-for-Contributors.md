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
4. When complete, Brackets will launch from ``/path/to/brackets-shell/installer/linux/staging/Brackets`` with www source copied to ``/path/to/brackets-shell/installer/linux/staging/www
5. To setup for Brackets development, we would normally use ``/path/to/brackets/tools/setup_for_hacking.sh`` to redirect the Brackets binary at the brackets git repository. There's still some open issues there, so instead, just manually run the following
```shell
ln -s /path/to/brackets /path/to/brackets-shell/installer/linux/staging/dev
``` 

Tested Distributions
----

* Ubuntu 12.04 32-bit
* Ubuntu 12.04 64-bit (manually replace brackets-shell/deps/cef/Release/libcef.so with the 64-bit binary from http://www.magpcss.net/cef_downloads/)