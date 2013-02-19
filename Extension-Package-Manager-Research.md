This is a look at existing extension and package managers to get an idea of the state of the art and whether there is any reusable infrastructure.

## Brackets Extension Manager Extension ##

[jdiehl and DennisKehrig made a Brackets extension](https://github.com/jdiehl/brackets-extension-manager) for extension management.

## Package Control ##

The [third-party package manager for Sublime Text](http://wbond.net/sublime_packages/package_control).

Package Control has a minimal user interface:

![Package Control UI](http://wbond.net/sublime_packages/img/package_control/install_package.png)

It leverages Sublime's "Goto Anything" UI to search packages. There are no ratings, reviews or information beyond just the couple of lines that appear there.

No restart is required for any package operations. Packages can include a text file that is displayed in a Sublime buffer when the package is installed or updated (specified in a [messages.json file](https://github.com/wbond/sublime_package_control/blob/master/example-messages.json) in the package repository).

Packages are hosted on GitHub or Bitbucket. In the documentation for [package developers](http://wbond.net/sublime_packages/package_control/package_developers) it is noted that the process for getting a new package in is basically to add your package location to the main repositories json file and issue a pull request.

Package Control automatically creates a version number for packages by looking at the repository update time and it also pulls from the description of the repository on GitHub or Bitbucket. Developers *can* create a custom package.json file with their own version number if they wish (or if their package does not work on all three platforms supported by Sublime).

Packages are automatically upgraded on startup.

## Eclipse ##

Eclipse is built on a foundation of plugins (packaged together as "solutions" that you can install), so extension management is a core feature. It has been a few years since the last time I used Eclipse regularly, so I decided to install the latest and see how it works today.

On the Mac, at least, plugin management seemed a bit more difficult than I expected for a system that is built around plugins. First of all, I looked all over for how to install a new plugin before discovering that it was in the Help Menu:

![Install New Software from Help menu?](https://www.evernote.com/shard/s24/sh/a2912957-6dfc-4e4a-a4c5-c2facc911ba0/757fa0ec5dec1371347d12e3c8e0e89a/res/ec3611e2-7dd1-451e-a7cb-bda902949b9b/skitch.png?resizeSmall&width=832)

Next, you have to select a software site and then you get a filterable, categorized list of available plugins:

![Eclipse Add New Software](https://www.evernote.com/shard/s24/sh/890f1012-c45d-41f4-94a4-bd23eab17ccc/d13a7390830a61d49a80d91bafeb5926/res/94d69521-e6ca-4f73-8da8-227db09aa896/skitch.png?resizeSmall&width=832)

Installing software pops up a modal installation dialog that can be changed to "run in the background". Installation requires a click through license step. I had to trust the 3rd party certificate for the plugin I was installing (PyDev from Aptana). It also required restarting Eclipse.

Once the plugin is installed, you can see it in the list of installed plugins (which it appears you can only access from the Add New Software dialog):

![Eclipse Plugin Manager](https://www.evernote.com/shard/s24/sh/e4cf32ed-ee3f-4fb6-8f3a-8886028d1614/f640f01d9ef0520c769dc8af36078b4f/res/65ed2b13-3a73-43eb-b750-5728b57a39c1/skitch.png?resizeSmall&width=832)

I could uninstall the PyDev plugin easily from there, requiring a restart again (not surprisingly).

You can get more Eclipse plugins from the Marketplace site:

![Eclipse Marketplace site](https://www.evernote.com/shard/s24/sh/1d6f4442-7f58-455b-91c7-eb8d90fbfc3b/4d0685bc5ed843a749c0e15ca996ad38)

The Marketplace site does not enable one click install of plugins. Instead, it provides you with the "Update Site" URL which you can add to your Eclipse installation via the preferences dialog. The site provides "reviews", but it is actually a threaded comments system. Not surprisingly, the first one listed for the "solution" page I looked at (EGit) was not really a review at all but rather someone having trouble with the plugin. Eventually, that thread led to a call to post bug reports in the proper place.

Seeing this reinforces my belief that it needs to be *very* clear what users should do in the event of things just not working and that reviews should be guided to cover just the functionality that is provided by the extension.

## npm and its web interfaces ##

[npm](https://npmjs.org/) is the package manager for Node. It's written in JavaScript for Node, so it may be possible to reuse npm bits, especially after [node integration](https://trello.com/card/5-live-development-on-localhost/4f90a6d98f77505d7940ce88/684) lands in Brackets.

## component ##

A [package system for browser-based components](https://github.com/component).

## bower ##

Another [package system for browser-based components](http://twitter.github.com/bower), created by Twitter. Somewhat similar to component, but seems simpler. Uses a `component.json` metadata file that's similar to npm's `package.json`.

## Firefox Add-ons ##

Firefox's [add-on manager](https://support.mozilla.org/en-US/kb/find-and-install-add-ons-add-features-to-firefox) and [add-ons website](https://addons.mozilla.org/en-US/firefox/) have a long history and a lot of development behind them.

* Star ratings, reviews
* Collections

Different kinds of add-ons:

* Traditional add-ons: could access any public API in Firefox and use "XUL overlays" (see also "XBL") to augment the user interface in nearly unlimited ways
* Restartless add-ons: these can do much of what traditional add-ons did, and in the same style of code. However, there are limitations, as [noted by the creator of Adblock Plus](http://adblockplus.org/blog/why-you-should-make-your-next-add-on-restartless). Even so, in that same article, Wladimir says that it is worthwhile to go restartless
* Add-on SDK (Jetpack): Jetpacks are organized with an extensions-specific API to make extension writing easier. Jetpacks are built on CommonJS modules and are restartless. To date, they have been "statically linked": the SDK itself and any libraries your add-on depends on, are bundled in your package. Having the SDK statically linked has proven troublesome, so they are migrating the SDK into Firefox.

All of these add-ons are basically zipped directories.

## Ubuntu ##

The [Ubuntu Software Centre](http://www.ubuntu.com/ubuntu/features/find-more-apps) is an open source "app store". It is notable as a graphical front end to [apt](http://en.wikipedia.org/wiki/Advanced_Packaging_Tool) which is a widely used Linux package manager that needs to be able to wrangle dependencies between lots of packages. The package management needed by Linux applications is likely an extreme case of what we would see in a Brackets extension ecosystem (though the Node community has proven with npm that a good package manager can result in the creation of many inter-related packages).

# Conclusions #

## Desirable Features ##
[nj] From a UX perspective, I think it would be good to aim for something between the bare-bones feel of Sublime (or npm) and the slick, app-store-like experience of Firefox or Ubuntu. We should expose more functionality that aids discovery than Sublime/npm, but avoid feeling too "marketingy". For example, perhaps we shouldn't have icons for extensions; we wouldn't want developers who create useful extensions to feel penalized in their appearance in the extension manager because they don't happen to have access to someone who can create nice artwork for them. On the other hand, we should have a good search mechanism, perhaps a categorization or keyword tagging scheme, and ultimately ratings and reviews, because those all aid discovery in practical ways.

## Traits to Avoid ##

## Reusable Infrastructure? ##