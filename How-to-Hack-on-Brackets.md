1. This page is mainly about modifying core Brackets code. If you're adding a new feature, consider [writing an extension](How to write extensions) instead.


1. Before submitting your first pull request, make sure to [sign the Brackets Contributor License Agreement (CLA)](http://dev.brackets.io/brackets-contributor-license-agreement.html), or we can't accept your pull request. You only need to do this once in your lifetime.

[CONTRIBUTING.md](https://github.com/adobe/brackets/blob/master/CONTRIBUTING.md) contains a high-level overview of what you may need to consider if you plan to contribute to Brackets.    

## How to Get Started ##

### How Brackets is Organized ###
Brackets is primarily built in HTML/JS/CSS, but it currently runs as a desktop
app inside a thin native app shell, currently based on [CEF](http://code.google.com/p/chromiumembedded/),
that lets it access local files. (It doesn't run in the browser yet, and in
fact will look amazingly terrible if you try to open it in Chrome from your local
filesystem. We're planning to work on the in-browser version soon.)

The Brackets installer packages the HTML/JS/CSS files into Brackets.app (on Mac) or in a `www` directory next to Brackets.exe (on Windows). If you're only changing HTML/JS/CSS files, you can use the `tools/setup_for_hacking` script to tell the Brackets application shell to use your local copy of the Brackets HTML/JS/CSS files. See [Running Your Copy of the Code](#setup_for_hacking) for more details.

For details on working with Brackets's architecture and APIs, see [[Brackets Development How-Tos]].

### What should I hack on? ###
Whatever you want, of course! But you might want to check the [public Brackets backlog](http://bit.ly/BracketsBacklog) to get ideas for things that are important to add soon. If you're new to Brackets, we've also tagged some good "starter features" ([backlog](http://bit.ly/BracketsBacklog) > Search and Filter Cards > Starter Feature) and "[starter bugs](https://github.com/adobe/brackets/issues?labels=starter+bug&page=1&state=open)" that should be easy to work on while you're still learning the code.

If you're planning to do something other than a small bugfix, please start a discussion on the [brackets-dev Google group](http://groups.google.com/group/brackets-dev) or the [#brackets IRC channel on freenode](http://freenode.net) to get feedback. There might already be some prior thinking on what you're working on, or some reason that it hasn't already been done.

### What's the process? ###
First, [sign the Brackets Contributor License Agreement (CLA)](http://dev.brackets.io/brackets-contributor-license-agreement.html). 
This is for your protection as well as that of the Brackets project.

Then, just submit changes as pull requests from your own fork of brackets or
brackets-shell. The core dev team works in 2.5-week sprints (weird length,
but it works for us). We'll try to review small pull requests quickly
in the current sprint. Larger submissions will be added to the 
[public Brackets backlog](https://trello.com/board/brackets/4f90a6d98f77505d7940ce88)
and scheduled to be reviewed and merged in an upcoming sprint.

Before you submit your pull request, please make sure it's merged with master and fully tested as described in the tl;dr section above. 

### <a name="getcode"></a> Getting a Copy of the Code ###
The first step is to fork the projects on GitHub so you can start making changes in your local repository. If you only plan to hack on the HTML/JS/CSS portions of the app, you only need to fork the [brackets repo](https://github.com/adobe/brackets). If you want to work on the native code as well, you can fork the [brackets-shell repo](https://github.com/adobe/brackets-shell) too. To fork one or both of the repos, simply click the "Fork" button at the top of the page while browsing the repo.

Next pull the repositories down to your local machine. 

The [brackets](https://github.com/adobe/brackets) repo has all of the HTML/JS/CSS files.
```bash
git clone https://github.com/<your username>/brackets.git
```

_Important:_ Brackets uses submodules to track third-party repos (like [CodeMirror](http://codemirror.net/)) that it depends on. To get these set up, you need to run this command from within the `brackets` folder:

```bash
git submodule update --init
```

> Because CodeMirror will update fairly often, you'll often find as you're switching between branches or merging that your CodeMirror files are showing up as modified when you run `git status`. Something like `M	src/thirdparty/CodeMirror2`. Run the command above to bring submodules back in sync.

The [brackets-shell](https://github.com/adobe/brackets-shell) repo has the code for the native shell. You _only_ need to clone this repo if you plan on making changes to the native shell. 

```bash
git clone https://github.com/<your username>/brackets-shell.git
```

### Building Native Shell ###
If you are running the Brackets native shell app from an installer, you can skip this section.

For most of your work on Brackets, you should only need to edit the HTML/JS/CSS code in the
brackets repo. To work in the latest brackets code, you may need to run the latest native app
shell in brackets-shell, which means you will need to build it. You can find instructions
on the [brackets-shell wiki](https://github.com/adobe/brackets-shell/wiki).

### <a name="setup_for_hacking"></a> Running Your Copy of the Code ###

If you're only hacking on HTML/JS/CSS files, you can have the installed version of the Brackets shell run your local copy of the HTML/JS/CSS code by running the `tools/setup_for_hacking` script. Here are the steps.

1. If you haven't already, download and install the latest Brackets sprint build from the [Downloads page](http://download.brackets.io). **Note:** Make sure you download a Mac .dmg or Windows .msi — the "Download as zip/tar.gz" files at the top **will not work**.
2. Clone or fork the brackets repo (see [Getting a Copy of the Code](#getcode) for details).
3. On a Mac: 
  1. Open a Terminal window
  2. `cd` to the root of your brackets repo
  3. run `tools/setup_for_hacking.sh`, passing the full pathname to your installed Brackets.app. For example:
```bash
tools/setup_for_hacking.sh "/Applications/Brackets Sprint 14.app"
```
4. On Windows:
  1. Open a Command Prompt (you will likely need to "Run as Administrator")
  2. `cd` to the root of your brackets repo
  3. run `tools\setup_for_hacking.bat`, passing the full path of the directory where Brackets.exe is installed. For example:
```bat
tools\setup_for_hacking.bat "C:\Program Files (x86)\Brackets Sprint 14"
```
5. Launch the installed copy of Brackets, select _Help > About_, and make sure that the version number says "sprint xx development build". This indicates that you're running Brackets from your git repo instead of the installed build. (If you see "sprint xx experimental build", you're not properly set up.)
6. To revert back to using the installed version of the Brackets source, run `tools/restore_installed_build.sh` (Mac) or `tools\restore_installed_build.bat` (Windows) from your Brackets repo.

(Alternatively, you can hold down the `shift` key while launching Brackets to get a file selector dialog. Select the `src/index.html` file from your copy of the code, and Brackets will run your copy of the HTML/JS/CSS files. _Note: this `index.html` file will only be used for the current Brackets session. Quitting and restarting will revert back to the previous `index.html` file._)

If you cloned the brackets-shell, see the brackets-shell [readme](https://github.com/adobe/brackets-shell/blob/master/README.md) and [wiki pages](https://github.com/adobe/brackets-shell/wiki) for information on building the project.

### Tracking Changes from the Main Repository ###
It's important to be working off of the latest build and the easiest way to do that is to make sure that your local copy of Brackets is tracking the main repository. This involves using the `git remote` command which lets you link your local version to a different remote repository (by default, it's linked to your github fork). To link your local repository to the main brackets repository, use this command from the _brackets_ directory:

```bash
cd brackets
git remote add upstream https://github.com/adobe/brackets.git
```

This will create a link from your local brackets repository to the main one called `upstream`. `upstream` will be how you reference the main brackets repository.

If you want to avoid getting branches other than master, you can add the `--track master` argument after `add`. However, that will mean that if you need to pull a different branch, you'll need to explicitly fetch it. 

Do the same for the brackets-shell repository (if you forked it).
```bash
cd brackets-shell
git remote add upstream https://github.com/adobe/brackets-shell.git
```

It's very important to always have the latest code from the main repository. That way you can make sure your pull requests will be clean merges and that you're always working with the most stable code. Any time you want to grab the master branch from the Brackets repository, you have to follow a two-step process. First, use the fetch command with the remote destination we created earlier:

```
git fetch upstream
```

The fetch command brings down any new commits into your repo, but doesn't actually update any of your branches. 
You can merge the changes into your local branch with

```
git merge upstream/master
```

Those two commands will merge any changes from the main Brackets repository into your currently checked-out  branch.

> If you want to update more than one branch--for example, your local "master" branch as well as another branch you're working on--you must checkout each branch you want to update and then do `git merge upstream/master` on each one.

If there have been CodeMirror changes you will also need to bring those in. You can do that by re-running the git submodule command from within the brackets folder.

```
git submodule update
```

If new submodules are added to Brackets, you'll need to run `git submodule update --init` to get them as well.

## Contributing Code ##

### Useful Tools for Development ###
If you use Brackets to edit Brackets, you can quickly reload the app itself by choosing *Debug > Reload Brackets* from the in-app menu. (If your Brackets gets really hosed, you may need to restart the application).  Note: because of bug [#1551](https://github.com/adobe/brackets/issues/1551), Reload may ignore changes to certain files unless you [disable caching via the Developer Tools](https://groups.google.com/forum/?fromgroups=#!topic/brackets-dev/E5iqcD8VqD4).

To bring up the Chrome Developer Tools on the Brackets window, use *Debug > Show Developer Tools*. This will open a new Chrome tab with the developer tools (Unlike previous builds of Brackets that opened up the developer tools in a new Brackets window, the new CEF3-based shell needs to open the developer tools in Chrome. We're hoping to get this fixed soon).

If you need to debug startup code, you can launch Brackets, open the developer tools, set your breakpoints, and then select *Debug > Reload Brackets*. This will re-run all of the startup code and stop at any breakpoints you have set.

You can open a second Brackets window from *Debug > New Window*. This is nice because it means you can use a stable Brackets in one window to edit your code, and then reload the app in the second window to see if your changes worked. You can bring up the developer tools on the second window, too. 

You can use *Debug > Run Tests* to run our unit test suite, and *Debug > Show Perf Data* to show some rudimentary performance info. There is also an article on [Debugging Test Windows in Unit Tests](https://github.com/adobe/brackets/wiki/Debugging-Test-Windows-in-Unit-Tests).

### Modifying the Code ###
So you have found an issue that you want to fix or a feature you want to implement. Start off by creating a new branch in your local directory. This assumes you are working in the brackets directory, but the same thing would apply for the brackets-app project as well. 

> Make sure to follow the [coding conventions](Brackets Coding Conventions) for Brackets so that your code matches the rest of the project.

Start by creating a new branch off of master for the feature you want to work on. This makes sure that your master branch can stay in sync with the main Brackets repository and if the feature doesn't work or breaks something, you can always start fresh with your local master branch. It also makes updating your pull request much easier as you can see below.

```
git checkout master
git branch mynewfeature
git checkout mynewfeature
```

> Note, that if you are fixing a particular issue, it can be useful to include the issue number in the branch name, for example _fix_issue_68_.

That creates a new branch called `mynewfeature` and sets it as your working branch. Any changes you make now will be linked to that branch. If you're working on a big feature that may take some time, remember that you can always use the git fetch and merge commands from upstream, as described above, to merge the latest master from the main Brackets repository into your current working branch. Dealing with merges incrementally like this can be better than trying to reconcile everything once you're finished with your feature. 

Go ahead and modify some code, make your fix, and be sure that it works in your copy of Brackets. Once you're happy with the fix, it's time to commit those changes and get ready to send it back to the team. You can use `git add <filename>` to add any files you've changed to the commit you want to make and then use `git commit -m "COMMIT MESSAGE"` to commit those changes. You can also use `git commit -a` to commit all of the current changes. Be sure to replace COMMIT MESSAGE with a detailed message that describes the changes you're making for that specific commit. 

Once that's done the next step is to push those changes to your GitHub account using git push. This is where it's important to understand branches, origins, and remotes. You've made all of these changes (and committed them) to your local copy of the Brackets repository. And hopefully you've been pulling down new versions from the linked, remote branch of Brackets, and merging them as you go. What you need to do now is tell Github about your branch and the commits it contains. Github repositories use `origin` to reference your Github-hosted fork. So to push your changes use.

```
git push origin mynewfeature
```

That command creates a branch on your Github-hosted fork of Brackets and commits all of the changes. Until now everything we did was local. Now Github knows about our branch as well as our changes.

> Before you submit a pull request you should make sure everything passes JSLint. This is easy because Brackets will show you anywhere in your file that JSLint sees an error. You also need to make sure that the unit tests pass without any errors. You can run the Brackets unit tests by going to *Debug > Run Unit Tests* in Brackets. The tests require Chrome and you should quit Chrome before running the tests for the most accurate results.
Please also review the [Pull Request Checklist](https://github.com/adobe/brackets/wiki/Pull-Request-Checklist) for additional guidance.
 

### Submitting a Pull Request ###
Now you're ready to submit a pull request. Go to the GitHub page for your fork of Brackets. In order to submit a pull request, you need to be looking at the branch you created, which we called `mynewfeature`. GitHub has a pulldown that lets you select branches in your fork of the repository. Click that, find the branch you were working on, and select it. Now you're looking at the code for that branch. 

Click the pull request button in the top right and you'll be brought to a page that describes the pull request. You want to make sure that you're submitting your pull request to the `adobe/brackets` repository and that the pull request is coming from the branch you've been working on. 

You'll be able to take a look at all of the commits associated with this pull request and all the files that you've modified. Make sure this is all correct and then in the description provide a detailed description of what your pull request does.

Now you've got your first pull request in! Check back for comments the team might have for the pull request. If it's set, they'll merge it in and you're officially a Brackets contributor. Sometimes they'll ask you to make changes.

### Fixing a Bug ###

If you are working on a specific bug, here are the steps to follow:

1. Create a new branch and use the issue number as the branch name. Be sure you have the latest code from the main repository before branching to make merging as easy as possible (instructions above).
1. Fix the bug.
1. Commit and push your changes according to the instructions above.
1. Submit a pull request. Make sure the text "issue #123" (use your specific issue number) is in the pull request in order to create a link to the issue.

That's it! You've just made Brackets even better.

## Making Changes to an Existing Pull Request ##
In a lot of cases you may have to make changes to your pull request. If you're working off a branch, Github makes this very easy. After you've gotten comments on the pull request and know what changes you need to make, go into your code and make sure you're working on your branch.

```
git checkout mynewfeature
```

Then go through the steps above to modify your code and make any changes based on comments from the pull request. Commit all of those changes with `git commit` and then push the changes to your Github-hosted fork with `git push origin mynewfeature`.

As soon as you push those changes to the origin, the pull request you submitted will automatically be updated with the new code. If you go look at the pull request, you'll see a new comment entry with the commits that you added to the branch. Sometimes it helps to add another comment right after that commit describing some of the changes you made in more detail. 

### Modifying the App Shell ###
For most of your work on Brackets, you should only need to edit the HTML/JS/CSS
code in the brackets repo. But if you need to do work on the native app shell
in brackets-shell, you can find instructions on the [brackets-shell wiki]
(https://github.com/adobe/brackets-shell/wiki).