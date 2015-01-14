State of Live Preview (formerly known as "Live Development") as of January, 2015.

### Connecting

Live Preview has been implemented 2 ways:

1. Chromium Dev Tools web socket (original)

    - [Remote Debugging Protocol](https://developer.chrome.com/devtools/docs/debugger-protocol)
    - [LiveDevelopment/Inspector/Inspector.js](https://github.com/adobe/brackets/blob/master/src/LiveDevelopment/Inspector/Inspector.js#L30)

2. New Multibrowser Implementation using Injected scripts

    - Currently disabled and can be switch to with a feature flag
    - Connects to default browser
    - URL can be pasted into any other browser
    - See [Live Preview MultiBrowser](https://github.com/adobe/brackets/wiki/Live-Preview-Multibrowser) for details about this implementation.

Implementation is set in [LiveDevelopment/main.js `_setImplementation()' method](https://github.com/adobe/brackets/blob/master/src/LiveDevelopment/main.js#L222).

#### Live Preview Server

3 types of server connections:

1. Built-in nodejs server - this is the default connection type.
2. Use local server - this is done by specifying Base URL in Project Settings... dialog.
3. Fallback is `file://` protocol.

For more info see [Server API](https://github.com/adobe/brackets/wiki/Live-Preview-API) and [URL Mapping](https://github.com/adobe/brackets/wiki/Live-Preview-URL-Mapping) docs.


#### Interstitial page

Loaded to ensure a connection before starting agents and then navigating to document url.


#### **brackets-shell** Native Implementation

- `NativeApp.openLiveBrowser()` and `NativeApp.closeLiveBrowser()` defined in [appshell/appshell_extensions.js](https://github.com/adobe/brackets-shell/blob/master/appshell/appshell_extensions.js) calls operating system specific native `OpenLiveBrowser()` and `CloseLiveBrowser()` methods, respectively. Defined in:
    - [appshell/appshell_extensions_win.cpp on Windows](https://github.com/adobe/brackets-shell/blob/master/appshell/appshell_extensions_win.cpp)
    - [appshell/appshell_extensions_mac.mm on Mac](https://github.com/adobe/brackets-shell/blob/master/appshell/appshell_extensions_mac.mm)
    - [appshell/appshell_extensions_gtk.cpp on Linux](https://github.com/adobe/brackets-shell/blob/master/appshell/appshell_extensions_gtk.cpp)


#### Toolbar Icon

Used to toggle Live Preview on/off. Can also use **File > Live Preview** menu item.

Icon shows 3 different states: disconnected, connecting, and connected. Tooltips display more detailed state information. Twipsy is displayed at icon when browser is disconnected for external reasons.

### Remote Functions

Agents/RemoteAgent.js injects Agents/RemoteFunctions.js into Live Preview page for:

- Highlighting
- Live HTML


### Live CSS

- Editing CSS
    - LiveDevelopment.js `_styleSheetAdded()`
    - Creates a `CSSDocument` for each stylesheet
    - Updating `CSSDocument.onChange()` sends entire updated stylesheet to browser
- Highlighting all elements that apply to current rule
    - Can be enable/disabled with **View > Live Preview Highlight** menu item.
    - Implemented using remote function injection & calling


### Live HTML

Currently only supported with nodejs server

- Editing HTML
- Highlighting current element

- [Kevin's talk at JSConfEU](http://youtu.be/Axpi1_OVSdo) - first ~17 minutes
- [Research: HTML DOM Data Structure](https://github.com/adobe/brackets/wiki/Research:-HTML-DOM-Data-Structure)

### All Other File Types

Reload File on Save

[Live JavaScript Research](https://github.com/adobe/brackets/wiki/Live-Development:-Research-for-live-JavaScript) - not yet implemented.


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

- [Live Development: lifecycle research and future directions](https://github.com/adobe/brackets/wiki/Live-Development:-lifecycle-research-and-future-directions)
- `experimental` flag: *very* old, so most likely no longer works


