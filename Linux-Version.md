A Linux version of Brackets has [always been on the roadmap](https://trello.com/card/linux-desktop-application/4f90a6d98f77505d7940ce88/457) but there is currently no official Brackets build for Linux. Recently however the community has started work on the Linux version. This wiki will serve as a resource for anyone wanting to work on or use Brackets in Linux. 

### The Challenge

The main challenge with a Linux version of Brackets is that Brackets uses the [Chromium Embedded Framework version 3 (CEF3)](http://code.google.com/p/chromiumembedded/) for [brackets-shell](https://github.com/adobe/brackets-shell/), the native wrapper that hosts Brackets. There is currently no Linux binary for CEF3 and building it requires some manual work.

## Building a Linux Version of Brackets

### Building CEF3 on Linux

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

Where should they report issues?