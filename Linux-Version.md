A Linux version of Brackets has [always been on the roadmap](https://trello.com/card/linux-desktop-application/4f90a6d98f77505d7940ce88/457) but there is currently no official Brackets build for Linux. Recently however the community has started work on the Linux version. This wiki will serve as a resource for anyone wanting to work on or use Brackets in Linux. 

### Current Status

* There's an [unofficial build (see below)](#wiki-pritam-build) based on the older Sprint 16 release of Brackets. It's possible to get this build working with newer Brackets source, but some functionality will be missing.
* **We need your help** to get newer builds working and create a more permanent, maintainable Linux build process. See steps below to [hack together a Linux build](#wiki-building) and get started tinkering.
* See ["Issues and Workarounds" at the bottom of this page](#wiki-issues) for more tips.

### The Challenge

Brackets uses the [Chromium Embedded Framework version 3 (CEF3)](http://code.google.com/p/chromiumembedded/) for [brackets-shell](https://github.com/adobe/brackets-shell/), the native wrapper that hosts Brackets. There is currently no Linux binary for CEF3 and building it requires some manual work.

In addition, the brackets-shell project includes several extensions to CEF to enable desktop functionality (file I/O, native menus, launching Chrome for Live Preview, managing the built-in Node.js server, drag-and-drop, etc.). The code for these extensions is mostly OS-specific and will need to be ported to Linux. Some was ported in Pritam's Sprint 16 build, but a significant amount of this platform-specific code was added _after_ Sprint 16.

We also need to create an automated build process for Linux, paralleling the build scripts already in place for Windows & Mac. This will make it possible keep the Linux version up to date as new Brackets sprints are released twice a month.


## <a name="pritam-build"></a> Older Unofficial Build (by Pritam Baral)

[Pritam Baral](https://github.com/pritambaral) put a bunch of effort into porting brackets-shell to Linux, and has posted pre-built binaries for download. To use:

* Clone the main repo https://github.com/adobe/brackets.git
* Download the latest binary from Pritam's downloads page: https://github.com/pritambaral/brackets-shell/downloads (looks like Brackets-shell-....bz2)
* Extract the binary.
* Symlink in "brackets/src" (from the main repo) as "www" in the directory with the Brackets binary
* Also symlink in the "samples" directory
* Run ./Brackets

Aka, in shell terms:

```bash
mkdir ~/brackets && cd ~/brackets
git clone https://github.com/adobe/brackets.git
cd brackets
git submodule update --init
cd ..
wget https://github.com/downloads/pritambaral/brackets-shell/Brackets-shell-32.tar.bz2
tar xf Brackets-shell-32.tar.bz2 && rm Brackets-shell-32.tar.bz2
ln -s brackets/src www
ln -s brackets/samples ./samples
./Brackets
```

This is known to work on 32 bit Ubuntu 12.10, at the least.

### Out of date

Be forewarned: these brackets-shell builds are based on Brackets Sprint 16, which dates to early November 2012. To get Brackets source that is _guaranteed_ to work with such an old shell, you'd need to `git checkout sprint-16` in your brackets repo.

[See workarounds below](#wiki-issues) to get these shell builds working with the _latest_ Brackets source.


## <a name="building"></a> Building Brackets for Linux

### Modifying the Brackets Shell

Pritam Baral has a [modified version of the Brackets Shell](https://github.com/pritambaral/brackets-shell/tree/linux) that works on Linux. He has also created instructions for building brackets-shell on Linux which are included below.

####Prerequisites

* make, g++, GTK-2.0 and glib-2.0 development headers & libraries
  * The linux branch on this repo. Not yet merged into master.
* gyp (Separately downloadable. Packaged in Ubuntu and Debian by default. Also provided in Chromium source code `src/tools/gyp/`)
  * If you have build-deps of Chromium installed, you don't need anything extra
* [CEF3 binary distribution](http://github.com/pritambaral/brackets-shell/downloads)  version 3.1271 (recommended) 
* Nothing more is required to modify the project files.

####Setup and Building

* Create a folder named `deps` inside the `brackets-shell` folder.
* Create a folder named `cef` inside the `deps` folder.
* Copy all of the contents of the [CEF3 binary distribution](#wiki-cef) into the `deps/cef` directory. 

Your directory structure should look like this:
```
brackets-shell
   deps
      cef
         // CEF3 binary content in this folder
   appshell
      // appshell source
   README.md
   ...
```

* Open a terminal window on the `brackets-shell` directory and run `scripts/make_symlinks.sh`. This will create symbolic links to several folders in the `deps/cef` directory.

* run the following commands in the `brackets-shell` directory:
```
gyp --depth .
make
```

A successful build will be placed in `out/Release/` directory (`out/Debug` for a Debug build)

####Running
Brackets should automatically scan for `www/index.html` in it's own directory. If it doesn't find one, you will be prompted to select an `index.html` file. Navigate to your local copy of the brackets repo and select `src/index.html`.

**[Workarounds are needed](#wiki-issues)** for functionality like menus and Live Preview to work correctly.

####Generating Projects
This is only required if you are changing the project files. **NOTE:** Don't change the Makefiles directly. Any changes should be done to the .gyp files, and new Makefiles should be generated.

* Open a terminal window on this directory and run <code>gyp --depth .</code>

## <a name="cef"></a> CEF 3 on Linux

### Download a Build

**Update 4/22/2013** Linux binary builds are now available here http://www.magpcss.net/cef_downloads/

> Old binary download: [cef_binary_3.1364.1131_linux.zip](https://docs.google.com/file/d/0B7as0diokeHxeTNqZFIyNWZKSWM/edit?usp=sharing)

### Build from Source

The CEF project page has several paths to building CEF for Linux. The one we've had success with begins here [https://code.google.com/p/chromiumembedded/wiki/BranchesAndBuilding#Release_Branch](https://code.google.com/p/chromiumembedded/wiki/BranchesAndBuilding#Release_Branch). Follow the steps outlined there. Additional notes and example steps are shown below

**Step 1**: **SKIP** http://dev.chromium.org/developers/how-tos/get-the-code - Using the tarball to bootstrap the setup does not seem to work for building a specific release branch. Instead, follow step 1, parts A through C.

**Step 2**: Identify the Chromium release branch based on the CEF release version (e.g. 1364 from CEF version  3.**1364**.1094 from [CHROMIUM_BUILD_COMPATIBILITY](https://code.google.com/p/chromiumembedded/source/browse/branches/1364/cef3/CHROMIUM_BUILD_COMPATIBILITY.txt))

```
# For CEF 3.1364.1094

# download source
mkdir chromium
cd chromium
gclient config http://src.chromium.org/svn/releases/25.0.1364.152
gclient sync --jobs 8 --force
cd src
# (optional?) ./build/install-build-deps.sh --no-chromeos-fonts
svn co http://chromiumembedded.googlecode.com/svn/branches/1364/cef3 cef

# create projects
cd cef
./cef_create_projects.sh

# build both Debug and Release (both required for distrib)
cd tools
./build_projects.sh Debug
./build_projects.sh Release
./make_distrib.sh

# distribution saved to chromium/src/cef/binary_distrib/cef_binary_3.1364.1131_linux.zip
```


## <a name="issues"></a> Issues and Workarounds

#### Re-enable HTML menus
Current Linux shell builds lack the native menu APIs that Brackets Sprint 19+ expect. To use HTML-based menus instead:

* In brackets.js, remove the conditional around `$("body").addClass("in-browser");` so that line always runs (and `$("body").addClass("in-appshell");` never runs).
* In Menus.js, change `_isHTMLMenu()` so it always returns true.


#### Get Live Preview to work

From Marcus Clearspring [on the mailing list](https://groups.google.com/d/msg/brackets-dev/K26IkouXAq0/L65r-auzNzcJ):

To answer my own question, here's how you get Live Preview working under Linux, courtesy of Pritam. The problem is that the Chrome executables have different names under various Linux distros.

The solution is to symlink the Chrome executable on your system to what Brackets expects, being "google-chrome". On Ubuntu, the executable is called chromium-browser.

Here's the command for Ubuntu. You will need to execute the command as root in most cases, hence sudo on Ubuntu. Adapt to your distro as needed:

`sudo ln -s /usr/bin/chromium-browser /usr/bin/google-chrome`

That should be the last step to getting all Brackets features working on Linux.