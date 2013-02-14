Brackets stores some data in a cache folder managed by CEF -- roughly equivalent to a browser cache, except specific to Brackets. At the moment, this folder stores both throwaway data **_and user preferences_**.

If you're having trouble starting Brackets after upgrading from a previous build, try deleting this folder. You'll lose any stored preferences and saved information like the recent folder list.

## Cache location:

* Mac: ```~/Library/Application Support/Brackets/cef_data```
* Win XP: ```C:\Documents and Settings\<username>\Application Data\Brackets\cef_data``` -- (aka ```%appdata%\Brackets\cef_data```)
* Win Vista/7: ```C:\Users\<username>\AppData\Roaming\Brackets\cef_data``` -- (aka ```%appdata%\Brackets\cef_data```)

Note: for **Edge Code**, replace `Brackets` in the path with `Adobe/Edge Code`.

## Cache location: (older brackets-app builds prior to sprint 13)

* Mac: ```~/Library/Application Support/com.adobe.Brackets.cefCache```
* Win XP: ```C:\Documents and Settings\<username>\Application Data\Brackets\cefCache``` -- (aka ```%appdata%\Brackets\cefCache```)
* Win Vista/7: ```C:\Users\<username>\AppData\Roaming\Brackets\cefCache``` -- (aka ```%appdata%\Brackets\cefCache```)

## Less thorough alternative

To clear out preferences _only_ (not the lower-level CEF cache), open the Developer Tools console and run

```
localStorage.clear()
```