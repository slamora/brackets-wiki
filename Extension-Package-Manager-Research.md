This is a look at existing extension and package managers to get an idea of the state of the art and whether there is any reusable infrastructure.

## Package Control ##

The [third-party package manager for Sublime Text](http://wbond.net/sublime_packages/package_control).

## Eclipse ##

Eclipse is built on a foundation of plugins, so extension management is a core feature.

## npm and its web interfaces ##

[npm](https://npmjs.org/) is the package manager for Node. It's written in JavaScript for Node, so it may be possible to reuse npm bits, especially after [node integration](https://trello.com/card/5-live-development-on-localhost/4f90a6d98f77505d7940ce88/684) lands in Brackets.

## component ##

A [package system for browser-based components](https://github.com/component).

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

## Traits to Avoid ##

## Reusable Infrastructure? ##