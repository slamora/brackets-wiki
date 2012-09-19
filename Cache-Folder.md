Brackets stores some data in a cache folder managed by CEF -- roughly equivalent to a browser cache, except specific to Brackets. At the moment, this folder stores both throwaway data _and_ user preferences.

## Cache location:

* Mac: ```~/Library/Application Support/Brackets```
* Win XP: ```C:\Documents and Settings\<username>\Application Data\Brackets``` -- (aka ```%appdata%\Brackets```)
* Win Vista/7: ```C:\Users\<username>\AppData\Roaming\Brackets``` -- (aka ```%appdata%\Brackets```)

## Cache location: (older brackets-app builds prior to sprint 13)

* Mac: ```~/Library/Application Support/com.adobe.Brackets.cefCache```
* Win XP: ```C:\Documents and Settings\<username>\Application Data\Brackets\cefCache``` -- (aka ```%appdata%\Brackets\cefCache```)
* Win Vista/7: ```C:\Users\<username>\AppData\Roaming\Brackets\cefCache``` -- (aka ```%appdata%\Brackets\cefCache```)

## Alternative Method

To clear out preferences, open the Developer Tools console and run

```
localStorage.clear()
```