# Extensions 2 #

Compiled by Kevin Dangoor with feedback from the Brackets team. Feel free to contact me (dangoor@adobe.com) with feedback.

**Status:** This is the second draft.

This document follows the research on [Extension Management](https://github.com/adobe/brackets/wiki/Research:-Extension-Management). We have charted out a general path forward describing how Brackets users will be able to find, install and manage the extensions they use. Making these things easier will likely increase the number of Brackets extensions significantly.

Extension Management covered extension packages and how they are shipped around. In that research, we did not delve into how extensions are written.

## Restartlessness ##

Our product owner has expressed that he would like extensions to be restartless. More specifically, users can install, upgrade, disable, re-enable and remove extensions at will without having to restart Brackets.

We *could* add this capability to Brackets extensions as they exist today (Sprint 21) by defining an extension lifecycle with a hook for removing everything an extension has plugged in. However, by not making automatic deregistration a core feature of our API, we are placing a burden on our extension developers. Adding the "restartless" feature will make writing extensions more difficult and will likely result in extensions that don't fully clean up after themselves.

To do restartless "right" means building out new APIs specifically for extensions.

## Side Effects of a New API ##

If we build a new extension API, we can build it such that we make some attractive side effects possible.

* pieces of Brackets core can be more independent, less coupled, better factored out
* live documentation about the APIs that's available within Brackets
* extensions can themselves be extensible
* poorly written extensions don't break Brackets
* or adversely affect Brackets performance
* we can hide away more of the complexity of working with Node
* it may actually be possible to secure extensions later on if we decide that's valuable (we don't have to do it, but we also don't have to shut the door on it)
* lazy loading? (my experiments to date have not incorporated this trait, but if we decide we want this feature it should be possible to incorporate)

## New Extensions Conceptual Model ##

![Conceptual Model](https://www.evernote.com/shard/s24/sh/e28f2fe4-8ed6-4264-bc35-13e985b6e1dd/d8030dec6889b9c424c551c05c077012/res/8ad1a591-277c-49af-99be-952ef08c030f/skitch.png?resizeSmall&width=832)

To enable all of these new features, extensions will be given a [façade][http://en.wikipedia.org/wiki/Facade_pattern] that allows them to communicate with the core Brackets code, the extensions' own Node code and other extensions. The communication happens via a mediator which offers a shared messaging bus that supports publish/subscribe messaging, request/response messaging and a notion of shared data.

A variation of this architecture is described in Addy Osmani's [Patterns for Large-Scale JavaScript Application Architecture](http://addyosmani.com/largescalejavascript/).
