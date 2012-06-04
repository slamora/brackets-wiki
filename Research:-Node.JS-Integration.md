## Overview

This document discusses the findings from a research spike to understand the benefits and tradeoffs of using Node.JS for native functionality instead of V8 native extensions (our current solution).

The main architectural idea is to implement the Brackets desktop application as a client/server application. The client javascript code would make calls over a websocket to a node server for things like file access and native windowing commands (e.g. adding native menus, opening native file dialogs).

The node server would be embedded in the desktop application (either as an executable that runs as a separate process, or as a dynamic library that is loaded by the app shell).

## Possible Benefits

* Leverage existing Node code for cross-platform native features. For example, Node has cross-platform file and directory watchers.
* Require us to architect our code in a client/server manner (and deal with asynchronicity) so that a browser version of brackets is easier.
* Create an "in-between" solution for optimizing code. For example, Node's file i/o is faster than our V8 extension for file i/o. So, we can make "find in files" faster by moving the existing JavaScript code to the Node side. This requires much less work than implementing "find in files" completely in C+\+ as a V8 extension (which would be the fastest reasonable implementation).
* Would give us an approach for third parties to write extensions that require native code (e.g. an ssh/sftp-based file system), because Node already has a mechanism for loading dynamic libraries.

## Possible Drawbacks

* Need to re-architect some brackets code for client/server approach (but this may pay off in creating a browser version)
* Possibly more complicated debugging (two JS VMs/contexts to debug)
* Increased download size (~8 MB additional) and startup time (rough testing suggests this is small)
* Adds an additional external dependency

## Research Spike Questions

### Can we have comparable solutions on Mac and Windows (and Linux)?

Yes. Using a pre-built Node executable works on both Mac and Windows. Node is run as a separate process and communication happens over pipes.

We are also able to build dynamic library (dylib/dll) versions of Node for both Mac and Windows. Doing this requires a 1-line change to the GYP build files (changing the build type from "application" to "shared_library". Build environments are easy to set up for both Mac (install xcode) and Windows (install latest Visual Studio Express and Python). These dynamic libraries would only need to be built once (and we've already built them).

Everything should work equivalently on Linux (Node works on Linux) but because we don't have any app shell solution for Linux, this is difficult to verify. It is likely that using Node will make our Linux story simpler.

### Should we use a separate process or a dynamic library?

I recommend using a dynamic library as a long term solution. This removes the need for IPC, which will either a.) be brittle, or b.) require a lot of implementation work to get right.

With a dynamic library solution, native functions (e.g. to create menus or show file dialogs) can be called directly from the Node extension code, rather than having to write a bridge that shuttles all arguments as strings.

### Where should native UI code go?

There are two approaches here: using Node, and using V8 extensions. Native UI code (e.g. showing an "open" dialog) typically needs to run in a specific UI thread (e.g. the "main" thread in Mac Cocoa). This poses some interesting challenges.

In CEF3 / Chrome Content Shell, the V8 engine runs in a separate process from the main app. So, IPC is needed to drive native UI. However, CEF3 provides a wrapper for doing this IPC. If we go with a separate process Node solution (discussed above), we'll need IPC for this as well. But, we'll have to write that IPC solution ourselves.

