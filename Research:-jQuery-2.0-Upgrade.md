** WIP **
This page can be used to keep track of changes for the jQuery 2.0 upgrade.

Upgrade Guides
----------------------------------------
<http://api.jquery.com/category/deprecated/>
<http://blog.jquery.com/2012/08/09/jquery-1-8-released/>
<http://jquery.com/upgrade-guide/1.9/>
<http://twitter.github.io/bootstrap/upgrading.html>

Deprecated Functions
----------------------------------------
* [`$.fn.andSelf`](http://api.jquery.com/andSelf/) (Replaced with [`$.fn.addBack`](http://api.jquery.com/addBack/))
    * [src/thirdparty/jstree_pre1.0_fix_1/jquery.jstree.js](../blob/master/src/thirdparty/jstree_pre1.0_fix_1/jquery.jstree.js)

* [`deferred.isRejected`](http://api.jquery.com/deferred.isRejected/) & [`deferred.isResolved`](http://api.jquery.com/deferred.isResolved/) (Replaced with [`deferred.state`](http://api.jquery.com/deferred.state/))
    * Resolved in [#3665](../pull/3665)

* [`deferred.pipe`](http://api.jquery.com/deferred.pipe/) (Replaced with [`deferred.then`](http://api.jquery.com/deferred.then/))
    * Resolved in [#4028](../pull/4028)

* [`$.fn.die`](http://api.jquery.com/die/)
    * Not Used?

* [`$.fn.error`](http://api.jquery.com/error/) (Replaced with [`$.fn.on('error',...`](http://api.jquery.com/on/))
    * Partial Fix in [#3814](../pull/3814)

* [`$.boxModel`](http://api.jquery.com/jQuery.boxModel/) (Removed)
    * ?

* [`$.browser`](http://api.jquery.com/jQuery.browser/) (Removed)
    * Bootstrap > Update Being Reviewed? [#3996](../pull/3996)

* [`$.sub`](http://api.jquery.com/jQuery.sub/) (Removed)
    * Not Used?

* [`$.fn.live`](http://api.jquery.com/live/) (Replaced with [`$.fn.on`](http://api.jquery.com/on/))
    * Not Used?

* [`$.fn.load`](http://api.jquery.com/load-event/) (Replaced with [`$.fn.on('load',...`](http://api.jquery.com/on/))
    * Partial Fix in [#3814](../pull/3814)

* [`$.fn.selector`](http://api.jquery.com/selector/) (Removed)
    * Not Used?

* [`$.fn.size`](http://api.jquery.com/size/) (Replaced with [`$.fn.length`](http://api.jquery.com/length/))
    * Resolved in [#4026](../pull/4026)

* [`$.fn.toggle`](http://api.jquery.com/toggle-event/)
    * ?

* [`$.fn.unload`](http://api.jquery.com/unload/) (Replaced with [`$.fn.on('unload',...`](http://api.jquery.com/on/) or [`$.fn.on('beforeunload',...`](http://api.jquery.com/on/))
    * Not Used?

jQuery 1.8 Upgrade
----------------------------------------
* [`$.fn.data('events')`]() (Removed)
    * Not Used?

* [`$.curCSS`]() (Removed)
    * Not Used?

* [`$.attrFn`]() (Removed)
    * Not Used?

jQuery 1.9 Upgrade
----------------------------------------