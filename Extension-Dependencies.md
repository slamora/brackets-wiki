# Extension Dependencies Research #

(This may eventually become documentation on how extension dependencies are handled.)

There are two different kinds of dependencies that extensions can have:

1. Brackets version
2. other extensions that provide necessary code/features for a given extension

For our initial take on extension management, it is imperative that we address the first concern, Brackets version compatibility. We *can* opt to handle dependency relationships between extensions later on.

What happens if we defer dependency relationships between extensions? The ability of extensions to relate to one another will affect the shape of the ecosystem. As an example of this, npm has taken package dependencies very seriously. As a result, npm has many small packages that "do one thing well" and rely on one another. If managing those dependencies was more difficult or fragile, the ecosystem of node modules would look quite different.

It is early enough for Brackets that we can defer this, but ultimately we'll want extensions to be able to provide capabilities that other extensions can take advantage of. In this document, we'll talk about extension dependencies and considerations of their management.

## Brackets Version Compatibility ##

Extensions work fine with the version of Brackets for which they were created. However, as Brackets continues to evolve, extensions will break if they are not maintained. Here are a few approaches for ensuring that a given extension is compatible with a user's version of Brackets:

1. extension developer keeps minimum and maximum version numbers up to date
2. [semantic versioning](http://semver.org/) is used to make extension API compatibility clear
3. "smart deprecation" and crowdsourcing marks keeps compatibility info up-to-date

I'm going to suggest that the third option is the best, but only after explaining the first two options for clarity as those first two options are the paths more often traveled.

### Min/Max Version Numbers in Metadata ###

Until Firefox 4, Firefox had "big bang" releases every 18 months or so. It was reasonable for Firefox add-on developers to update their metadata after one of these big bang releases after they did the work to ensure that their add-on was working fine with the new release. Firefox 4 spent 8 months in beta, giving developers ample opportunity to update their add-ons before the release, so that there wasn't a big problem of popular add-ons not being compatible with the new release.

Three months after Firefox 4's release, Mozilla changed to a 6 week "rapid release" cycle. API changes between these releases were possible and did happen. The requirement for add-on developers to check their add-ons for each release remained. Some add-on developers, however, did not want to do this every 6 weeks. An increasing number of add-ons started appearing to be incompatible, though many were still just fine because there weren't *that* many API changes in a given six week period.

Addons on addons.mozilla.org started having their metadata automatically updated to reflect compatibility with newer versions of Firefox. However, many add-ons were not installed from addons.mozilla.org. After a few releases of users complaining about "add-ons not being compatible", Firefox switched to having [add-ons be compatible by default](https://wiki.mozilla.org/Features/Add-ons/Add-ons_Default_to_Compatible).

The [Add-on Compatibility Reporter](https://addons.mozilla.org/en-US/firefox/addon/add-on-compatibility-reporter/) allows users to report add-ons that are having trouble with newer versions of Firefox.

With its short release cycles, having Brackets extensions specify their max versions as the version for which the extension was written will result in users being penalized for keeping their Brackets up to date – many of their extensions will likely stop working when new updates come out. This is especially true given that Brackets has no beta test period at this time.

> *resolved* (pf) It doesn't seem like the design FF _stabilized_ on is that bad, though: authors specify a min and the max is optional. Unless a max is specified the extension is assumed to work with all future versions. That sounds pretty similar to the below -- the main difference being that in FF only the extension author can add a max constraint, whereas below it might happen automatically due to various flagging.

> (kd) I changed the language in the paragraph above to note specifically that I meant specifying a max version as the version for which an extension was written (which, in theory, is the only one the author knows for sure is compatible). See below regarding allowing the author to set a max version.

### Semantic Versioning ###

[Semantic Versioning](http://semver.org/) is a convention for version numbers that provides real meaning behind them. semver is a standard among npm users.

To separate the marketing needs for version numbers from the extensions' needs for version numbers, we can talk about the "API version". If the Brackets API used semver, an extension made for Brackets API 1.x is *guaranteed* to work until Brackets API 2.0.0 is released. (It would be considered a bug if an extension *did* break during the 1.x cycle.)

This is an improvement over the min/max version scheme because it gives the Brackets team the ability to consciously choose when we make breaking changes and to ensure that there's enough warning for developers to deal with it.

The drawback to this scheme, though, is that it's very coarse-grained. If we felt the need to make even *one* backwards-incompatible change, semver dictates that we move to API 2.0. The number itself is not meaningful, but the fact that we've just broken all of the extensions *is*.

### Smart Deprecation + Crowdsourcing ###

In the Python standard library, a function deprecated in version 2.5 will start displaying warnings when used. When 2.6 comes out, that function will be gone. This policy gave Python a chance to move forward, while giving people a reasonable amount of time to heed the warnings and build to the new API.

With a central extensions repository, we can take this a step farther. Extension developers need not specify which version of Brackets their extensions are compatible with. The repository would keep track of the information for everyone. (If an extension developer *knows* that they're extension is not compatible past a certain version of Brackets and doesn't have time to update the extension, they can go ahead and add a max version.)

> **resolved** (pf) I think we should still let authors explicitly set a max if they want to -- if a dev _wants_ to be conscientious, we might as well accept their input.

> (kd) True, there's no harm in accepting what the developer says. We'll want to be clear in docs how/when to set that value.

> (re) What about the same argument for minimum version? For example, what if I introduce an extension in 0.20 that only uses API calls that have been in code since 0.15, then I don't want Brackets to think that min version is 0.20. Another option would be for Brackets to scan code and determine min version based on which API calls are used.

Here's an example to show the idea:

* I publish an extension called WriteMyCodeForMe. Brackets 0.20 is the current version at the time.
* The repository marks the extension as having no upperbound version of Brackets. (It may default to a minimum version of the current version when the extension is first published.)
* Brackets 0.22 comes out and starts warning (via developer tools console, possibly via other UI) that WriteMyCodeForMe is using a now-deprecated API. The warning states that the API will go away in 0.24.
* When a user:
  * runs Brackets 0.22
  * that includes WriteMyCodeForMe that was installed from the repository
  * and that copy of WriteMyCodeForMe has not been marked as max API version 0.23
  * a message is sent to the repository warning of the impending problem
* during the next update check, everyone else's copy of WriteMyCodeForMe would get the new max API version message
* we could even send the author an email message
* when Brackets 0.24 is released, if WriteMyCodeForMe has *not* been updated, everyone's copies will be disabled

> (pf) I'm confused re "a message is sent to the repository warning of the impending problem". Are we saying Brackets will do this _automatically_ without user intervention? That implies that when a deprecated API is called we can look up the call stack to determine which extension called us. Can that really be done reliably? (for example, IIRC filenames are not always available in stack traces when you don't have the dev tools open)

> (kd) You're right. *If* we were to end up with a sandboxed extension API, we would be able to do this reliably. Otherwise, we cannot. (I can think of some common cases that we *could* handle, but I can also think of others that we simply wouldn't have the info.)

> (pf) However, I sort of think the combination of spewing warnings to the console + social flagging might be enough. Or for that matter, maybe even just spewing warnings + the core team manually marking a max-version when we hear complaints (Twitter-driven development FTW!). We have 50+ extensions now and so far we've gotten by alright with only half of _that_ in place (since we have near-zero advance notice before APIs go away and never log deprecation warnings to the console).

> (kd) Excellent point.

What if no one had WriteMyCodeForMe installed, and then someone installed it on top of Brackets 0.24? There would be a button in the extension manager that a user can click to say "This extension isn't working properly with my version of Brackets."

> (pf) It may be hard to interpret that data robustly. If 20 people flag an extension, does it mean (a) it's incompatible with that version of Brackets, (b) it's buggy in general, or (c) it interacts poorly with some other popular extension (regardless of Brackets version) that those 20 users all have installed? Granted, in all those cases it's still useful feedback for us to capture :-) But making automated assumptions about version compatibility based on such data seems tricky.


The combination of these features would mean:

1. users see fewer extensions get disabled because of being "incompatible" (though probably not really incompatible)
2. developers need to do less busywork keeping their extensions up-to-date

> (nj) I like this "smart deprecation + crowdsourcing" idea. One further thought on top of that: if an
> extension had a good unit test suite, we could actually just run the unit tests and see if any deprecated
> APIs were hit. So perhaps each time we make a release, we could automatically run the unit tests for
> all the extensions in the repository that have them, which would automatically trigger the version
> incompatibility process (flagging it in the repo, emailing the developer, etc.) even before any user
> ran it. That would be even more incentive for devs to have good unit tests :), and in those cases
> it would make it much less likely that you'd hit the scenario where the first time we'd detect a
> problem is when a user hits it.

> (pf) The unit tests idea is cool, but I wonder how many extension devs will take the time to write a suite with good coverage -- everyone's doing this in spare time.

> (nj) Are there some kinds of API changes that would be difficult to support in this scheme? For example,
> if we need to change a method signature rather than getting rid of the method outright, would we need
> to essentially create a new method ("myMethodName2()")? Or, if a method signature doesn't technically
> change--because, say, its first argument is still an object--but the expectations for the fields in that
> object change, would we have to create a new method and deprecate the old one? I could imagine that
> becoming cumbersome over time. In general, it would be good to game out a few different API change 
> scenarios to think about how they would shake out (which might include things like object structure 
> changes, DOM structure changes, or perhaps even CSS changes that could break things, in addition to 
> method or module function changes).

> (pf) I think we should treat deprecation warnings as a "best effort" on our part instead of a rigid rule. We should strive to make API changes that either use a new vs. old method name, or when changing args do it in such a way that we can have (temporary) code at the top of the function that detects which signature you're using. But if we can't (e.g. major refactoring, HTML structure change that involves no callable API, etc.) then that should be ok too.

> (kd) I agree. I think that the places in which we're unable to do deprecation warnings would be good signals to us that we've identified a spot where having better defined extension APIs could help.

> (pf) And I think that also covers the CM case below --  since it's unsupported API, it seems fair to not try too hard there. (Although perhaps we should do another audit of `_codeMirror` usages to see if there are any glaring holes in the Editor API surface).

> (kd) I agree again. This was my thought exactly. Deprecation warnings for private APIs should *not* be expected.

> (nj) Another problematic case: API changes in CodeMirror--we got bitten by this in the CMv3 merge.
> In *theory*, no one should ever access CM APIs directly, and in a perfect world we would have
> Brackets APIs for everything that call through to CM where necessary. But we might not get there
> any time soon, and we might never want to get there (e.g., if someone wants to make a new CodeMirror
> mode as part of their Brackets extension, they should be able to plug that directly into CM).
> Perhaps we could do a trick where, if CodeMirror changes some API incompatibly, we manually patch
> in a proxy around that function that calls our deprecation logic. That's still not quite good
> enough, though, because (for the general scheme to work) we'd actually have to patch it in a
> Brackets release *in advance* of the CM API change. That would probably force us into a world where
> we're staying behind upstream master by a couple of weeks, which has caused us problems in the past
> (because then if we have critical bugfixes we need Marijn to make, we have to monkeypatch them 
> into an earlier version).

> (nj) Is there a danger that a single "deprecated API access report" causing the extension to become 
> disabled eventually would invite potential abuse? Would we wait for some number of these reports to 
> come in from different accounts? (Also, does that mean that we'd need a user to authenticate in order to
> send in a deprecation report?)

## Dependencies for Extensions ##

Next, we get into the idea of how extensions can depend on other extensions. The main purpose in allowing an extension to say that it depends on another is:

1. that extension plugs in to features provided by the one it depends on
2. if the user wants to install that extension, they automatically get the depended-on extension as well, to ensure that their newly installed extension works properly

Another use case that *could* be mentioned is an extension that relies on another extension to merely act as a *library*, providing useful functions and data structures. That does not seem quite a Brackets-specific a problem. In fact, that is a problem that is well-handled by npm, which tries to combine the best aspects of static and dynamic linking. My focus here is not on extension dependencies for purposes of code sharing, but rather for the purposes of *extending Brackets in ways not directly supported by Brackets core*. See "What npm Does" below.

In the [list of extensions today](https://github.com/adobe/brackets/wiki/Brackets-Extensions), we don't actually see a need for this. This is doubtless in part because Brackets extensions currently don't have a way to share code or make their objects and events available to one another. When we do our extension API research, we will doubtless improve the ways in which extensions can share and increase the likelihood that extensions will rely upon each other. A need for extensions to provide facilities for other extensions would be more obvious today if Brackets itself was built on a foundation of extensions.

Let's start with a hypothetical scenario. As part of the [JavaScript code hinting work](https://trello.com/card/2-code-hinting-javascript/4f90a6d98f77505d7940ce88/775), we're adding a JS parser (esprima) to the package. esprima parses on a worker thread, to ensure that the UI stays responsive. Even so, you'd ideally only do the parsing *once* and provide the data to as many consumers as needed. Perhaps one extension would like to use the AST that's built. Another may want to subscribe to events as the AST changes. (These are hypothetical, please ignore whether we can actually get these things from our current parser setup.)

These JS parsing features are going to be built-in to Brackets, so they're still not a good example of an extension requiring another. Imagine, though, an extension that wants similar access to TypeScript parsing from the [TypeScript extension](https://github.com/tomsdev/brackets-typescript-code-intel). Someone else comes along and they want to build a TypeScript beautification extension that takes advantage of an AST already built by the TypeScript extension.

### What npm Does ###

npm uses semantic versioning and fancy module loader tricks to make code happy when it brings in library code. Let's say an application uses libraries to connect to Twitter and Google. The Twitter package depends on some "oauth" library version 1.x and the Google package relies on version 2.x of the same library.

When the Twitter library says `require("oauth")`, it gets version 1.2.3. The Google library says `require("oauth")` and it gets version 2.1.2. Both libraries are happy.

npm also has a feature called ["peer dependencies"](http://blog.nodejs.org/2013/02/07/undefined/) that is specifically for plugins.

### But, That's Not The Problem We Have ###

In my TypeScript example, the problem we're solving is that we're not trying to access code in a library. We're trying to access a *service*. We want a current AST, or a stream of events.

Another way to think of our problem: some Brackets extensions will add UI elements. If we handled these like npm does, the UI bits would get hooked up *twice*, once for each version.

What if extension authors can use exactly the same mechanism that's suggested for Brackets up above? Smart deprecation + crowdsourcing.

The TypeScript extension will provide its services (through means that will be discussed during the extension API research). If it changes how these work, it will deprecate the old ways, marking the version planned to remove the deprecated APIs. Extensions that specify (in the metadata) that they rely on the TypeScript extension will be marked incompatible following the deprecation scheme. Users will also be able to use the same button to report that "an extension has become incompatible" to let people know that the extension no longer works as it used to (whether that's because of Brackets changing or TypeScript changing).

> (pf) Does this add any wrinkles to the updating process? For example, do we need to warn users that updating extension A will disable extension B, since they both depend on a service from extension C and A requires a newer version of it than B supports? Also, does this imply that we should encourage service-providing extensions to _not_ offer any user-facing UI themselves? (Otherwise, updating extension A could affect other features you weren't expecting the update to touch, since they're provided by some other extension C).

> (pf) Seems like this makes the "incompatible" flagging data even harder to process -- if users are flagging an extensions we don't know which of its multiple dependencies (including Brackets core) is the thing it's become incompatible with.

# Smart Deprecation + Crowdsourcing + API Versions #

In conclusion, my suggestion is that we use "smart deprecation + crowdsourcing" (if you haven't been reading the whole way through, see the section with that title above for more detail), with one further addition.

If we made some *huge* breaking change to how extensions work in Brackets, we'd be able to change the extension loader code in one way or another to recognize new vs. old extensions (something I did recently with my prototype that could load 3 different styles of extensions all in the same Brackets instance).

Extensions that offer capabilities for other extensions do not have this ability. If the TypeScript extension needed to make a big change, it should still be able to do so. The author could register a "TypeScript2" extension, but that wouldn't be so good because then users could have both "TypeScript" and "TypeScript2" installed and the behavior may not be desirable.

As a solution to this, my suggestion is that the Brackets API and extensions like the hypothetical TypeScript extension would have an "API version", which would be a whole number for simplicity's sake. This will almost always just be "1" (and can, in fact, default to that). When an extension makes a change to its API that is too large for the deprecation mechanism to work well, the "API version" is incremented and any extensions that used the old API version will no longer be compatible.