If we a.) decide to go with a separate process Node solution, and b.) if we can be certain that Node code will never need to reach into the main process (e.g. Node code won't ever do UI stuff), then we may be able to avoid writing complicated IPC code for Node. Instead, we can use V8 extensions in CEF3 (and CEF3s IPC solution) to do our native UI work.

However, if we go with an in-process library version of Node  (my current recommendation), we could do all of our UI stuff in Node. This would allow us to avoid using IPC on the V8 side. And, this would have the added architectural benefit that _all_ native stuff (UI and otherwise) would go through the same channel. (This may be a small benefit, though.)

Ultimately, it isn't yet clear what the best solution is. We need to decide whether we'll use an in-process library version of Node or a separate-process executable version of Node.

### How does code architecture and the programming model change?

We built a client <-> server websocket proxy that abstracts the communication layer into regular function calls on both ends. So, if existing functionality is already (correctly) written to work asynchronously, the architecture change is minimal or non-existant.

For example, to switch file i/o from V8 extensions to Node, we simply swapped out the implementation of ```brackets.fs```. The higher-level FS operations (in NativeFileSystem) did not have to change _at all_.

For things that are not currently implemented as asynchronous calls, there will need to be a change. However, it seems likely that everything that would be implemented on the Node side _should_ be implemented asynchronously even if we don't use Node.

The main change to the programming model is that we'll need to decide _where_ to implement features: either on the client JS side or on the server JS side. In general, the client should only implement things that we know will be roughly the same in the desktop and browser versions of Brackets. Anything unique to the desktop version should be implemented (as much as possible) on the Node side.

### How the development process (and debugging) change?

The main change is that there will be two JS engines. These will need to be debugged separately.

Node provides debugging tools -- called [node inspector](https://github.com/dannycoates/node-inspector) -- modeled after the JavaScript portion of WebInspector. These work well with both the separate process version and the dynamic library version of Node.

### Performance

The two main areas of performance concern are _startup time_ (because spinning up Node, an http server, and a websocket server takes time) and _file i/o time_ (because this will be the main use of Node). There are no performance concerns around initiating native UI interactions.

#### Test hardware

All of the performance numbers below were generated on a 2010 MacBook Air with SSD.

#### Startup time

It appears that most of the startup time will be governed by the web component (e.g. CEF) startup time. A warm launching of a simple Cocoa application with Node and a WebKit WebView component takes **250ms**. This time includes starting Node and redirecting the WebView to a webpage served by Node. The timing measurement goes from the first line of ```main()``` to the webpage calling back to Node over a websocket.

Actual startup time measurements will need to be determined after we've decided on a web component solution.

#### File I/O

The most intensive file i/o Brackets is likely to do is a "find in files" command over a large codebase.

To test this, we created a test codebase that consisted of all the js files in the brackets repo (located in the same directory structure). This test set consists of 89743 lines of code at a size of 3.8MB. We performed a search for the regular expression ```/brackets/gi``` which returned \~600 results in \~95 files.

* Existing app shell using V8 extension: **475ms**
* Pulling all files into Brackets client code from Node over websocket: **882ms**
* Moving search code to Node server, only sending results over websocket: **198ms**
* For comparison, using 'grep' at command line: **25ms**

To summarize, naively switching our file i/o to Node results in a **1.85x slowdown**. Moving file intensive JS code to Node (which requires only minimal changes to the code) results in a **2.4x speedup**.

### Extensibility -- can extensions add stuff to Node?

Yes. Loading modules in Node is simpler than in Brackets client code because Node has native support for "require" and direct access to the file system. Modules can be loaded dynamically. We can use the same loading approach (i.e. looking in a directory) as we use for client extensions.

We implemented a prototype extension that adds a "grep" command to the client/server proxy for doing "find in files" on the server. The implementation was straightforward.

### External dependency / state of node.js development

This does represent an additional external dependency. However, the current version of Node seems stable enough for our use, so there isn't a big concern about whether Node will remain in active development.

Furthermore, we can use the distributed (i.e. not built by us) Node executables. They are completely statically linked, so do not require any installation.

Finally, dynamic library versions of node are easy to build (see above).

### Security implications

The Node server maintains an http and websocket server on the user's `localhost` interface. This interface is not accessible from other computers (either on the same network or over the internet).

Another process running on the user's computer could connect to this interface and use it to execute arbitrary code on the user's computer. However, this process could _also_ execute arbitrary code itself. Brackets doesn't run with any special privilege, so this doesn't represent a large security concern.

If the user's computer were running malicious code, this malicious code could also make the localhost interface available over the internet. Again, this doesn't really represent a security concern -- the same malicious code could present a similar interface to the Internet by itself without Brackets present.

## Prototype code

A prototype implementation is checked into the `node-proxy` branches in [adobe/brackets](https://github.com/adobe/brackets/tree/node-proxy) and [adobe/brackets-app](https://github.com/adobe/brackets-app/tree/node-proxy) repos. This prototype only provides Node file i/o and websocket implementation. It does not contain any native ui implementation.

To run this code prototype:

1. Check out both branches
2.  Install node.js
3.  As a temporary hack, edit brackets/src/project/ProjectManager.js line 540 to point to a path that exists on your machine
4.  in brackets-app/src/node, run ```npm install```
5.  in the same directory, run ```node webserver```
6.  in your browser, go to [http://localhost:3000](http://localhost:3000)

Shell prototypes (with native ui implementation) are currently checked in to the [joelrbrandt/node-shell](https://github.com/joelrbrandt/node-shell) repo. (However, the code is a bit messy and there isn't any documentation beyond comments in the source.)