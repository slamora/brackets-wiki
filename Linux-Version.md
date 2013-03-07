A Linux version of Brackets has [always been on the roadmap](https://trello.com/card/linux-desktop-application/4f90a6d98f77505d7940ce88/457) but there is currently no official Brackets build for Linux. Recently however the community has started work on the Linux version. This wiki will serve as a resource for anyone wanting to work on or use Brackets in Linux. 

### Using the Unofficial Build (by Priam Baral)

* clone the main repo https://github.com/adobe/brackets.git
* download the latest binary from Pritam's downloads page: https://github.com/pritambaral/brackets-shell/downloads (looks like Brackets-shell-....bz2)
* extract the binary.
* symlink in "brackets/src" (from the main repo) as "www" in the directory with the Brackets binary
* also symlink in the "samples" directory
* run ./Brackets

The shell of it looks like:

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

[this worked for me on 32 bit Ubuntu 12.10]

### The Challenge

The main challenge with a Linux version of Brackets is that Brackets uses the [Chromium Embedded Framework version 3 (CEF3)](http://code.google.com/p/chromiumembedded/) for [brackets-shell](https://github.com/adobe/brackets-shell/), the native wrapper that hosts Brackets. There is currently no Linux binary for CEF3 and building it requires some manual work.

## Building a Linux Version of Brackets

### Building CEF3 on Linux

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
svn co http://chromiumembedded.googlecode.com/svn/branches/1364/cef3 cef

# create projects
cd cef
./cef_create_projects.sh

# build both Debug and Release (both required for distrib)
cd tools
./build_projects.sh Debug
./build_projects.sh Release
./make_distrib.sh
```

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

Create a folder named `deps` inside the `brackets-shell` folder.
Create a folder named `cef` inside the `deps` folder.
Copy all of the contents of the CEF3 binary distribution into the `deps/cef` directory. 

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

Open a terminal window on the `brackets-shell` directory and run `scripts/make_symlinks.sh`. This will create symbolic links to several folders in the `deps/cef` directory.

run the following commands in the `brackets-shell` directory:
```
gyp --depth .
make
```

A successful build will be placed in `out/Release/` directory (`out/Debug` for a Debug build)

####Running
Brackets should automatically scan for `www/index.html` in it's own directory. If it doesn't find one, you will be prompted to select an `index.html` file. Navigate to your local copy of the brackets repo and select `src/index.html`.

####Generating Projects
This is only required if you are changing the project files. **NOTE:** Don't change the Makefiles directly. Any changes should be done to the .gyp files, and new Makefiles should be generated.

* Open a terminal window on this directory and run <code>gyp --depth .</code>

### Modifying Brackets Core

Any changes needed?

## Distributions 

A list of working distributions and download locations?

## Resources

Pritam's repo? 

## Issues

#### Getting Live Preview to Work

From Marcus Clearspring [on the mailing list](https://groups.google.com/d/msg/brackets-dev/K26IkouXAq0/L65r-auzNzcJ):

To answer my own question, here's how you get Live Preview working under Linux, courtesy of Pritam. The problem is that the Chrome executables have different names under various Linux distros.

The solution is to symlink the Chrome executable on your system to what Brackets expects, being "google-chrome". On Ubuntu, the executable is called chromium-browser.

Here's the command for Ubuntu. You will need to execute the command as root in most cases, hence sudo on Ubuntu. Adapt to your distro as needed:

`sudo ln -s /usr/bin/chromium-browser /usr/bin/google-chrome`

That should be the last step to getting all Brackets features working on Linux.