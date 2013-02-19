# Extension Dependencies Research #

(This may eventually become documentations on how extension dependencies are handled.)

The experience that Node users have with npm provides some useful background:

* [npm now supports "peer dependencies" for packages that are needed but not required in](http://blog.nodejs.org/2013/02/07/undefined/)
* There was a useful discussion about how [npm avoids dependency hell](https://groups.google.com/forum/?fromgroups=#!topic/nodejs/0iQDxCIznO0) in October 2011

## Kevin's notes ##

* Two separate considerations
  * Brackets version
  * Dependency on other extensions that need to be downloaded as 
* npm supports version conflicts
  * different libraries can get different versions of the same 
* semver helps but is not granular enough…
  * we may change some APIs that would break some extensions, but not likely all or possibly even 
* What about Deprecation Warnings that not only print to the console but actually register the coming incompatibility with the repository?
* Firefox has the Add-on Compatibility Reporter
  * users can report that an extension is no longer compatible
  * the idea is that beta users would use this
  * the repo could track which version the reports started coming in for
* Are shared libraries worth it? (see Go for example)
  * what about shared capabilities?
  * rather than multiple extensions sharing an esprima, maybe it's more useful to share the AST that esprima produces? (one background thread parsing JS, rather than multiple!)
  * npm goes to great lengths to avoid DLL Hell. Maybe we don't want to reinvent that wheel? (Either use npm or statically include dependencies?)
* This gets into extension APIs but rather than sharing capabilities via modules, you share via named extension points and pub/sub
  * Rather than a fully dynamic pub/sub where subscribers may subscribe to a message that never gets published, what if we had warnings for messages that never get published?
* Will we be keeping every version of every extension around?
