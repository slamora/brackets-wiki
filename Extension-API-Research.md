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
* _Restrictions:_ all APIs are async; all APIs can only receive & return JSON values (no complex objects)
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

#### Tabled - not in near future
* Run extensions in a sandbox and/or web worker thread
* Call Brackets APIs from Node-side code / enable code that uses Braackets APIs to be agnostic as to whether it runs Node-side vs. shell-side

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
    * Caveat: I didn't actually get them running against Kevin's branch; the ported code is only hypothetical
    * Caveat: I made up a bunch of API redesigns on the fly. But I think they are all fairly sane.
* **Verbosity** - Extensions stayed about the same size (on average only +2 lines), but the splitting of service reference declaration & assignment did feel annoying.
* **Storing off service references** - 3 of 4 extensions required some retained references (even after a bunch of light refactoring). I would want to store off separate refs for individual services, rather than just storing the whole `services` object, to avoid having to use fully-qualified names on every ref.
* **Restartless**
    * Already run into a _large set of APIs that would need to be auto-restartless_: commands, menu items, bare keybindings, keybinding patching, QuickOpen, EditorManager/DocumentManager listeners, PanelManager listeners, DOM injection (panels, toolbar icons, etc.), stylesheet loading
    * 3 of 4 extensions required _manual unload code_: per-Editor/Document listeners, caches hung off of Editor/Document objects (or could just leave these around), global DOM listeners. Need to play with some more extensions to see if these are the biggest sticking points -- could be mitigated if so:
        * Could offer singleton proxies for 'currentEditor', 'currentDocument', etc. to get around many use cases for per-Editor/Document listeners (w/o hitting the tricky per-object listener cleanup issues below).
        * Could offer a per-file cache/storage service to avoid hanging per-extension data directly off of core objects (though it's so "JavaScriptey" that some authors may keep doing it anyway?)
    * _Getting restartless right isn't easy_ -- in 2 of 4 extensions, I erroneously thought I was done making it restartless, then later realized I was wrong after a more careful review.


## Restartless questions & discussion

* Peter: Realistically, how many extensions would work without any manual work in the extension?  Would we trust extension authors to correctly identify what cleanup (if any) needs to happen manually?  (Not even sure I trust _myself _- see above). How bad would the experience be when a not-actually restartless extension fails to fully clean itself up?
    * We should definitely default to non-restartless unless an extension explicitly declares that it can handle it.
* Peter: Demand from users hasn't been that high yet.  Demand will rise with extension update notifications, but we could do some things to mitigate that - for example:
    * Automate a full restart instead of quit & manual-relaunch
    * Throttle / batch extension update notifications
    * Hold notifications until next launch (or quit, or idle time - it might be a preference kind of things, since Kevin pointed out he hates notify-on-launch while Peter prefers it)
    * Notify & download updates whenever, but don't force a restart until later (updates take effect whenever restart occurs)
* Need an extremely clear way to document / make predictable _which_ APIs are automatically reversible and which aren't...
* _Listeners_ are a very tricky area: hard for extensions to remember to detach all of them, but hard to make them auto-detach too. Auto-detach seems like it would have to mean:
    * a) Disallow listeners on non-singletons -- move to a pub-sub like architecture
    * b) All event-dispatching objects listen for extension unload to auto-remove listeners. But this implies all event-dispatching objects must be explicitly disposed (can't just be GC'ed). True for Editor and Document already, but this still feels like a nasty requirement...
    * c) Event-dispatching objects lazily check for & remove 'dead' listeners each time they trigger an event. Avoids the above limitations, but might be slow.
