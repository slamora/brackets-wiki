## Implementation Notes

### P1 vs. P2

These notes are for the implementation that started during innovation days and is slated to be completed during Sprint 36.

The first thing to note is that I've made *two* implementations. They have similar concepts at the top level, but the underlying implementations have different characteristics. The [first implementation](https://github.com/adobe/brackets/blob/6f363c93ece9fc37e757bba3e9aded5f66530ec1/src/preferences/PreferencesBase.js), which I'll call P1:

1. Was straightforward
2. Has to traverse the data structures on every preference look up
3. Has to look at basically *everything* in order to send out change notifications when a layer changes (i.e. whenever you switch files)
4. supports dynamic changes to the parts of a preference manager
5. Works

The [second implementation](https://github.com/adobe/brackets/blob/91ca1d1b36bf89cc5aabe21f6726d27fcb1e6c9a/src/preferences/PreferencesBase.js), henceforth P2:

1. Has a bit more indirection (but is only 70 lines longer)
2. Returns the value of a pref in a single object lookup
3. Looks at a far more limited set of data in order to compute change notifications
4. Does not support changes to the parts of the preferences manager
5. Does not quite work (but should pretty easily)

P2 may seem like a premature optimization, but I had two things in mind:

1. The number of preferences and use of preferences is likely to grow considerably once this is out there. Don't believe me? Open up Firefox and go to `about:config` in the URL bar.
2. I wanted to try to make a more coherent underlying model. It's not perfect, but I think the new implementation is a bit more coherent.

**Note**: in the end, we went with the P1 implementation due to its simplicity, ease of debugging and ability to handle a previously unconsidered use case: batch processes that need to look at many different files and not just the currently edited file.

### The High-Level Concepts

The code that I'm going to be talking about is in `PreferencesBase.js`. `PreferencesManager.js` acts as a singleton for getting and setting preferences.

At the top, the level at which most people will interact, is the **PreferencesManager** object. The most common operation is `get(id)`, which simply retrieves the value of a given preference.

The PreferencesManager has an *ordered* collection of **Scope**s. Each Scope holds one level of settings. `PreferencesManager.js` sets up a singleton PreferencesManager that has the following Scopes:

1. default (the default values for any settings that are explicitly registered)
2. user (the user's customized settings – the equivalent of Brackets' old localStorage-based system. This is the settings file that lives in AppData)
3. paths (a Scope for each .brackets.json file starting at the current open file)

For example, if `spaceUnits` has a value set in a .brackets.json file at the root of a project, then a call to `get("spaceUnits")` would return the project level value. The closest .brackets.json file to the open file comes first, followed by any other .brackets.json files going up the tree, user values next, default values last. If the setting is not known at all, `undefined` is returned.

Each Scope has an associated **Storage** object that knows how to load and save the preferences value for that Scope. There are two implementations: MemoryStorage and FileStorage.

The final concept that is common between P1 and P2 is that of **Layer**s. A Layer can be applied to every Scope and provides an additional level for preference lookups. Generally, a Layer looks for a collection of preferences that are nested in some fashion in the Scope's data. Under certain circumstances (decided upon by the Layer object), those nested preferences will take precedence over the main preferences in the Scope. (See the next section for further discussion of Layers).

**(Obsolete)** P2 adds a data structure that sits underneath the PreferencesManager and Scopes: the **MergedMap**. A MergedMap is a map-like object that merges multiple maps into a single one for lookups and sends out change notifications when there is a change to a value in the map. The PreferencesManager itself is a MergedMap and the Scopes are nested MergedMaps. The Layers are implemented as additional levels in the Scopes.

### PathLayer vs. LanguageLayer

Our initial thinking was to implement per-language settings, like so:

```json
{
    "spaceUnits": 4,
    "language": {
        "html": {
            "spaceUnits": 2
        }
    }
}
```

**Note**: As of this writing, the LanguageLayer has been removed.

The LanguageLayer implements this. When you change files, it looks up the language for the given file and then looks in the preference data to see if there's a "language" object and if that object has a match for the current language. In the example above, when editing HTML files `spaceUnits` will be 2. For all other files, it will be 4.

So, the LanguageLayer has a `setLanguage` method that is used to change the language and the Layer sends out a notification if there is a change to its data.

After releasing the demo of the P1 implementation, I got feedback from someone about the [EditorConfig](http://editorconfig.org/) project. There are a number of JavaScript projects that include EditorConfig files, so it would seem to make sense to ultimately support it. EditorConfig works based on paths/file globs rather than a higher level concept like "language". This is more powerful for our use, because it means that we could include settings specific to `thirdparty/codemirror`, for example. One note about EditorConfig: our current implementation just looks for a `brackets.settings.json` file at the root of the project. EditorConfig looks at each directory relative to the current file (and all the way to the root of the filesystem, I believe). Support that should be out of scope initially.

My thinking is not that we directly support EditorConfig at this time, but rather have a preferences model that *could* support EditorConfig easily down the line. Plus, working with globs is just generally more powerful than the language model.

PathLayer supports setting preferences based on the file path.

For the time being, we have choosen to support *only* the PathLayer, and that is likely adequate. The advantage to also enabling the LanguageLayer is that some languages are invoked by multiple extensions and the user would not need to explicitly pick which files are affected by a setting. Also, using the PathLayer from user-level preferences doesn't make a lot of sense whereas using the LanguageLayer does. There is [an issue open for resurrecting the LanguageLayer](https://github.com/adobe/brackets/issues/6558).

### Setting Preferences

As it stands now, there are a couple of outstanding issues with respect to setting preferences:

1. UI
2. Layers

The first is definitely the bigger issue. Where should the setting be set? If a user changes the `spaceUnits` from the UI, where does that get stored?

Previously, those changes were stored in localStorage. As an interim step, it makes sense to simply do the analog: store these changes at the "user" level.

As for issue #2, the API for setting preferences is `set(levelName, id, value)`. So, you can call `set("user", "spaceUnits", 2)`, for example. The API does not currently support changing settings for layers. The problem is that layers have their own segmentation which is specific to layers. For example, it's not enough to say `set(["user", "language"], "spaceUnits", 2)` because that is ambiguous about *which* language it should save the setting to. Paths are even more ambiguous.

It *could* be possible to change a setting at whatever level is currently providing that setting, but I'm not sure that's useful API. My thinking is that we'll let the future UI determine how the `set` API changes to accommodate settings at different levels.

For now, `set("user", "key", "value")` is really the only productive API.

### View State vs. Preferences

The old localStorage-based prefs implementation was used not only for user-changed settings but also for things that we could call "view state". The big difference between view state and preferences is that the user wouldn't edit view state manually or put view state in a version control system. Here are a couple examples of view state:

1. window size
2. current open projects
3. files in the working set

View state *can* (and I'd argue, should) be managed by the new PreferencesManager, but it should be a separate PreferencesManager instance that stores its values in a different file.

### Conversion

There is a [conversion function in PreferencesManager](https://github.com/adobe/brackets/blob/1484c427534c2a88ecc5cc77b619eddeaf7b1b00/src/preferences/PreferencesManager.js#L275) for converting from the old format prefs to the new.

# Original Research Results

## A. Introduction

This document presents background and a proposal for an updated Preferences/Settings implementation for Brackets and Edge Code.  It attempts to extend the current Preferences system implemented in Brackets, so that we can support an increased number of preferences in a broader variety of contexts and applications.  This proposal will specifically focus on the underlying implementation and resulting file storage.

This proposal does not attempt to cover the resulting UI changes except to the extent of how this implementation would provide support for various workflows and user stories.  It also does not attempt to itemize or discuss individual preference settings, other than to provide example workflows that this system should (or should not) support.

The body of this proposal describes the conditions and the details of the new system.  The appendices provide additional insight into how other products (ie. Adobe CC and competitive) implement their preference settings.

For more information, please see this [research page](https://github.com/adobe/brackets/wiki/Preferences) on the Brackets team Wiki.

## B. Proposed Use Cases and Workflows

The proposed, preference system must be able to store attributes with the following characteristics:

1. needed to persist across application sessions
1. preferred by the user
 * View > Line Numbers
 * View > Highlight Active Line
1. shared between the user’s devices/machines
 * portable features that work across platforms, screen sizes, in-browser/standalone
1. shared between users
 * features needed to satisfy team requirements
 * eg. View > Enable JSLint
1. common across projects
 * features specific to a particular project or purpose
 * eg. indentation requirements, such as 4 spaces vs tabs
1. common between languages
 * features specific to a specific file-type or language syntax
 * eg. html vs js vs C++ formatting

The proposed, preference system should avoid storing attributes with the following characteristics:

1. needed only within a single session
 * eg. find results, cache settings
1. dependent on the current document/context
 * eg. jslint errors
1. might change/conflict between multiple users
 * eg. github username, file paths
1. might change/conflict between multiple devices/machines
 * eg. window or panel sizes, last path opened

The proposed, preference system should support the following, example workflows:

1. setting the default indentation style
 * globally
 * per-project
 * per-file-type (eg. HTML) in a project
 * per-file-type, per directory/folder, within a project
1. configure JSLint options for a project
1. setting which font to use globally
1. setting a "hidden pref"
 * user wants to turn on a feature that is not yet fully shipping
 * a feature is causing trouble and user wants to turn it off
 * see about:config in Firefox or chrome://flags in Chrome
1. supporting settings of (3rd party) extensions
1. ignoring invalid, unknown, deprecated, etc settings


## C. Proposed Implementation

Currently, Brackets serializes its preferences into a not-easily-user-editable, binary store, persisted via localStorage.  Each setting is saved as a name-value pair, organized by client ID and then by module.  Each module, such as project/WorkingSetSort.js, calls the PreferenceManager (defined in PreferenceManager.js) to manage access to the preferences model.  In turn, the PreferencesManager serializes the data to localStorage via PreferenceStorage (defined in PreferenceStorage.js).

To support this new feature epic, we propose to extend this implementation in the following ways:

First, we propose updating the Brackets' existing PreferenceStorage.js implementation to use the native file system APIs (or [Glenn's upcoming, evolved file system](https://github.com/adobe/brackets/wiki/File-System)) to replace the currently used localStorage file i/o.  Rather than a single, binary localStorage database, the new implementation will serialize individual JSON files, named `brackets.settings.json` that will contain text-based, user-readable/-editable JSON name-value pairs.

_**[Glenn]** You mention above that things like full file paths and panel sizes should not be saved in preferences, but the proposal is to move **all** of existing preferences to a `brackets.settings.json` file. Since the current prefs store full paths and panel sizes, what are the plans for these items?_

_**[Bryan]** Great point.  What if we kept a file, for example 'local.settings.json', in the user's folder that stored the non-synchronized, non-shared settings?  Then, machine- and user-specific, such as cross-application instance, cached settings could be kept there._

Second, we propose extending the customization of preferences by supporting multiple `brackets.settings.json` files, based on the scope/context of the user's settings.  Specifically, through the presence of multiple, optional files, Brackets will support the following hierarchy of settings:

1. Default settings - defined in code;
1. per-Extension default settings - (optional) `brackets.settings.json` JSON file saved in an extension's root folder;
1. per-User default settings - `brackets.settings.json` JSON file saved in the "user folder".  Specifically, the already defined folder containing 'extensions' and 'cef_data'  eg. on Windows, <User>\AppData\Roaming\Brackets\;
1. per-Project settings - (optional) `brackets.settings.json` JSON file saved in the root of any opened project
1. per-Folder settings - (optional) `brackets.settings.json` JSON file saved in any (parent) folder of an opened document
  
The "effective" settings for any given document open in Brackets would then be the union of all settings defined from the above contexts.  Settings given in the most specific context (ie. lower in the list) would override those same settings given in a more general context (ie. higher in the list).  For example, the indentation settings optionally defined per-Project would override those same indentation settings perhaps defined at the per-User level.

Please note that both Brackets and Edge Code will expect the file to be named `brackets.settings.json`.  Doing so is consistent with existing names, such as already existing "brackets.config.json".  Also, using just one name will allow, for example, third-party extensions to work in both products without changing filenames.

_**[Glenn]** I understand the desire to have a single name, but using the name "brackets" in a user-editable file in Edge Code doesn't seem right. One possible option is using `settings.json` for extensions and `brackets.settings.json`/`edgecode.settings.json` for all other directories._

_**[Bryan]** Earlier feedback to this proposal requested using "brackets" in the filename.  Plus, there's also a precedent of the same pattern used for "brackets.config.json".  Still, since the settings filename would be more frequently used and more visible to the user, I do agree that `brackets.settings.json` may be confusing to an EdgeCode user.  We'd benefit from having a single name -- eg. extensions written for both Brackets and EC could just use a single preferences filename.  Consequently, I'm fine with just using `settings.json`._

_**[Kevin]** I think I was the one who suggested adding "brackets". The reason is two-fold: 1) `settings.json` is entirely non-descriptive. In a world of gruntfiles, jslint configurations and possible config files for a number of other tools that people may use, calling something `settings.json` seems uncomfortably generic. Other editors/IDEs store their project-related info in files/directories that are clearly named for that package. 2) This settings file will go into version control. If everyone editing the project is using one of Edge Code or Brackets, then it's fine to go with (productname).settings.json.  But are we sure that you won't have some people using Edge Code and some using Brackets? Are we sure that you won't have some people using the Chrome packaged app? `brackets.settings.json` strikes me as the generic name that would be in common with all of these._

_**[Bryan]** Good point.  I'm fine with going with `brackets.settings.json`, especially since we already do so with `brackets.config.json`.  I just want to make sure we choose a single filename to use for both, so that extensions only have to write to one file to be used by both products._

Third, although the actual design of the UI is beyond the scope of this proposal, we would like to propose that, unless specifically designed otherwise, settings updated in Brackets would, by default, be written to the "per-User default settings" context above.  So, for example, changing View > Line Numbers or View > Enable JSLint would write any changed values to the "per-User" `brackets.settings.json` file.  Of course, the upcoming UI redesign might choose another mechanism to write changes into a per-Project or a per-Folder basis.  For example, like Sublime 2, there could be an alternate set of menus or shortcuts to support the user in writing settings changes to a more specific context.

_**[Glenn]** What value is reflected in the menu? If "show line numbers" is false in the "per-user" settings file, but true in the "per-project" settings file, what is shown? What happens when I select the menu item?_

_**[Kevin]** Glenn has touched on the most difficult part of UI considerations for prefs like this. Here are a couple of ideas: 1) we add another layer to the prefs hierarchy: in-memory, one session only, settings. If you use the View menu to turn off line numbers, they are removed from view only for that session. 2) Same as #1, but with an additional aspect: if the setting is only set at the User defaults level, the setting is changed at that level. If it has been overridden at the project level, then the change is only temporary. 3) Avoid that confusion entirely! Remove the Enable JSLint menu item because that is extremely likely to vary from project to project. View > Line Numbers is not so likely to be a project setting but rather an individual user choice._

_**[Bryan]** To Glenn's first question, I'd think that the menu would reflect the effective value for the current context.  So, the menu would be checked b/c the "per-project" settings would override the "per-user" setting._

_To the second point, I agree with Kevin that this is the challenging part.  I like the idea of a temporary, in-memory session, which I guess is really just going to be a non-persistent "per-document" set of settings.  The only downside then is that the user would need to edit the JSON file to change the default for other documents(?).  Consequently, his suggestion #2 seems to be the most appealing.  In short, changing a setting in the UI would update the "per-user" settings (by writing it back to the JSON file).  However, if the effective settings were overridden at any other level (eg. per-project, per-folder, etc), then those changes would only be made to the temporary session._

_So, to take this point a bit further, what should probably happen is that when a document is opened (or created), we should generate the effective, temporary session settings based on the `brackets.settings.json` files.  Then, those are the settings used by that document's editor session until it is closed.  This means that if the JSON settings are changed elsewhere (eg. some other session editing the "per-user" JSON file), then those changes don't affect the currently open document until it has been closed and re-opened._

Fourth, although we are not discussing specific attribute definitions, we do propose that the new file format support possible per-Language settings as additional categories or groupings of settings within individual `brackets.settings.json` files.  For example, because a project could use 2 spaces for HTML and 4 spaces for Python, we'd suggest that this is represented in the one file, such as:

```javascript
{
    "indent": 4,
    "indentStyle": "spaces",
    "languages": {
        "html": {
            "indent": 2
        }
    }
}
```

_**[Glenn]** Are these language values "inherited"? In other words, if I specify HTML indent in the per-user settings, and set general indent (but not HTML indent) in per-project settings, what is used for HTML files?_

_**[Bryan]** Kevin suggested the language-specific settings, so he might have a better suggestion or clarification.  In general, I guess we could go either way, so long as we're consistent.  From a user's perspective, I'd expect that the general indent in per-project settings would *not* override the indent settings for html, in this example._

_**[Glenn]** This spec doesn't address preferences UI, but have we considered various UI proposals to make sure they can map to this format? Here are a couple concerns off the top of my head:_
* _Scope - how to represent settings at various scopes_
* _Language - how to define per-language settings, in various scopes_

_**[Bryan]** Not really yet.  Based on what I've seen of Sublime and other applications in general, making the UI too sophisticated to accommodate accessing every setting at every level can result in an overly complex UI that just ends up confusing the user.  Personally, I'd recommend a UI that targets the general user, but then just provide alternate support for the power user.  For example, let the primary UI (eg. menus, dialogs, etc) configure the "effective" settings.  If the power user wants to provide, for example, per-project-based settings, they can just edit the `settings.json` file explicitly._

_**[Kevin]** For many people, the user-level settings are enough. Their projects will have few contributors and the standards are likely the same between the projects. For anyone who frequently edits a variety of open source projects, per-project settings become a lifesaver because projects often have different settings. That said, I would hope that we can do a good enough job with things like detecting indentation that people wouldn't generally have to manually change the setting (it's more an issue when creating new files). It's quite possible that a UI for user-level prefs and normal editing for project-level prefs is all that's needed. We *could* open a custom editor for `brackets.settings.json` files, but it seems unlikely that that's a user story we'd get to!_

