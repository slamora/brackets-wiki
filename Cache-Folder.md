Brackets stores some data in a cache folder managed by CEF -- roughly equivalent to a browser cache, except specific to Brackets. At the moment, this folder stores both throwaway data _and_ user preferences.

If you're having trouble starting Brackets after upgrading from a previous build, try deleting this folder. You'll lose any stored preferences and saved information like the recent folder list.

## Cache location:

* Mac: ```~/Library/Application Support/Brackets/cefCache```
* Win XP: ```C:\Documents and Settings\<username>\Application Data\Brackets\cefCache``` -- (aka ```%appdata%\Brackets\cefCache```)
* Win Vista/7: ```C:\Users\<username>\AppData\Roaming\Brackets``` -- (aka ```%appdata%\Brackets\cefCache```)

## Cache location: (older brackets-app builds prior to sprint 13)

* Mac: ```~/Library/Application Support/com.adobe.Brackets.cefCache```
* Win XP: ```C:\Documents and Settings\<username>\Application Data\Brackets\cefCache``` -- (aka ```%appdata%\Brackets\cefCache```)
* Win Vista/7: ```C:\Users\<username>\AppData\Roaming\Brackets\cefCache``` -- (aka ```%appdata%\Brackets\cefCache```)

## Alternative Method

To clear out preferences, open the Developer Tools console and run

```
localStorage.clear()
```