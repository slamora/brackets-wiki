## Preferences System Basics

The preferences system has a collection of "scopes" that are searched in order for a given preferences key. There are a handful of ways to customize the behavior as needed, but a typical preference lookup will search scopes in this order:

* session: in-memory preferences. There wouldn't be anything in here by default.
* project: the .brackets.json file in the top-level directory of the project
* user: the brackets.json file in the user's appdata directory
* default: the default value as set by the `definePreference` call that declares the existence of a preference

If you set a value for a preference, the default behavior is to set that preference in the location from whence the preference's current value came. If the current value came from the default scope, then a call to `set` will set the value in the user scope.

Preference lookup is further customized by "layers". There is a PathLayer applied to the project scope that allows customization for specific files or sets of files in the project. The important thing to know about the PathLayer is that a default preference lookup could potentially change every time the user changes files in the editor.

## Preferences and View State

The PreferencesManager module defines two PreferencesSystem objects: one for prefs and one for "view state". What should be a pref and what should be view state? A pref is something that:

1. The user may want to customize for a project and put in their version control system
2. The user may want to edit themselves

Any other configuration value you need to save would be view state. For example, "spaceUnits" is a preference because it is something that a person would want to store in version control. "staticserver.port" is something a person would want to hand edit. Things like window sizes or the checked state of a given menu item would likely be view state.

## Preferences UI?

Currently, there is no standard user interface for preferences. There is a [backlog item for one](https://trello.com/c/5GwJgKfi/480-8-preferences-dialog).

## Conversion from the pre-36 Preferences System

Prior to Brackets 36, PreferencesManager provided preferences that were namespaced to a given module and stored in localStorage. In Brackets 36, there is a new preferences system and, starting with Brackets 37, the old preferences system has been deprecated and its use should be discontinued.

The PreferencesManager module has a utility function, `convertPreferences` that will help convert from the old system to the new. The common case is like the one in the CodeInspection module:

```javascript
    PreferencesManager.convertPreferences(module, {
        "enabled": "user linting.enabled",
        "collapsed": "user linting.collapsed"
    });
```

This will convert the old "enabled" preference for the CodeInspection module to a new user-level preference called "linting.enabled". The conversion will only be done once, and the old value is saved in localStorage so that a user can switch between older and newer versions of Brackets.

Some parts of Brackets stored information for specific files or projects in the old preferences system using keys that have a prefix. For example, DocumentManager stored working set files this way. The new preferences system stores the data in JSON objects, making it easier to have nested structures, but the conversion is more complex. Take a look at DocumentManager (currently on the rlim/view-state-migration branch, but should be on master for release 37) to see this more complex form of `convertPreferences` in action.