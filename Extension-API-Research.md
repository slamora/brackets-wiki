## Proposed API changes

In priority / implementation order. See also [Extension API Evolution](Extensions2): includes some longer-term ideas that are deferred here, but also has more concrete into on the near-term work listed here.

#### 1. Shared services architecture
* Allows extension-to-extension dependencies
* Easy shimming/mocking for unit tests
* Decouples our API from our file structure (so we can refactor w/o breaking APIs, offer services whose backing impl spans several files, etc.)
* Initially, we can offer this _without any core APIs_ available -- it can be used purely for cross-extension dependencies. Later we can move over core APIs individually, cleaning them up in the process.

#### 2. Service-aware code hinting
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
* _Further out_ -- still an open area of research
* But we'd want to settle this before exposing many core APIs as services, since the way we do restartless has a heavy influence on the API design

#### 6. Allow a generic Node utility module to be loaded shell-side
* Easier to reuse some of our own headless utility code
* Enables reusing NPM utility modules, if they're headless (no dependencies on Node-only APIs)