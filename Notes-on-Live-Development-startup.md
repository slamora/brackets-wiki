

Here's a problem with Live Development startup that may be manifesting itself as issue [#2858](https://github.com/adobe/brackets/issues/2858).

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

Two possible solutions to this ordering problem are: 

1. Fix the ordering problem, ensuring that the CSS agent does not register a pageLoad handler until after the interstitial page is known to have finished loading and before the target page has begun loading;  or
2. Don't fix the ordering problem, but empower the CSS agent to ignore pageLoad events from the interstitial page.

Pull request [#2865](https://github.com/adobe/brackets/issues/2865) attempts to solve the problem using the second technique. Upon handling  a pageLoad event, an asynchronous request is made for the window's URL and the CSS agents handler is only triggered once the response is received and if it indicates that the URL of the window is the target page. This solution has an apparently benign race: if the interstitial pageLoad event fires after the CSS agent registers its event handler and then subsequently refreshes on the client to the target page then the response to the asynchronous request could report the latter value. That should be ok, however, because (assuming the window with the target page doesn't navigate elsewhere) subsequent stylesheet queries will also be handled by the target page. NJ reports, however, that this pull request has not been merged because it seems to cause or expose other bugs. This could possibly be because other agents have racy startup behavior, and/or because the additional asynchronous request delays startup further. 

Instead, the first technique listed above may be better because it eliminates the possibility of startup races between the interstitial and target page loads for all the agents. An outline of this solution is as follows: 

1. Register a "startup" pageLoad event handler;
2. Navigate to the interstitial page initially; do not automatically redirect to the target page from the client;  
3. Handle the single page load event: 
  i) the agents, including the CSS agent, register their own page load handlers;
  ii) navigate to the target page; 
  iii) the agents handle the target page's pageLoad event.

One potential problem with this is that if the target page is already open when live development begins then it's still necessary to open the interstitial page instead of directly querying the page for its stylesheets. However, this already seems to be happening---though I'm not yet sure why. (My reading of `LiveDevelopment.open()` would lead me to believe that this shouldn't happen.) Potentially this could also slow down opening the first live development page, but that doesn't seem like a huge problem either, especially if the alternative is racy.