## Lifecycle research 

This section describes in some detail a problem with Live Development startup that was be manifesting itself as issue [#2858](https://github.com/adobe/brackets/issues/2858).

### A CSS Agent race condition

Upon Live Development startup, Brackets tries to interact with the Chrome remote debugger to find a web socket, one end of which is connected to the window/tab in which live development will take place. This interaction consists of:

1. a query to the remote debugger for a list of available sockets, each of which corresponds to a debuggable window (or tab or extension); and 
2. a search for a sockets with an associated URL that matches either the URL for the page on which live development is to occur (i.e., the "target" page), or the URL for an interstitial page. 

The search for a suitable socket may succeed either for the interstitial or the target page. The interstitial page may also redirect itself to the target page. (It's not clear to me why this complication is necessary.) 
There are also two failure modes: 

* the request for the list of sockets succeeds, but the list does not contain a socket with a matching URL; or 
* the request for the list of sockets fails, which indicates either that Chrome is not currently running, or that it is running but without remote debugging support enabled.

In the latter case, Chrome is restarted. In either case, a new window is opened with the interstitial page. 

Once a socket is open to a window holding either the interstitial or target page, the CSS agent registers a pageLoad event handler. This event is intended to mark the point at which it is safe to query the target page for a list of its loaded stylesheets that can be used for live preview. If this list isn't loaded correctly then live preview won't work right either. After registering the event handler, the location of the window is changed to the target page with the intent that the corresponding pageLoad event will be handled by the CSS agent. 

Unfortunately, there is a race condition here which, in some cases, can cause the CSS agent to handle the pageLoad event of the interstitial page instead of the target page. In particular, the following order of events is possible: 

1. begin loading interstitial page;
2. connect to window holding interstitial page;
3. CSS agent registers pageLoad event handler;
4. interstitial pageLoad event fires;
5. navigate to target page. 

In this case, the CSS agent handles the pageLoad event of the interstitial page instead of the target page, and hence queries the interstitial page for its list of stylesheets instead of the querying the target page.

### The solution

The problem was addressed by pull [#3142](https://github.com/adobe/brackets/pull/3142) (specifically, commit [5b2dc4](https://github.com/iwehrman/brackets/commit/5b2dc4eb928374d9bdd640f104bb2355d256b29b))by ensuring that the interstitial page has loaded before attempting to load the target page. To ensure this, it is not sufficient to listen for the page load event of the interstitial page because the connection to the browser may not be set up before the page load event fires. Instead, the interstitial page was modified to set a global flag (`window.isBracketsLiveDevelopmentInterstitialPageLoaded`) on pageLoad, and that variable is polled remotely by Brackets. Once the flag is observed to be true, then we know that the page has certainly loaded. From that point, the agents can be loaded and, once they are ready (i.e., have registered their page load handlers), we can navigate away from the interstitial page to the target page, having certain knowledge that the page load event will be triggered after the agents registered their handlers. 

Pro tip: a simple but surprisingly effective way to flush out race issues with Live Development is to just open up multiple HTML or CSS files, enable on live development, and then hold down control-tab to very quickly switch among all the documents. This puts some stress on the open/openDocument/changeDocument/closeDocument/close part of the Live Development lifecycle. There are still existing bugs!

## Future directions

It seems likely that there exist more asynchrony issues in the Live Development implementation, especially w.r.t. the various agents interact with the single connection to the browser. Instead of hunting these races down and fixing them individually, a better next step would probably be to generalize Live Development to handle multiple simultaneous open sessions or windows. This would mean generalizing the Inspector to manage multiple WebSockets, parametrizing LiveDevelopment the lifecycle methods by WebSocket URL, updating the implementation of the agents from singletons to state-encapsulating, instantiable "classes" that are parametrized by the WebSocket URL they service, and also updating the the UI and XD workflow of Live Development accordingly.

A second dimension of generalization is along the connection between Brackets and the [Chrome remote debugger](https://developers.google.com/chrome-developer-tools/docs/debugger-protocol). This interaction should be abstracted to support live development with the remote debugging interfaces of other browsers like [Firefox](https://wiki.mozilla.org/Remote_Debugging_Protocol), or remote inspection and debugging services like [Weinre](http://people.apache.org/~pmuellr/weinre/docs/latest/). Next steps are to clearly define this interface (probably simplifying it in the process), and then refactor the existing Inspector and Agent objects to implement it. The trick will be finding the right level abstraction for the remote interface: presumably more fine-grained than just a few big methods to change CSS style definitions and highlight elements that match a particular rule, but more coarse-grained than the interface of the Chrome remote debugger which is currently used to implement those big features.