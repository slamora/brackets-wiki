## Proposed API changes

In priority / implementation order. See also [Extension API Evolution](Extensions2): includes some longer-term ideas that are deferred here, but also has more concrete into on the near-term work listed here.

#### 1. Shared services architecture
* Allows extension-to-extension dependencies
* Easy shimming/mocking for unit tests
* Decouples our API from our file structure (so we can refactor w/o breaking APIs, offer services whose backing impl spans several files, etc.)
* Initially, we can offer this _without any core APIs_ available -- it can be used purely for cross-extension dependencies. Later we can move over core APIs individually, cleaning them up in the process.

#### 2. Service-aware code hinting
* **Open question:** should this move down in priority, closer to "Begin exposing Brackets core APIs"?
* Most useful once we start offering core APIs as services (see below)
* (Could be done with the existing `brackets.getModule()` paradigm too -- but no point if those will be legacy APIs soon)

#### 3. Expose services from Node-side modules
* _Restrictions:_ all APIs are async; all APIs can only receive & return JSON value (no complex objects)
* Makes it easier to do our current arrangement, where more business logic is in shell-V8 code but certain APIs are implemented on Node side
* Makes it easier for core and extensions to reuse useful utilities via NPM packages
* A few NPM packages won't really be usable this way, without loading directly into the shell-side -- e.g. a collections libary

#### 4. Begin exposing Brackets core APIs as services
* Take the opportunity to clean up APIs as we go
* Existing getModule()-based APIs would continue to work, at least for the near future

#### 5. Track extension API calls so extensions can be made restartless semi-automagically
* _Further out_ -- plenty of open questions
* After extension auto-updating is implemented, we can wait to see what the experience feels like with simler mitigations (e.g. auto-restart, batched update notifications) to inform the priority of this.
* **But** we'd want to settle this before exposing many core APIs as services, since the way we do restartless has a heavy influence on the API design

#### 6. Allow a generic Node utility module to be loaded shell-side
* Easier to reuse some of our own headless utility code
* Enables reusing NPM utility modules, if they're headless (no dependencies on Node-only APIs)

## Open questions

* **Open question:** Need a nicer pattern for storing off references to services after `load()`. It's too hard to remove all refs from non-init code (see below), and it's ugly to pass `services` as an arg to every other function in the module.
    * Could we hang `services` off of a module-global var, e.g. `module.services.*`?
* **Open question:** Some Brackets core APIs feel weird to be called "services," e.g. StringUtils. Would we permanently leave some modules behind `getModule()`?
* **Open question:** Do we still want to define general API principles (to be used by core APIs when we port them over later) in Sprint 29's research story? Seems like what the APIs look like depends a lot on our restartless thinking, which we've deferred a bit (e.g. restartless might imply moving to pub-sub for everything).

#### Resolved questions

* The shared-services architecture (item #1 above) feels akin to writing a whole new module loader (we have to deal with load order, cycle breaking, etc.).
    * _Resolved:_ We think that's the right thing to do. Service dependencies have much in common with module dependencies, but just enough not in common to make reusing something like Require not viable.
* Do we require extensions to explicitly declare every core service they depend on?  Should we hide services that are undeclared, to prevent subtle bugs from slipping in? (e.g. extension depends on a service that's implemented in, or later migrated to, a core/default extension; without a declaration, we can't guarantee the load order will be right)
    * _Resolved:_ You must declare your service dependencies _only if_ you are in an extension, and the service is implemented by a different extension in the same "pool" as you (default vs. non-default). We’ll guarantee that all core modules load first, then all default extensions, then all user extensions (the latter part is not true today).  But within a pool, we'll need to use dependencies to determine a "safe" load order. -- This should reduce the burden on extension authors, where very few dependencies will be on other non-default extensions.

## Prototyping notes

#### Notes from Peter
* I converted four simple extensions over to an approximation of the services architecture: Markdown Preview, Everyscrub, Goto Last Edit, and File Navigation Shortcuts.
    * Caveat: none of these used Node-side code
    * Caveat: I didn't actually get them running against Kevin's branch; the ported code is onoly hypothetical
* **Storing off service references** - 3 of 4 extensions required some retained references (even after a bunch of light refactoring). I would want to store off separate refs for individual services, rather than just storing the whole `services` object, to avoid having to use fully-qualified names on every ref.