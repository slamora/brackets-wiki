Miguel Castillo's [Brackets-Themes](https://github.com/MiguelCastillo/Brackets-Themes) extension is built in a way that would allow its code to be refactored and slimmed down and moved into core. Themes needs to be a core feature so that they can be packaged and shipped as independent extensions.

Here's a quick rundown of features that we'd want:

* Themes are extensions
* Themes are registered with a theme manager
* Perhaps themes would be allowed to be JS-less extensions? (Some mechanism other than calling an addTheme call?)
* We ship with a couple of themes out of the box (default extensions)
* Should use the new prefs system
* Custom settings screen is okay at this point
* Settings should use whatever technology we’re using for views (either current plain jQuery or React if we move in that direction)
* (optional) it would be beautiful to have custom extension manager UI for themes to show off screenshots and to separate themes from other extensions
* (optional) layered settings for stuff like font (theme can provide default user override)

And here are changes required to Brackets-Themes in order for it to meet our needs:

1. ThemeManager goes into core. ExtensionLoader has special logic for themes extensions that don’t have JS
2. Settings pared down to just your “General” screen
3. All of the other loading/scanning code can go away
4. Use jQuery promises, sorry :*(
5. Settings screen should not use Knockout
6. Use new prefs API
7. Unit tests for theme loading and the customizations (scrollbars, etc.)

## ThemeManager

The ThemeManager will live in `src/view/ThemeManager.js` with its matching tests in `test/spec/ThemeManager-test.js`. It will have an API to add a theme, but generally themes will be built as JavaScript-less extensions (see below). The ExtensionLoader will use this new API to register themes.

## Theme Extensions

ExtensionLoader will be modified to handle theme extensions. Theme Extensions will have a package.json file that looks something like this:

```json
{
    “name”: “super-cool-theme”,
    "title": "Super Cool Theme",
    “version”: “1.0”,
    “theme”: “superCoolStyles.css”,
    “screenshot”: “http://foobar.baz/bop.gif"
}
```

The `theme` property of the package.json file points to the CSS (or LESS) file that will be loaded. `screenshot` is new and could possibly be used in the extension manager to display a screenshot.

## UI

The View menu will have a new "Theme..." menu item. It will open up a dialog that has the settings from the Brackets-Themes "General" dialog as well as a selector for the theme itself.

