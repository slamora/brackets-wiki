## tl;dr ##
1. Before you get started, post to the [brackets-dev Google group](http://groups.google.com/group/brackets-dev) or the [#brackets IRC channel on freenode](http://freenode.net) about what you're thinking of working on, so you can get early feedback.
2. When coding, make sure to follow our [coding conventions](https://github.com/adobe/brackets/wiki/Brackets-Coding-Conventions).
3. Before submitting any pull request, make sure to:
    1. merge from adobe/brackets master
    2. re-test your code after the merge
    3. run the unit tests with Debug > Run Tests -- everything should pass
    4. if your change is nontrivial or might have affected the UI, run through the [Brackets smoke tests](https://github.com/adobe/brackets/wiki/Brackets-Smoke-Tests).
4. Before submitting your first pull request, make sure to [sign the Brackets Contributor License Agreement (CLA)](http://brackets.io/brackets-contributor-license-agreement.html), or we can't accept your pull request. You only need to do this once in your lifetime.

## How to Get Started ##

### What should I hack on? ###
Whatever you want, of course! But you might want to check the 
[public Brackets backlog](http://bit.ly/BracketsBacklog) 
to get ideas for things that are important to add soon. Also, if you're 
planning to do something other than a small bugfix, please start a discussion 
on the [brackets-dev Google group](http://groups.google.com/group/brackets-dev)
or the [#brackets IRC channel on freenode](http://freenode.net) to get
feedback. There might already be some prior thinking on what you're working on,
or some reason that it hasn't already been done.

### Getting a Copy of the Code ###
The first step is to fork the projects so you can start making changes in your local repository. Fork both brackets-app and brackets. 

Next pull both of those repositories down to your local machine. Your folder structure should have the brackets project within brackets-app.

```
git clone git@github.com:<your username>/brackets-app.git
cd brackets-app
git clone git@github.com:<your username>/brackets.git
```

Because Brackets relies on [CodeMirror](http://codemirror.net/) you want to make sure you have the latest version of CodeMirror. Brackets uses submodules to help track both CodeMirror and the Brackets project from Brackets app. To make sure everything is up to date go back to your brackets-app folder and run.

```
git submodule update --init --recursive
```

That should both get the latest version of CodeMirror and sync your fork of Brackets with brackets-app. If you run _bin/mac/Brackets.app_ or _bin/win/Brackets.exe_ you'll have your just-forked version of Brackets up and running.

### Getting a Copy of the Code ###
It's important to be working off of the latest build and the easiest way to do that is to make sure that your local copy of Brackets is tracking the main repository. This involves using the `git remote` command which lets you link your local version to a different remote repository (by default, it's linked to your github fork). To link your local repository to the main Brackets repository, use this command:

```
git remote add --track master upstream git@github.com:adobe/brackets-app.git
```

This will create a link from your local Brackets repository to the main one called `upstream`. `upstream` will be how you reference the main Brackets repository. 

Do the same for the Brackets project
```
cd brackets
git remote add --track master upstream git@github.com:adobe/brackets.git
```

It's very important to always have the latest code from the main repository. That way you can make sure your pull requests will be clean merges and that you're always working with the most stable code. Any time you want to grab the master branch from the Brackets repository simply use the fetch command with the remote destination we created earlier:

```
git fetch upstream master
```

The fetch command won't merge the changes, but it does bring them down so that you can merge them manually. 
That won't merge changes, but it will grab them. You can merge the changes into your local fork with

```
git merge upstream/master
```

Those two commands will grab and merge any changes from the main Brackets repository for your current branch. 

If there have been CodeMirror changes you will also want to bring those in. You can do that by re-running the git submodule command.

```
git submodule update --init --recursive
```

## Contributing Code ##

### Modifying the Code ###
So you have found an issue that you want to fix. Start off by creating a new branch in your local directory. This assumes you are working in the brackets directory, but the same thing would apply for the brackets-app project as well. 

> Make sure to follow the [coding conventions](https://github.com/adobe/brackets/wiki/Brackets-Coding-Conventions) for Brackets so that your code matches the rest of the project.

Start by creating a new branch for the feature you want to work on. This makes sure that your master branch can stay in sync with the main Brackets repository and if the feature doesn't work or breaks something, you can always start fresh with your local master branch. It also makes updating your pull request much easier as you can see below.

```
git branch mynewfeature
git checkout mynewfeature
```

> Note, that if you are fixing a particular issue, it can be useful to include the issue number in the branch name, for example _fix_issue_68_.

That creates a new branch called `mynewfeature` and sets it as your working branch. Any changes you make now will be linked to that branch. If you're working on a big feature that may take some time, remember that you can always use the git upstream commands above to merge the latest version of the main Brackets repository into your current working branch. Dealing with merges incrementally like this can be better than trying to reconcile everything once you're finished with your feature. 

Go ahead and modify some code, make your fix, and be sure that it works in your copy of Brackets. Once you're happy with the fix, it's time to commit those changes and get ready to send it back to the team. You can use `git add <filename>` to add any files you've changed to the commit you want to make and then use `git commit -m "COMMIT MESSAGE"` to commit those changes. You can also use `git commit -a` to commit all of the current changes. Be sure to replace COMMIT MESSAGE with a detailed message that describes the changes you're making for that specific commit. 

Once that's done the next step is to push those changes to your Github account using git push. This is where it's important to understand branches, origins, and remotes. You've made all of these changes (and committed them) to your local copy of the Brackets repository. And hopefully you've been pulling down new versions from the linked, remote branch of Brackets, and merging them as you go. What you need to do now is tell Github about your branch and the commits it contains. Github repositories use `origin` to reference your Github-hosted fork. So to push your changes use.

```
git push origin mynewfeature
```

That command creates a branch on your Github-hosted fork of Brackets and commits all of the changes. Until now everything we did was local. Now Github knows about our branch as well as our changes.

### Submitting a Pull Request ###
Now you're ready to submit a pull request. Go to the Github page for your fork of Brackets. In order to submit a pull request, you need to be looking at the branch you created, which we called `newfeature`. Github has a pulldown that lets you select branches in your fork of the repository. Click that, find the branch you were working on, and select it. Now you're looking at the code for that branch. 

Click the pull request button in the top left and you'll be brought to a page that describes the pull request. You want to make sure that you're submitting your pull request to the `adobe/brackets` repository and that the pull request is coming from the branch you've been working on. 

You'll be able to take a look at all of the commits associated with this pull request and all the files that you've modified. Make sure this is all correct and then in the description provide a detailed description of what your pull request does.

Now you've got your first pull request in! Check back for comments the team might have for the pull request. If it's set, they'll merge it in and you're officially a Brackets contributor. Sometimes they'll ask you to make changes.

## Making Changes to an Existing Pull Request ##
In a lot of cases you may have to make changes to your pull request. If you're working off a branch, Github makes this very easy. After you've gotten comments on the pull request and know what changes you need to make, go into your code and make sure you're working on your branch.

```
git checkout mynewfeature
```

Then go through the steps above to modify your code and make any changes based on comments from the pull request. Commit all of those changes with `git commit` and then push the changes to your Github-hosted fork with `git push origin mynewfeature`.

As soon as you push those changes to the origin, the pull request you submitted will automatically be updated with the new code. If you go look at the pull request, you'll see a new comment entry with the commits that you added to the branch. Sometimes it helps to add another comment right after that commit describing some of the changes you made in more detail. 
