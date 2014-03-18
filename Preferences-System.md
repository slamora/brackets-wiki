## Preferences System Basics

The preferences system has a collection of "scopes" that are searched in order for a given preferences key. There are a handful of ways to customize the behavior as needed, but a typical preference lookup will search scopes in this order:

* session: in-memory overrides that go away when Brackets is restarted. There wouldn't be anything in here by default.
* project: the .brackets.json file in the top-level directory of the project
* user: the brackets.json file in the user's appdata directory
* default: the default value as set by the `definePreference` call that declares the existence of a preference

If you set a value for a preference, the default behavior is to set that preference in the location from whence the preference's current value came. If the current value came from the default scope, then a call to `set` will set the value in the user scope.

Preference lookup is further customized by "layers". There is a PathLayer applied to the project scope that allows customization for specific files or sets of files in the project. The important thing to know about the PathLayer is that a preference lookup could change every time the user changes files in the editor.

The `stateManager` only has user and default scopes. It does have a `ProjectLayer` attached to its user scope which puts values into `project` object that is keyed on a project's top-level directory.

## Preferences and View State

The PreferencesManager module defines two PreferencesSystem objects: one for prefs and one for "view state". What should be a pref and what should be view state? A pref is something that:

1. The user may want to customize for a project and put in their version control system
2. The user may want to edit themselves

Any other configuration value you need to save would be view state. For example, "spaceUnits" is a preference because it is something that a person would want to store in version control. "staticserver.port" is something a person would want to hand edit. Things like window sizes or the checked state of a given menu item would likely be view state.

## Using the Preference System in Your Extension

The PreferencesManager module has convenience functions for working with the main preferencesManager and stateManager. Look in PreferencesBase to see everything that's available on a PreferencesSystem object.

If you are working with preferences and view state that are specific to your extension, you should prefix all of your preferences. There's an easy way to do that:

```javascript
    var PreferencesManager = brackets.getModule("preferences/PreferencesManager"),
        prefs = PreferencesManager.getExtensionPrefs("myext"),
        stateManager = PreferencesManager.stateManager.getPrefixedSystem("myext");
```

Now, anything pref you look up or set on `prefs` or `stateManager` will automatically have "myext." at the beginning.

If you're looking up global Brackets prefs (things like "spaceUnits"), you would just use the unprefixed functions.

### Defining a Preference

Once you have your prefixed PreferencesSystem, you can then define a preference.

```javascript
    prefs.definePreference("greeting", "string", "Hello");
    prefs.definePreference("showGreeting", "boolean", "true").on("change", function () {
        console.log("The value of showGreeting is now", prefs.get("showGreeting"));
    });
```

Define a preference with the ID of the pref (these would be prefixed to be `myext.greeting` and `myext.showGreeting`), the type of the pref and the default value. You can also pass in an options object with `name` and `description` properties as a fourth parameter that could be used to generate a mythical, friendly-looking UI.

`definePreference` returns a Preference object which allows you to listen for change events.

### Change Events

As noted in the last section, you can get change events for individual preference objects. You can also get change events for a whole PreferenceSystem:

```javascript
    prefs.on("change", function (e, data) {
        console.log("Possibly changed prefs:", data.ids);
    });
```

**Important**: Given the scoping for lookups and the way layers and scopes interact, its quite likely that you will get some change notifications when there wasn't actually a change. The notification is just telling you that a pref *may* have changed. It's up to you to then look up the current value of the pref for your context and decide if action needs to be taken.

### Looking up Prefs

The simplest form returns the current value at the highest precedence:

```javascript
    alert(prefs.get("greeting"));
```

You can optionally pass a context object in as a second parameter to be more specific about what you care about. If, for example, you have a preference that only makes sense at the project level (like Live Preview preferences), you can do this:

```javascript
    prefs.get("staticserver.port", PreferencesManager.CURRENT_PROJECT);
```

and the value will be looked up not for the current file (which could have come from outside of the project), but from the current project.

### Setting Prefs

The simplest form of setting a pref:

```javascript
    prefs.set("greeting", "Namaste");
```

As described in the first section, this will set the pref in the same place as its current value is pulled from. You can also be specific about where you want the new value to go:

```javascript
    prefs.set("greeting", "Namaste", {
        location: {
            scope: "user"
        }
    });
```

This will force the greeting preference to be set to "Namaste" at the user level.

If you set the value to `undefined`, the setting will be deleted at the appropriate level.

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

Some parts of Brackets stored information for specific files or projects in the old preferences system using keys that have a prefix. For example, DocumentManager stored working set files this way. The new preferences system stores the data in JSON objects, making it easier to have nested structures, but the conversion is more complex. Take a look at DocumentManager to see this more complex form of `convertPreferences` in action.

Default Preferences and Format  
```javascript
    {  
        "useTabChar": false,  
        "tabSize": 8,  
        "spaceUnits": 4,  
        "closeBrackets": false,  
        "showLineNumbers": true,  
        "styleActiveLine": false,    
        "wordWrap": false,  
        "linting.enabled": true,  
        "linting.collapsed": false,    
        "quickview.enabled": true,  
    }  
```
