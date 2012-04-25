Folder organization
-------------------

Brackets is currently built using a thin native app shell around an HTML/JS/CSS
app. If you pull the repos as described below, or look in the contents of the ZIP
file, you'll see that the main app binaries are in the bin/ folder, but the
actual HTML/JS/CSS files that implement the main app are in the brackets/
subfolder. The src/ folder is just the source for the native app shell, which
is currently based on [CEF](http://code.google.com/p/chromiumembedded/).

Getting the source
------------------

Brackets is currently hosted in two github repos: the 
[brackets-app repo](http://github.com/adobe/brackets-app), which contains
the native app shell, and the [brackets repo](http://github.com/adobe/brackets), 
which contains the HTML/JS/CSS code. The brackets-app repo contains the brackets
repo as a submodule. (These repos are currently private, so you'll need to
contact us for access.) We also have our 
[own fork of CodeMirror](http://github.com/adobe/CodeMirror2); we're working with
Marijn Haverbeke, the owner of CodeMirror, to get our changes merged into the main
repo.

In addition to pulling brackets-app from github, you'll need to also grab submodule
references by the Brackets application. To do so, first make sure you have SSH 
access to github (since the submodules are referenced via a git: URL rather than 
https). Then run the following command in the root of your brackets-app repo:

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
build schemes. This only needs to be done once since these setting will persist 
when you quit xcode.

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
developer tools on the second window, too.

You can use *Debug > Run Tests* to run our unit test suite, and *Debug >
Show Perf Data* to show some rudimentary performance info.