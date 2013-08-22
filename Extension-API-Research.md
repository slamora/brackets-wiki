(See ["Extension API Evolution"](Extensions2) for the _predecessor_ to this proposal, including more detail on the longer-term ideas such as restartless extensions & sandboxing)

## Proposed API changes

In priority / implementation order:

#### 1. Better module loader
* Easy shimming/mocking for unit tests
* Extensions authors get code hints for core APIs
* Less verbose per-module boilerplate, matching Node module format more closely
* [Implementation research notes here...](https://github.com/adobe/brackets/wiki/ModuleLoader)
* **Open question:** What do we do for worker-thread code? E.g. see _tern-worker.js_ in JS code hints &ndash; it looks like an unusual edge case for module loading...

#### 2. Cross-extension dependencies
* Built atop new module loader: APIs pulled in the same way as core APIs. Factored into the dependency-driven load order.
* Extensions are treated as singleton "service" providers rather than NPM-style dependencies
* Extension Manager automatically pulls down dependency extensions & identifies conflicts
* **Open question:** Do we enforce that the dependency is declared in package.json?

#### 3. Transparently expose APIs from Node-side modules
* _Restrictions:_ all APIs are async; all APIs can only receive & return JSON values (no complex objects)
* Makes it easier to do our current arrangement, where more business logic is in shell-V8 code but certain APIs are implemented on Node side
* Makes it easier for core and extensions to reuse useful utilities via NPM packages
* A few NPM packages won't really be usable this way, without loading directly into the shell-side -- e.g. a collections libary

#### 4. Clean up core APIs to simplify extension authoring
* Done incrementally, driven by pain points we see extension authors hitting
* Old APIs would continue to work for a while, with deprecation warnings

#### 5. Allow a generic Node utility module (installed via NPM) to be loaded shell-side
* Easier to reuse some of our own headless utility code
* Enables reusing NPM utility modules, if they're headless (no dependencies on Node-only APIs)
* **Open question:** If we wait too long to do this, does it encourage people to use the cross-extension dependency mechanism to share utility code instead? (Which is not really the right model - see discussion of services vs. libraries).

#### Tabled - not in near future
* Track API calls so extensions can be made restartless semi-automagically &ndash; Wait & see how extension auto-updating feels with simpler mitigations (e.g. auto-restart, batched update notifications).
* Run extensions in a sandbox and/or web worker thread &ndash; Wait & see how much extensions affect performance/stability as ecosystem develops. Could _potentially_ be done without changing APIs by mirroring model objects across sandboxes.
* Call Brackets APIs from Node-side code / enable code that uses Brackets APIs to be agnostic as to whether it runs Node-side vs. shell-side &ndash; Doesn't seem like a strong need.

## Open questions

* **Open question:** See inline note above re priority of NPM-style loader resolution work
* **Open question:** Should we deprecate `require()` paths that lack a "./" prefix? This is more consistent with Node and the module loaders we might use for Brackets. We could do it without breaking extensions since the deprecated `brackets.getModule()` API could continue to work the old way.

#### Resolved questions

* The shared-services architecture (item #1 above) feels akin to writing a whole new module loader (we have to deal with load order, cycle breaking, etc.).
    * _Resolved:_ We think that's the right thing to do. Service dependencies have much in common with module dependencies, but just enough not in common to make reusing something like Require not viable.
    * _Update:_ Using a new module loader is now the _centerpiece_ of the latest proposal.
* Do we require extensions to explicitly declare every core service they depend on?  Should we hide services that are undeclared, to prevent subtle bugs from slipping in? (e.g. extension depends on a service that's implemented in, or later migrated to, a core/default extension; without a declaration, we can't guarantee the load order will be right)
    * _Resolved:_ You must declare your service dependencies _only if_ you are in an extension, and the service is implemented by a different extension in the same "pool" as you (default vs. non-default). We’ll guarantee that all core modules load first, then all default extensions, then all user extensions (the latter part is not true today).  But within a pool, we'll need to use dependencies to determine a "safe" load order. -- This should reduce the burden on extension authors, where very few dependencies will be on other non-default extensions.
    * _Update:_ With a new module loader, this would be _automatically_ taken care of, the same way e.g. Require detects dependencies between core modules.
* Need a nicer pattern for storing off references to services after `load()`. It's too hard to remove all refs from non-init code (see below), and it's ugly to pass `services` as an arg to every other function in the module.
    * _Resolved:_ In new proposal, we continue to use the existing `require()` pattern with references both declared & assigned at top of module.
* Some Brackets core APIs feel weird to be called "services," e.g. StringUtils. Would we permanently leave some modules behind `getModule()`?
    * _Resolved:_ In new proposal, things aren't explicitly labeled as "services."
* Do we still want to define general API principles (to be used by core APIs when we port them over later) in Sprint 29's research story? Seems like what the APIs look like depends a lot on our restartless thinking, which we've deferred a bit (e.g. restartless might imply moving to pub-sub for everything).
    * _Resolved:_ Deferred to a later story that will specifically cover API cleanup. Restartless is lower on the priority list now, so we shouldn't block API cleanups on that anymore (and ideally we'd try to do restartless without imposing big new constraints on API design anyway).


## Prototyping notes

#### Notes from Peter
* I converted four simple extensions over to an approximation of the services architecture: Markdown Preview, Everyscrub, Goto Last Edit, and File Navigation Shortcuts.
    * Caveat: none of these used Node-side code
    * Caveat: I didn't actually get them running against Kevin's branch; the ported code is only hypothetical
    * Caveat: I made up a bunch of API redesigns on the fly. But I think they are all fairly sane.
        *  (kevin) That's not so much a caveat. That's a great exercise that I was hoping we'd do some of during this research
* **Verbosity** - Extensions stayed about the same size (on average only +2 lines), but the splitting of service reference declaration & assignment did feel annoying.
    *  (kevin) I'll note that it actually felt really good to me to remove the `define(function...` stuff at the top of the modules
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
        * (kevin) We talked about this one and it *seems* like the native menu problem should be tractable allowing for simply "refresh" of Brackets rather than relaunch. I will note that we'd also need to make sure that the Node side is restarted properly.
    * Throttle / batch extension update notifications
    * Hold notifications until next launch (or quit, or idle time - it might be a preference kind of thing, since Kevin pointed out he hates notify-on-launch while Peter prefers it)
    * Notify & download updates whenever, but don't force a restart until later (updates take effect whenever restart occurs)
* Need an extremely clear way to document / make predictable _which_ APIs are automatically reversible and which aren't...
* _Listeners_ are a very tricky area: hard for extensions to remember to detach all of them, but hard to make them auto-detach too. Auto-detach seems like it would have to mean:
    * a) Disallow listeners on non-singletons -- move to a pub-sub like architecture
    * b) All event-dispatching objects listen for extension unload to auto-remove listeners. But this implies all event-dispatching objects must be explicitly disposed (can't just be GC'ed). True for Editor and Document already, but this still feels like a nasty requirement...
    * c) Event-dispatching objects lazily check for & remove 'dead' listeners each time they trigger an event. Avoids the above limitations, but might be slow.
    * (kevin) d) if the objects exposed through the ServiceRegistry are actually wrappers that know which extension is accessing them, they can automatically track listeners.
        * (peter) At first I thought this would have the same problem as (b), but I _think_ it could duck that. E.g. all wrapper APIs return wrapper objects, and whenever a wrapper object is generated it's added to an extension-specific wrapper registry. When an extension unloads, we'd traverse its registry and dispose all wrapper objects (letting them unlisten from the core objects they're mirroring), and then throw out the registry itself to let the wrappers get GC'ed.

## Discussion of new proposal

(peter: the top sections above now reflect this proposal)

(kevin)

There are a whole bunch of different ways to approach our extension API and a path forward for it.  In my prototype, I tried to think about a way to make a whole bunch of things possible:

1. Restartlessness
2. Sharing of services between extensions
3. Code hinting (that even works as you dynamically update extensions)
4. Easier access to Node-side code
5. Ability to use Node modules (ideally installed via npm) in Brackets client side code (in addition to in Brackets Node-side code)
6. A possible route to allowing things to run in a Web Worker or other sandbox
7. Better unit testing of core code by handing the core code the other services that it needs (and mock services when testing)

After reviewing my prototype, Peter talked about how making the ServiceRegistry work is similar to writing a module loader. Along the way, I have had similar thoughts. In fact, I am even proposing that we write a module loader so that Brackets can seamlessly load modules from node_modules directories.

Peter did a great job of breaking out the various features so that we can consider their priorities independently. But, at the same time, we need to take a longer term, holistic view to know where we want to end up. Over time, I've become more convinced that restartlessness doesn't *have* to be an important feature, as long as the user experience for updates in unobtrusive.

On the other hand, I've started thinking that perhaps Web Workers/Sandboxes are more important than we've been thinking. The more extensions that a user has, the more likely that those extensions will start impacting the core performance of the editor. It happened with Sublime Text (which now sandboxes extensions) and it happened with Firefox (which has taken a variety of measures over time to deal with problematic extensions). Of course, sandboxing becomes tricky when we want to also provide UI flexibility for extensions (something that Sublime doesn't have to contend with).

In the end, I built a prototype that, at least in prototype quality, provides all of the features that I listed above. But, looking at the extensions that Peter ported to an imaginary ServiceRegistry-based API made me think that perhaps there's another, better path forward.

Our inability today to share modules between extensions and to conveniently share code between the Node side and the client side actually stems from our use of RequireJS. *Part* of my proposal has been to make a new module loader. It occurs to me now, depending on exactly what our priorities are, maybe building a new module loader should *be* the proposal, or at least the very first step.

Our own module loader would be able to load modules in whatever way we deem fit. And, for testing purposes, we can have simple enough control over our module loader to allow us to load in mocks for the services.

A new module loader would also represent a much subtler shift for extension authors. They wouldn't *have* to make any change, but they could start by simply:

* Removing the `define()` call at the beginning of their modules
* Changing from `brackets.getModule()` to `require()`

I think we'd still want to provide a higher-level interface to Node code for extensions, but I think that should be straightforward. Here's an example:

* an extension defines a module called "node.js"
* That module makes functions available on its `exports`
* client side code can `require('extensionName/node')`
* that module on the client side will automatically proxy to the Node side

That's actually a little more convenient than the API my prototype provides (though my prototype provided seamless communication in both directions).

### What about Restartlessness and Sandboxes?

If these *aren't* priorities, then we really are in a position to just do step-by-step incremental improvements to our API over time.

If we *do* decide to go restartless later, we still have some options. In this new scenario, we control the module loader. So, we would have the power to make `require('commands/CommandManager')` return a proxy object that *knows* what extension is using it so that added commands are automatically registered to the given extension.

For sandboxing, the hardest part is anything that manipulates the UI. For anything else, we could create a system of proxy objects that allow the extension author to manipulate the document object, etc. and the changes propagate from a worker to the main thread.

### What about code hinting?

This one should be a priority, I think. The prototype has nice code hinting. It gives the user an idea of common extension points.

We can still do that. If we can recognize that a user is working in the context of an extension, we can make `require()` calls provide hints for modules that are most likely to be of interest to them.


### So, what's this "top-level question"?

I guess there are two, one for Adam and one for the engineers.

Adam: what is the priority of restartlessness and sandboxing, given that they are likely expensive?

Team: what do you think about taking control of our module loading rather than building a services API?

## API Cleanups

I think we should still review extensions that are out there and incrementally update the extension APIs to be more convenient. For example, Markdown Preview adds a document change handler during which it turns off its document listener and turns on a new one. This kind of thing can be made more declarative ("listen for changes to documents with these extensions"). These also seem like the kind of events that could be pub/sub channels if we decided to go that route.