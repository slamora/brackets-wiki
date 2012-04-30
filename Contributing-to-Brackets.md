How to Get Started
---------------------
The first step is to find something to fix. [How to Hack on Brackets] (wiki/How-to-Hack-on-Brackets) has some good pointers for finding issues and getting your head around how to get up to speed with Brackets. 

The nextstep is to fork the projects so you can start making changes in your local repository. Fork both brackets-app and brackets. 

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

Tracking Changes
-----------------------
Brackets development moves pretty quickly so it is really handy to be able to always track the changes from the main branch and pull them down to your local fork. To link your local fork of brackets-app with the main project, in your brackets-app directory type:

```
git remote add --track master upstream git@github.com:adobe/brackets-app.git
```

Then do the same for the Brackets project
```
cd brackets
git remote add --track master upstream git@github.com:adobe/brackets.git
```

Any time you want to get the latest changes from the main repo you can use

```
git fetch upstream master
```

That won't merge changes, but it will grab them. You can merge the changes into your local fork with

```
git merge upstream/master
```

You'll have to do this for both the brackets-app project and the brackets project. I also find it helpful to run 

```
git submodule update --init --recursive
```

again in the brackets-app directory to get any changes from CodeMirror.

Making Changes
---------------------
So you have found an issue that you want to fix. Start off by creating a new branch in your local directory. This assumes you are working in the brackets directory, but the same thing would apply for the brackets-app project as well. 

***
### Coding Conventions ###
Brackets uses a few coding conventions that are important to keep in mind before you commit.

* Must pass [JSLint] (http://www.jslint.com/)
* Use double quotes (") in JavaScript instead of single quotes

***

Start by creating a new branch so that you can always be sure master doesn't get messed up.

```
git branch mynewfeature
git checkout mynewfeature
```

> Note, that if you are fixing a particular issue, it can be useful to include the issue number in the branch name, for example _fix_issue_68_.

That creates a new branch called mynewfeature and sets it as your working branch. Any changes you make now will be linked to that branch. Go ahead and modify some code, make your fix, and be sure that it works in your copy of Brackets. Once you're happy with the fix, it's time to commit those changes and get ready to send it back to the team. If you're adding new files you'll have to run <code>git add</code> but if you're just modifiying existing files you can run

```
git commit -m "COMMIT MESSAGE"
```

Replace COMMIT MESSAGE with a detail message describing the changes. Once that's done the next step is to push those changes to your Github account using git push.

```
git push origin mynewfeature
```

That sends the branch (and the commits) to your Github-hosted fork of the project. Until now everything we did was local. Now Github knows about our branch as well as our changes.

Submitting a Pull Request
----------------------------
Now you're ready to submit a pull request. Go to the Github page for your fork of Brackets. If everything is working correctly, Github will actually detect that you pushed a branch and will show you a Pull request button. Click that button and you'll be brought to the pull request page where you can see the commits, look at the file differences, and add some comments. Use the comment box to explain in more detail your changes, why you made them, and any specific issues you're fixing. Then send it to the team.

Now you've got your first pull request in! Check back for comments the team might have for the pull request. If it's set, they'll merge it in and you're officially a Brackets contributor. Sometimes they'll ask you to make changes.

Making Changes to an Existing Pull Request
-----------------------------------------------