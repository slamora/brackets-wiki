# November 2013 Live Development Future Notes

This page originally detailed Ian Wehrman's look at issues with Live Development's connection stability and made some recommendations for the future. We are now approaching the time where we want to implement that future. Ian's original notes appear in the "March 2013 Research" section below.

## Three Parts to the Browser Connection

As we imagine future possibilities such as Brackets supporting Live Development with browsers other than Chrome or Brackets running in a browser, it's worth noting that there are really three almost entirely separate parts:

1. The API we program to for Live Development features
2. The protocol that is spoken between Brackets and the browser that is being used for previewing
3. The network transport used to move protocol messages from place to place.

By consciously treating these as three separate things, we will be able to support the variety of use cases we want in the future.

## The API

Long-term, it would be great if the browsers themselves provided a standard JavaScript API that allowed browser-based tools to work with a separate browser window that contains the user's application. We're a long way away from that.

In the meantime, Brackets will have its own JavaScript API that is used by its various Live Development features. We should design this API to provide the functionality we need to implement our Live Development features without tying the API to a specific protocol.

## The Protocol

Today, Brackets knows how to speak the [Chrome Remote Debugging Protocol](https://developers.google.com/chrome-developer-tools/docs/debugger-protocol), which is what ties Brackets to Chrome. Firefox uses a [different protocol](https://wiki.mozilla.org/Remote_Debugging_Protocol). By abstracting the protocol away in our JavaScript API, as noted above, we can choose to support both the Chrome protocol and the Firefox one.

Or, we could make use of the [RemoteDebug](http://remotedebug.org/) project's work to speak to Firefox via the protocol used by Chrome. Or, we could create an entirely new protocol that can be interpreted by code that we inject into the page when we start Live Development. Ideally, we can make any of these choices without changing the features in Brackets.

We do want to support multiple browsers and the browsers speak different protocols, so somewhere along the way there will be a translation that occurs.

## The Network Transport

Safari uses the same protocol as Chrome (at least, it did when both browsers were based on WebKit). But, [Safari on iOS uses a different transport for the protocol](http://stackoverflow.com/questions/11361822/debug-ios-67-mobile-safari-using-the-chrome-devtools). If we want to support an in-browser Brackets, we may find ourselves needing to implement a different transport, one that proxies all of the messages through a separate server in order to deal with security restrictions. If we proxy all of the messages to a server in another location from the user, then we start having to think about the latency involved.

Choosing how we transport protocol messages is going to depend again on our target browsers, and also on other constraints such as latency.

## Conclusion

Keeping these three concerns separate will give us a chance to more easily add incremental support for the features we want in the future (things like support for other browsers and in-browser Brackets).

See also the "Future Directions" section that Ian wrote below with specific suggestions based on how the code is structured presently.

# March 2013 Research

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

The problem was addressed by pull [#3142](https://github.com/adobe/brackets/pull/3142) (and specifically by commit [5b2dc4](https://github.com/iwehrman/brackets/commit/5b2dc4eb928374d9bdd640f104bb2355d256b29b)) by ensuring that the interstitial page has loaded before attempting to load the target page. To ensure this, it is not sufficient to listen for the page load event of the interstitial page because the connection to the browser may not be set up before the page load event fires. Instead, the interstitial page was modified to set a global flag (`window.isBracketsLiveDevelopmentInterstitialPageLoaded`) on pageLoad, and that variable is polled remotely by Brackets. Once the flag is observed to be true, then we know that the page has certainly loaded. From that point, the agents can be loaded and, once they are ready (i.e., have registered their page load handlers), we can navigate away from the interstitial page to the target page, having certain knowledge that the page load event will be triggered after the agents registered their handlers. 

Pro tip: a simple but surprisingly effective way to flush out race issues with Live Development is to just open up multiple HTML or CSS files, enable on live development, and then hold down control-tab to very quickly switch among all the documents. This puts some stress on the open/openDocument/changeDocument/closeDocument/close part of the Live Development lifecycle. There are still existing bugs!

## Future directions

It seems likely that there exist more asynchrony issues in the Live Development implementation, especially w.r.t. the various agents interact with the single connection to the browser. Instead of hunting these races down and fixing them individually, a better next step would probably be to generalize Live Development to handle multiple simultaneous open sessions or windows. This would mean generalizing the Inspector to manage multiple WebSockets, parametrizing LiveDevelopment the lifecycle methods by WebSocket URL, updating the implementation of the agents from singletons to state-encapsulating, instantiable "classes" that are parametrized by the WebSocket URL they service, and also updating the the UI and XD workflow of Live Development accordingly.

A second dimension of generalization is along the connection between Brackets and the [Chrome remote debugger](https://developers.google.com/chrome-developer-tools/docs/debugger-protocol). This interaction should be abstracted to support live development with the remote debugging interfaces of other browsers like [Firefox](https://wiki.mozilla.org/Remote_Debugging_Protocol), or remote inspection and debugging services like [Weinre](http://people.apache.org/~pmuellr/weinre/docs/latest/). Next steps are to clearly define this interface (probably simplifying it in the process), and then refactor the existing Inspector and Agent objects to implement it. The trick will be finding the right level abstraction for the remote interface: presumably more fine-grained than just a few big methods to change CSS style definitions and highlight elements that match a particular rule, but more coarse-grained than the interface of the Chrome remote debugger which is currently used to implement those big features.