## A. Introduction

This document presents background and a proposal for an updated Preferences/Settings implementation for Brackets and Edge Code.  It attempts to extend the current Preferences system implemented in Brackets, so that we can support an increased number of preferences in a broader variety of contexts and applications.  This proposal will specifically focus on the underlying implementation and resulting file storage.

This proposal does not attempt to cover the resulting UI changes except to the extent of how this implementation would provide support for various workflows and user stories.  It also does not attempt to itemize or discuss individual preference settings, other than to provide example workflows that this system should (or should not) support.

The body of this proposal describes the conditions and the details of the new system.  The appendices provide additional insight into how other products (ie. Adobe CC and competitive) implement their preference settings.

For more information, please see the [Edge Code Trello card](https://trello.com/card/5-research-preference-format/51072abad1b1f8a4560086bf/97) and the [Brackets team Wiki](https://github.com/adobe/brackets/wiki/Preferences).

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
  
## Appendix A - Creative Cloud Sync Support and Requirements

Currently, the Creative Cloud does not yet "officially" dictate how preferences syncing should work.  For the initial CC release, several other CS6 apps followed Photoshop's lead as noted below.  They used [CoreTech's C++ "C4" library](https://zerowing.corp.adobe.com/display/UITechnologies/UxTechC4) to abstract the synchronization.

Going forward though, Eric Wilde is leading an effort to design a "Sync Settings 2.0" framework to provide a more standardized preference sync implementation without requiring a deep understanding of Stormcloud APIs.  They hope to have more details in about 6 months, but here's how he describes it so far:

> From a technical point of view, what we're trying to achieve is an abstraction on preference management such that a native API takes a dictionary of key/value pairs and handles synchronization of that dictionary with Creative Cloud. The API can also accept file pointers and folder pointers as most products also have external files for presets (a type of preference such as brushes and patterns) and keep their preference files themselves in a folder hierarchy.

In short, choosing to implement our preference settings as JSON name-value pairs will allow us to smoothly integrate with any upcoming CC prefs sync strategy.

### Other Adobe app pref sync implementations

* [Flash Pro](https://zerowing.corp.adobe.com/display/FlashAuthoring/Sync+Preferences+using+Adobe+CCM)
* [Photoshop](https://zerowing.corp.adobe.com/download/attachments/800929995/PS+Sync+v1.4.pdf?version=1&modificationDate=1354034173103)

### CC Sync Prefs Notes

Taken from: https://zerowing.corp.adobe.com/download/attachments/799345645/SyncSettings_MVP_Nov6.pdf?version=1&modificationDate=1352404475607

1. sync prefs-
 * single user across multiple computers
 * shared prefs among a team
2. sync content
 * custom workspaces
 * settings
 * styles
 * keyboard shortcuts
 * code snippets
 * menu customization
 * etc
3. CC membership requirements
 * CC membership required to access sync
 * feature fully available to all Free and Paid (even if cancelled)
 * only allow sync to CC -- not to DropBox or other external folders
4. Desktop product requirements
 * minimal UI
 * no performance degradation
5. Sync Strategy
 * pushed to CC upon user request
 * received upon update notification
6. Conflict resolution
 * most recent wins
 * no user option to resolve manually
7. CC requirements
 * sync prefs do *not* affect CCM storage quota
 * sync prefs not visible in CC files

Additional notes:

Preferences on the Creative Cloud MVP: https://zerowing.corp.adobe.com/download/attachments/799345645/SyncSettings_MVP_1_8_13.pdf?version=2&modificationDate=1357937509583

Personas and Use Cases: https://zerowing.corp.adobe.com/display/CCM/Sync+Settings+Personas+and+Use+Cases
 
## Appendix B - Competitive Product Survey

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
