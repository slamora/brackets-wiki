State of Live Preview (formerly known as "Live Development") as of January, 2015.

### Connecting

1. Chromium Dev Tools web socket

    - [Remote Debugging Protocol](https://developer.chrome.com/devtools/docs/debugger-protocol)
    - [LiveDevelopment/Inspector/Inspector.js](https://github.com/adobe/brackets/blob/master/src/LiveDevelopment/Inspector/Inspector.js#L30)

2. Injected scripts

    - Connects to default browser
    - URL can be pasted into any other browser
    - Currently disabled with a feature flag
    - See [Live Preview MultiBrowser](https://github.com/adobe/brackets/wiki/Live-Preview-Multibrowser) for details.


#### Live Preview Server

3 ways to connect:

1. Built-in nodejs server - this is the default connection type.
2. Use local server - this is done by specifying Base URL in Project Settings... dialog.
3. Fallback is `file://` protocol.

For more info see [Server API](https://github.com/adobe/brackets/wiki/Live-Preview-API) and [URL Mapping](https://github.com/adobe/brackets/wiki/Live-Preview-URL-Mapping) docs.


#### Interstitial page

Loaded to ensure a connection before starting agents and loading document url.


#### **brackets-shell** Native Implementation

- `NativeApp.openLiveBrowser()` and `NativeApp.closeLiveBrowser()` defined in [appshell/appshell_extensions.js](https://github.com/adobe/brackets-shell/blob/master/appshell/appshell_extensions.js) calls operating system specific native `OpenLiveBrowser()` and `CloseLiveBrowser()` methods, respectively. Defined in:
    - [appshell/appshell_extensions_win.cpp on Windows](https://github.com/adobe/brackets-shell/blob/master/appshell/appshell_extensions_win.cpp)
    - [appshell/appshell_extensions_mac.mm on Mac](https://github.com/adobe/brackets-shell/blob/master/appshell/appshell_extensions_mac.mm)
    - [appshell/appshell_extensions_gtk.cpp on Linux](https://github.com/adobe/brackets-shell/blob/master/appshell/appshell_extensions_gtk.cpp)


#### toolbar icon

- states: disconnected, connecting, connected
- tooltips


### Remote Functions

Agents/RemoteAgent.js injects Agents/RemoteFunctions.js into Live Preview page for:

- Highlighting
- Live HTML


### Live CSS

- Editing CSS
    - LiveDevelopment.js _styleSheetAdded
    - creates a CSSDocument for each stylesheet
    - updating CSSDocument.onChange sends updated stylesheet to browser
- Highlighting all elements that apply to current rule
    - can enable/disable with pref, menu command
    - remote function injection & calling


### Live HTML

Currently only supported with nodejs server

- Editing HTML
- Highlighting current element

- [Kevin's talk at JSConfEU](http://youtu.be/Axpi1_OVSdo) - first ~17 minutes
- [Research: HTML DOM Data Structure](https://github.com/adobe/brackets/wiki/Research:-HTML-DOM-Data-Structure)

### Live JS Research

- Not yet implemented.
- [Research: Live JavaScript](https://github.com/adobe/brackets/wiki/Live-Development:-Research-for-live-JavaScript)


### Reload File on Save

- All other file types...


### User Docs

- [How to Use Brackets: Live Preview](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#live-preview)
- [Troubleshooting Brackets: Live Preview](https://github.com/adobe/brackets/wiki/Troubleshooting#livedev)

Other Use Cases:

- When starting Live Preview...
    - if .css or .js file is selected, Brackets searches for nearest index.html
    - if .php is selected and Base URL is not specified, prompt for Base URL
- If Live Preview is running...
    - and another HTML file is selected, it switches to that file
    - and switch to a different project, Live Preview is disconnected


### Misc.

- [Live Development: lifecycle research and future directions](https://github.com/adobe/brackets/wiki/Live-Development:-lifecycle-

research-and-future-directions)
- `experimental` flag: *very* old, so most likely no longer works


