What should I hack on?
----------------------

Whatever you want, of course! But you might want to check the 
[public Brackets backlog](http://bit.ly/BracketsBacklog) 
to get ideas for things that are important to add soon. Also, if you're 
planning to do something substantial, please start a discussion on the 
[brackets-dev Google group](http://groups.google.com/group/brackets-dev)
or the [#brackets IRC channel on freenode](http://freenode.net) to get
feedback.

What's the process?
-------------------

Just submit changes as pull requests from your own fork of brackets or
brackets-app. The core dev team works in 2.5-week sprints (weird length,
but it works for us). We'll try to review small pull requests quickly
in the current sprint. Larger submissions will be added to the 
[public Brackets backlog](https://trello.com/board/brackets/4f90a6d98f77505d7940ce88)
and scheduled to be reviewed and merged in an upcoming sprint. 

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
repo as a submodule. There's also a [Brackets-specific fork of CodeMirror](http://github.com/adobe/CodeMirror2)
(see [Notes on CodeMirror](https://github.com/adobe/brackets/wiki/Notes-on-CodeMirror)).

In addition to pulling brackets-app from github, you'll need to also grab submodule
references. To do so, run the following command in the root of your brackets-app repo:

    git submodule update --init --recursive
    
See [Pro Git section 6.6](http://progit.org/book/ch6-6.html) for some caveats 
when working with submodules.

**Note:** We don't update the brackets submodule SHA in brackets-app very often. So,
to make sure you're getting the latest brackets core code, once your submodules are
set up, make sure to do a `git pull` on master in the brackets subfolder. In general,
you should keep up to date with the brackets core by pulling it directly instead of
relying on `git submodule update` in brackets-app.

To test if everything is working, run bin/mac/Brackets.app or bin/win/Brackets.exe. 
You should see the brackets interface. 

By default, Brackets opens its own source folder (which is in the "brackets" 
subfolder/submodule repo). So hack away!

Useful tools
------------

If you use Brackets to edit Brackets, you can quickly reload the app itself by 
choosing *Debug > Refresh Window* from in-app menu. (If your Brackets gets really
hosed, you can try *View > Refresh* from the native menu.) You can also bring up 
the Chrome developer tools on the Brackets window using *Debug > Show Developer Tools*.

You can open a second Brackets window from *Debug > New Window*. This is nice 
because it means you can use a stable Brackets in one window to edit your code, 
and then reload the app in the second window to see if your changes worked. You 
can bring up the developer tools on the second window, too. Note that 
this can be flaky; if you find that the dev tools aren't showing scripts, just 
close the second window and reopen it, then open the dev tools window again.

You can use *Debug > Run Tests* to run our unit test suite, and *Debug >
Show Perf Data* to show some rudimentary performance info. We plan to beef
this up in upcoming sprints.

Modifying the App Shell
-----------------------
For most of your work on Brackets, you should only need to edit the HTML/JS/CSS
code in the brackets repo. But if you need to do work on the native app shell
in brackets-app, you can find instructions on the [brackets-app wiki]
(https://github.com/adobe/brackets-app/wiki).