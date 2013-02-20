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

With its short release cycles, having Brackets extensions specify their min/max versions will result in users being penalized for keeping their Brackets up to date – many of their extensions will likely stop working when new updates come out. This is especially true given that Brackets has no beta test period at this time.

### Semantic Versioning ###

[Semantic Versioning](http://semver.org/) is a convention for version numbers that provides real meaning behind them. semver is a standard among npm users.

If Brackets used semver, an extension made for Brackets 1.x is *guaranteed* to work until Brackets 2.0.0 is released.

The experience that Node users have with npm provides some useful background:

* [npm now supports "peer dependencies" for packages that are needed but not required in](http://blog.nodejs.org/2013/02/07/undefined/)
* There was a useful discussion about how [npm avoids dependency hell](https://groups.google.com/forum/?fromgroups=#!topic/nodejs/0iQDxCIznO0) in October 2011

## Kevin's notes ##

* Two separate considerations
  * Brackets version
  * Dependency on other extensions that need to be downloaded as 
* npm supports version conflicts
  * different libraries can get different versions of the same 
* semver helps but is not granular enough…
  * we may change some APIs that would break some extensions, but not likely all or possibly even 
* What about Deprecation Warnings that not only print to the console but actually register the coming incompatibility with the repository?
* Firefox has the Add-on Compatibility Reporter
  * users can report that an extension is no longer compatible
  * the idea is that beta users would use this
  * the repo could track which version the reports started coming in for
* Are shared libraries worth it? (see Go for example)
  * what about shared capabilities?
  * rather than multiple extensions sharing an esprima, maybe it's more useful to share the AST that esprima produces? (one background thread parsing JS, rather than multiple!)
  * npm goes to great lengths to avoid DLL Hell. Maybe we don't want to reinvent that wheel? (Either use npm or statically include dependencies?)
* This gets into extension APIs but rather than sharing capabilities via modules, you share via named extension points and pub/sub
  * Rather than a fully dynamic pub/sub where subscribers may subscribe to a message that never gets published, what if we had warnings for messages that never get published?
* Will we be keeping every version of every extension around?
