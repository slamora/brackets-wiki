What should I hack on?
----------------------

Whatever you want, of course! But you might want to check with us first. We
have a public backlog **link TBD** that might give you ideas for things we
think are important to do soon. Also, if you're planning to do something
substantial, please start a discussion on the [brackets-dev Google group](http://groups.google.com/group/brackets-dev)
or the [#brackets IRC channel on freenode](http://freenode.net) to get
feedback.

Submitting changes
------------------

You can submit changes as pull requests from your own fork of Brackets. 
For small pull requests, we'll try to review them quickly. For larger
submissions, we'll add them to our public backlog and schedule them to be
reviewed and merged in an upcoming sprint. Our sprints are currently
2.5 weeks long, so the delay shouldn't be too bad.

Folder organization
-------------------

Brackets is primarily built in HTML/JS/CSS, but it currently runs as a desktop
app inside a thin native app shell, currently based on [CEF](http://code.google.com/p/chromiumembedded/),
that lets it access local files. (It doesn't run in the browser yet, and in
fact will look amazingly terrible if you try to open it in Chrome from your local
filesystem. We're planning to work on the in-browser version soon.)

If you pull the repos as described below, or look in the contents of the ZIP
file, you'll see that the main app binaries are in the bin/ folder, but the
actual HTML/JS/CSS files that implement the main app are in the brackets/
subfolder. The src/ folder is just the source for the native app shell.

Getting the source
------------------

Brackets is currently hosted in two github repos: the 
[brackets-app repo](http://github.com/adobe/brackets-app), which contains
the thin native app shell, and the [brackets repo](http://github.com/adobe/brackets), 
which contains the main HTML/JS/CSS code. The brackets-app repo contains the brackets
repo as a submodule. We also have our 
[own fork of CodeMirror](http://github.com/adobe/CodeMirror2); we're working with
Marijn Haverbeke, the owner of CodeMirror, to get our changes merged into the main
repo. **TODO: link to info on this**

In addition to pulling brackets-app from github, you'll need to also grab submodule
references. To do so, run the following command in the root of your brackets-app repo:

    git submodule update --init --recursive
    
See [Pro Git section 6.6](http://progit.org/book/ch6-6.html) for some caveats 
when working with submodules.

To test if everything is working, run bin/mac/Brackets.app or bin/win/Brackets.exe. 
You should see the brackets interface. 

Here are instructions for modifying the app shell on Mac (Win instructions coming
soon):

To modify the application shell code, load src/mac/Brackets.xcodeproj into 
XCode 4.1 (or newer). 

There are three main build targets: 

1. Brackets CEF Debug - this is a full debug build and is **really** slow. 
   This target should only be used if you need breakpoint debugging.
2. Brackets Development - this is a "release development" build. It's much 
   faster, but you can't set breakpoints.
3. Brackets Archive - this target does a full release build and copies the 
   build to the bin/mac directory. This target MUST be built before checking in any changes to the shell application.

**NOTE:** Before you build anything, you need to make a couple changes to the 
build schemes. This only needs to be done once since these settings will persist 
when you quit Xcode.

1. Click the combobox at the top that says "Brackets Archive".
2. Select "Edit Scheme..." from the dropdown.
3. In the window that opens, select "Run Brackets.app" on the left hand list
4. In the right pane, select "Release" for Build Configuration
5. Click OK to close the window
6. Select "Brackets Development" from the dropdown
7. Repeat steps 1-5 for the Brackets Development scheme

**IMPORTANT:** If you make changes to the application shell, you **MUST** build 
the Brackets Archive target. This will ensure a updated Release build is checked 
in to /bin/mac.

Useful tools
------------

Because Brackets is built in HTML/JS/CSS, we've actually started using Brackets
itself to edit its own source code. It's pretty fun!

If you use Brackets to edit Brackets, you can quickly reload the app itself by 
choosing *View > Refresh* from the native menu (not the in-Brackets menu).
You can also bring up the Chrome developer tools on the Brackets window using
*View > Show Developer Tools*.

You can open a second Brackets window from the *Debug > New Window* item in
the in-Brackets menu (not the native menu). This is nice because it means you
can use a stable Brackets in one window to edit your code, and then reload the
app in the second window to see if your changes worked. You can bring up the
developer tools on the second window, too. (However, note that this can be
flaky; if you find that the dev tools aren't showing scripts, just close the
second window and reopen it, then open the dev tools window again.)

You can use *Debug > Run Tests* to run our unit test suite, and *Debug >
Show Perf Data* to show some rudimentary performance info. We plan to beef
this up in upcoming sprints.

