Brackets stores some data in a cache folder managed by CEF -- roughly equivalent to a browser cache, except specific to Brackets. At the moment, this folder stores both throwaway data _and_ user preferences.

## Cache location: (current builds)

* Mac: ```~/Library/Application Support/com.adobe.Brackets.cefCache```
* Win XP: ```C:\Documents and Settings\<username>\Application Data\Brackets\cefCache``` -- (aka ```%appdata%\Brackets\cefCache```)
* Win Vista/7: ```C:\Users\<username>\AppData\Roaming\Brackets\cefCache``` -- (aka ```%appdata%\Brackets\cefCache```)

## Cache location: (future brackets-shell)

* Mac: ```~/Library/Application Support/Brackets/cef_data```
* Win XP: ```C:\Documents and Settings\<username>\Application Data\Brackets\cef_data``` -- (aka ```%appdata%\Brackets\cef_data```)
* Win Vista/7: ```C:\Users\<username>\AppData\Roaming\Brackets\cef_data``` -- (aka ```%appdata%\Brackets\cef_data```)

## Alternative Method

To clear out preferences, open the Developer Tools console and run

```
localStorage.clear()
```