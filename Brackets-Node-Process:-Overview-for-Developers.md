**Status:** Implemented in Sprint 21. Document last updated 3/1/13.

Overview
--------

Starting with Sprint 21, the Brackets has a [Node.js](http://nodejs.org/) process integrated into the shell. The purpose of this document is to give a brief overview of the architecture as well as pragmatic examples of how to develop for and debug code that leverages node.

For more information on _why_ we've integrated Node in the way we have, take a look at [[Research: Node.JS Integration]].

###API Stability

The API is expected to change dramatically over the next few sprints (starting with Sprint 22). Brave developers are encouraged to use this extensibility mechanism and give us feedback. However, things may break frequently.

We will message any API changes via the [Brackets-Dev Google Group](https://groups.google.com/forum/?fromgroups#!forum/brackets-dev) and update these docs accordingly.

Architecture
------------

Usage Example
-------------

Here we walk through how to implement a _very simple_ domain that can be accessed from the client. This domain allows client code to retrieve system memory usage information.

The entire example is available as a Brackets extension at https://github.com/joelrbrandt/brackets-simple-node

### Step 0: Setting up the extension.

A brackets extension lives in a single folder, and its entry point is main.js. By convention (but not by necessity), we put all node code in a directory called 'node'.

**Note on third-party modules:** Convention recommends putting any third-party modules inside a "node_modules" directory that lives inside the "node" directory. For development, node modules can be installed through npm by putting a "package.json" file in the "node" directory and running ```npm install``` from inside that directory. By doing this (and adding "node/node_modules" to your .gitignore) you can avoid checking third party code into your repo. For _distribution_, the actual bits of any third-party modules should be bundled in to the zip. (This is recommended practice in the node community to ensure that all end users get the same bits. See http://www.futurealoof.com/posts/nodemodules-in-git.html )

### Step 1: Implementing the domian.

All of our node code will live in a file called "node/SimpleDomain.js". The first thing we need to do in this file is require the built-in libraries we'll be using:

```javascript
var os = require("os");
```

Next, we need to implement a command handler that will actually get the data we want.

```javascript
/**
 * @private
 * Handler function for the simple.getMemory command.
 * @return {{total: number, free: number}} The total and free amount of
 *   memory on the user's system, in bytes.
 */
function cmdGetMemory() {
    return {total: os.totalmem(), free: os.freemem()};
}
```

When a domain gets registered, the DomainManager calls an init function (if one exists). We need to write our init function that will actually register the command we just created.

When we register a command, we can optionally pass in documentation. This documentation isn't actually used by node in any way. But, it is output through the "api" call and could be used to actually build human-readable documentation that is (hopefully) in sync with the code. The last three parameters to ```registerCommand``` are documentation parameters.

```javascript
/**
 * Initializes the test domain with several test commands.
 * @param {DomainManager} DomainManager The DomainManager for the server
 */
function init(DomainManager) {
    if (!DomainManager.hasDomain("simple")) {
        DomainManager.registerDomain("simple", {major: 0, minor: 1});
    }
    DomainManager.registerCommand(
        "simple",       // domain name
        "getMemory",    // command name
        cmdGetMemory,   // command handler function
        false,          // this command is synchronous
        "Returns the total and free memory on the user's system in bytes",
        [],             // no parameters
        [{name: "memory",
            type: "{total: number, free: number}",
            description: "amount of total and free memory in bytes"}]
    );
}
```

Finally, we need to actually export this init function so the DomainManager can call it.
```javascript
exports.init = init;
```

That's it! The domain is now implemented. Note that in the full version in the github repo mentioned earlier, the entire file is wrapped in an anonymous function that is immediately called. This is so we can add the "use strict" pragma once. There's no other significance of that code

### Step 2: Implementing the client code

Our client code is simple, too: At appReady time, our code will:

1. Connect to node
2. Load the domain we just created in the previous step
3. Call the logMemory command that we just created, and log the data to the console

Of course, this is marginally complicated because every step is asynchronous. To make the code cleaner, we start by defining a helper "chain" function that chains together promise-returning functions.

```javascript
// Helper function that chains a series of promise-returning
// functions together via their done callbacks.
function chain() {
    var functions = Array.prototype.slice.call(arguments, 0);
    if (functions.length > 0) {
        var firstFunction = functions.shift();
        var firstPromise = firstFunction.call();
        firstPromise.done(function () {
            chain.apply(null, functions);
        });
    }
}
```

Note that this is a _simple_ chain helper, not a _robust_ chain helper, and is merely present to make the rest of the code simpler.

Next, we need setup our node connection. We start by getting the NodeConnection module:

```javascript
NodeConnection = brackets.getModule("utils/NodeConnection");
```

And then by making a new connection object:

```javascript
var nodeConnection = new NodeConnection();
```

Now, we need some helper functions to do the connection, domain loading, and command calling:

```javascript
// Helper function to connect to node
function connect() {
    var connectionPromise = nodeConnection.connect(true);
    connectionPromise.fail(function () {
        console.error("[brackets-simple-node] failed to connect to node");
    });
    return connectionPromise;
}

// Helper function that loads our domain into the node server
function loadSimpleDomain() {
    var path = ExtensionUtils.getModulePath(module, "node/SimpleDomain");
    var loadPromise = nodeConnection.loadDomains([path], true);
    loadPromise.fail(function () {
        console.log("[brackets-simple-node] failed to load domain");
    });
    return loadPromise;
}

// Helper function that runs the simple.getMemory command and
// logs the result to the console
function logMemory() {
    var memoryPromise = nodeConnection.domains.simple.getMemory();
    memoryPromise.fail(function (err) {
        console.error("[brackets-simple-node] failed to run simple.getMemory", err);
    });
    memoryPromise.done(function (memory) {
        console.log(
            "[brackets-simple-node] Memory: %d of %d bytes free (%d%)",
            memory.free,
            memory.total,
            Math.floor(memory.free * 100 / memory.total)
        );
    });
    return memoryPromise;
}
```

Each of the above functions returns a promise (because each call to NodeConnection is asynchronous and promise-returning). So, we can easily chain them together.

```javascript
// Call all the helper functions in order
chain(connect, loadSimpleDomain, logMemory);
```

That's it! There is no step 3! (since we started at step 0...)

Note that there are a few more non-node-related things you need to do to make your extension work. Check out the full source at https://github.com/joelrbrandt/brackets-simple-node

How to Debug
------------

Debugging domains is fairly straightforward. Debugging the node core launching process is much more complicated.

### Standard debugging

To debug, you will need to start by installing node and node-inspector. Node can be installed from http://nodejs.org and node inspector can then be installed with ```npm install -g node-inspector```.

Once these are installed, do the following:

1. Launch Brackets
2. Enable node debugging from the "Debug" menu
3. Launch node-inspector
4. Node inspector will print out a URL to go to for debugging. On some machines, you may need to replace "0.0.0.0" with "localhost" in the url.
5. Set your breakpoints and call your code.

There are some important caveats in debugging. **Read the "Common 'Gotchas'" below!**

### Debugging the startup process

If you need to debug the launch process, start by opening "Launcher.js" in node-core. As a first pass, try setting the LOG_FILENAME_ON_LAUNCH variable to an absolute path that you have write-access to. See if you get the information you need in this log file.

If that doesn't work, set DEBUG_ON_LAUNCH to true. Then, run Brackets (which launches node-core). The debugger will automatically be enabled. Connect to it with node-inspector, set your breakpoints, and then call the globally-defined function ```debugLaunch``` from the console. This will start the actual launch routine asynchronously, and you'll hit your breakpoints.

**Important:** It is _very_ likely that you will get abandoned Brackets-node processes when doing this. **Read the "Common 'Gotchas'" below!** Failure to pay attention to this will cause massive confusion.


Common "Gotchas"
----------------

 *  **Abandoned Node process during debugging** -- In order to figure out when to end its process, Node pays attention to what's happening on it's stdin and stdout pipes. If you hit a breakpoint while debugging and then never resume, Node won't pay attention to its pipes. If you then quit Brackets, the Node process will get abandoned.

    If this happens, the only way to end the process is to use ```pkill Brackets-node``` / Activity Manager / Task Manager to kill the process. Note that this should only happen to _developers_, not to end users.

    Making matters worse, while this abandoned process sticks around, it will hold on to the debugging port. This means that if you don't realize you have an abandoned process and try to debug _another_ Brackets-node process, you'll have a bad time.

 *  **You cannot debug code that is called synchronously from the console in node-inspector** -- That is, if you set a breakpoint in your global function "foo" and call "foo" from the console, the debugger will _not_ stop at the breakpoint.

    A simple workaround is to do ```process.nextTick(foo)```

 *  **Must restart Node process if you make code changes** -- Reloading the Brackets window does _not_ restart the Node process. So, if you make changes to both client and node code, you have to reload Brackets and restart Node. This can be done from the Debug menu.

 *  **All extensions share the same "domain" space in node, so use unique names** -- If one extension loads a domain called ```foo```, every other extension will have access to that domain. This can be advantageous, but it means that unique names are necessary to avoid collisions.

Future Work
-----------

 *  **Implement "safe mode" in node core** -- It is easy to write a domain that continually crashes Node. Right now, if we get two crashes within 5 seconds, we do _not_ restart the node process. The risk here is that the user will install an extension that repeatedly crashes node, parts of Brackets will become unusable. For the things we have implemented now, this isn't a huge problem. But, if we start using node for filesystem access, that means we could get into a state where the user's node process crashes and he or she can't save their open documents.

     A fix for this would be to implement a "safe mode" in node core that doesn't allow registering of new domains. We would only load known-good domains (e.g. a filesystem domain). On any un-intended crash/restart, we would enter this safe mode.

 * **Remove auto-reconnect / auto-reload-domains from NodeConnection** -- This code seems to work great, and has unit tests that seem to consistently pass. But it's really complicated, which means it's more likely to have bugs. If we go to a safe mode, we might be able to remove this logic. (But maybe not: We want intended reloads that happen during the development process to work properly.)

 * **Improve the event infrastructure** -- Right now, we send all events to every connection. We could instead have connections register for events they care about so that we don't transfer unwanted data over the websocket

 * **Figure out a way to transfer binary data very quickly** -- Right now, the safest way to transfer binary data over the websocket is to base64 encode it. Ugh. We don't actually use binary data in Brackets right now, so this isn't a problem. But it could be soon...