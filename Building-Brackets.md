(work in progress)

# Introduction

This page documents build steps for Brackets that are typically only run on the build servers when building installers.

To on-board new contributors quickly, we omit a few non-essential setup steps in our [How to Hack on Brackets](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets) instructions. Notably, we don't ask contributors to install Node.js or Grunt.

Also, since most contributors work on the core Brackets repository (HTML/JS/CSS code base), they have no need to build the native shell, see [Building Brackets Shell](https://github.com/adobe/brackets-shell/wiki/Building-Brackets-Shell) for more details. The hacking instructions outline how to point an existing shell (usually from the most recent release) to the raw source from GitHub.

# Prerequisites

* Git command line tools - follow the setup instructions [on GitHub](https://help.github.com/articles/set-up-git) or download [here](http://git-scm.com/downloads)
* [Node.js](http://nodejs.org/)
* [Grunt](http://gruntjs.com/getting-started/)
* Run `npm install` at the root of the Brackets repository

# Build

Run `grunt build` to compile the Brackets `src` tree to the `dist` folder.

# Usage

After a successful build, ...