Lastly, as we transition to the new implementation, we propose (optionally) migrating existing settings saved in localStorage into the per-User default `brackets.settings.json`.  Alternatively, if time is running short, since there are not a lot of already existing preference settings, we could just drop/ignore already existing preference settings.

[Jeff] For path based preferences, do you build a hierarchy and look up the hierarchy of paths for a preference so if a preference isn't defined in the same folder as the file, do you look to its parent, then its parent or is it a flat scheme?

[Kevin] The current implementation just looks at the top of the project, which is not quite ideal. More ideal is to follow the [EditorConfig](http://editorconfig.org/) way, which is to look in all of the paths starting at the file's location. The farther up the tree you go, the lower the precedence for a setting.

[Jeff] Are preferences user based so that multiple users on the same system have their own set of preferences?  (i.e. are user prefs stored in the user's home or document folder)

[Kevin] They are stored in the same place that we store user extensions and such (`~/Library/Application Support/Brackets` on the Mac), so this is a user-specific location.

[Jeff] Why aren't extension preferences code based?  Can we rely on the extension to manage its own prefs?Conversely, why can't global core brackets prefs be stored in a base-settings JSON file?

[Kevin] Err, I missed that in my implementation. I believe that all of the preferences should be defined in code because I'm thinking that there may ultimately be little bits of code that go along with prefs (for handling conversion between versions, or customizing automatically generated preference UI).

[Jeff] Does brackets sync settings to the the CC cloud or is that an edge code only feature? 

[Kevin] This is not a Brackets feature.

[Jeff] Filename needs to begin with a dot by convention.

[Kevin] Agreed, and we discussed in the Architecture Meeting that perhaps we should go with JSON+Comments and call the file something like `.brackets.prefs` or `.brackets.jsonc` so that it's clear that it's not a pure JSON file.

## D. Proposed Next Steps

To move forward with this story, we propose the following, possible User Stories be entered into Trello:

1. PREFERENCES: Serialize Preferences as JSON vs localStorage
 * re-implement PreferenceManager to serialize JSON using native file APIs, rather than localStorage
 * first-run from older version, migrate existing preferences in localStorage into JSON files.  then, delete localStorage settings.
 * implement traversing user- and project-folders to generate "effective" preferences from multiple JSON files
1. RESEARCH: Preference UI
 * research possible changes to the UI in support of managing the multiple JSON files.  Or decide not to.
 * research feasibility and usefulness of a new Preferences Panel


# Appendices

These appendices provide additional research context re: this proposal.  
  
 
## Appendix A - Competitive Product Survey

Notes on other, competitive apps and how they implement their preference settings.

### Sublime 2
See also http://www.sublimetext.com/docs/2/settings.html.

Preferences configurable via menus.  Can also view and modify default, user, and Package-specific (eg. language, file-type) via Preference menu, which opens the corresponding *.sublime-settings file in the editor.  A full list of settings provided and documented via Preferences>Settings-Default menu.

Sublime 2 supports multiple settings files.  Settings files are evaluated hierarchically in order: global defaults, default by platform, user, project, syntax, user syntax, and then buffer-specific (aka current file only) settings.

Also includes preference settings for a "distraction free mode" (ie. full-screen, no adornments view mode).

### Coda 2
Preferences editable via menus, Coda>Preferences...  Settings are stored in a single, non-user-readable file: ~/Library/Preferences/com.panic.Coda2.plist.

### TextMate 2
See also http://blog.macromates.com/2011/git-style-configuration.

Preference settings configurable via menus, TextMate>Preferences...  Can also configure project level options via a .tm_properties file in the root directory of a project.  Each .tm_properties file affects every file below it.  .tm_properties in lower paths override those in higher.  A .tm_properties file in the user's home (~) operates at the highest level.

A list of all available settings: http://wiki.macromates.com/Reference/Settings
