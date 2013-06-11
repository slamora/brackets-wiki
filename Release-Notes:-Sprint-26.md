_This is a draft!_
--------------------
_This document will not be finalized until the end of Sprint 26 -- approximately June 10._

What's New in Sprint 26
-----------------------
* **Extensions**
    * [Easier extension updating](https://trello.com/card/2-extension-listing-update/4f90a6d98f77505d7940ce88/877): When installing a newer version of an extension you already have installed, Brackets will automatically replace the older version.
* **File Management**
    * [Delete file/folder improvements](https://trello.com/card/2-delete-file-folder/4f90a6d98f77505d7940ce88/382): Better handling of unsaved changes.
* **Overall UI**
    * [Status bar redesign](https://trello.com/card/1-ux-implement-status-bar/4f90a6d98f77505d7940ce88/808): New visual design, updated JSLint validation status icons.
* **Architecture**
    * [Upgrade Brackets source to jQuery 2.0, Bootstrap 2.3.1, LESS 1.3.3](https://trello.com/card/3-upgrade-jquery-less-bootstrap/4f90a6d98f77505d7940ce88/813): From jQuery 1.7, Bootstrap 1.4.0, LESS 1.3.0
* **Code Hinting**
    * CSS URL Hints - In `url()` notation of an @import or property value, code hints will show relative paths to files in your project.


_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-25...sprint-26#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-25...sprint-26#commits_bucket)


UI Changes
----------
**Status bar** - Visual design, layout and typography changes. Extensions that use status bar indicators (via ``StatusBar`` ``addIndicator`` and ``updateIndicator`` methods) do not require any modifications.

**OK/Cancel Dialog Buttons on Windows** - OK and Cancel buttons in dialogs now display in the correct order. Previously the order was reversed.

API Changes
-----------
**Dialog API changes** - The following changes were made to the Dialog API:
* `showModalDialog` now receives an additional parameter: buttons. Buttons is a an array that contains for each button a class name, an id and a text. If omitted, the dialog will be created with an OK button that will close the dialog when clicked. Additionally, `showModalDialog` will always creates a new dialog element from a default template using the given class name, title, message and buttons. Instead of adding a dialog template to the DOM and using `showModalDialog` to show the dialog, use `showModalDialogUsingTemplate` with the parsed template as a parameter.

* The title and message parameters from `showModalDialogUsingTemplate` where removed.

* Both `showModalDialog` and `showModalDialogUsingTemplate` now return a Dialog object with 4 methods: `getElement` that returns the dialog jQuery object; `getPromise` that returns the promise used by the dialog (previously returned by both methods); `close` that can be used to close the dialog; and `done` to attach a done function to the dialog's promise as before. Notice that the promise is never rejected and is resolved when the dialog is dismissed, so the always and fail methods on the promise aren't required.

**jQuery API changes** - jQuery's [1.8 release notes](http://blog.jquery.com/2012/08/09/jquery-1-8-released/) and the [1.9 upgrade guide](http://jquery.com/upgrade-guide/1.9/) cover the API changes of our upgrade from 1.7 to 2.0. The list below highlights some major changes that we experienced during our upgrade:
 * Use `val()` instead of `attr("value")`
 * CSS position properties like `$foo.css("left")` always return pixel values. If you want percentage, use `$foo[0].style.left`.

**Bootstrap API / CSS changes** - 
The [Bootstrap Upgrade Guide](http://twitter.github.io/bootstrap/upgrading.html) covers all of the CSS changes. The primary changes affecting Brackets code are:
* Tables:
  * Add class `table`
  * Change `zebra-striped` to `table-striped`
  * Change `condensed-table` to `table-condensed`
* Alerts:
  * Change `alert-message` to `alert`

Pretty much all of the Bootstrap JavaScript plugins have [been re-written](http://twitter.github.io/bootstrap/upgrading.html#javascript). If your extension depends on specific Bootstrap JS components, make sure you test it fully.

New/Improved Extensibility APIs
-------------------------------


Known Issues
------------
* Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not digitally signed yet. To work around this, right click the Brackets app and choose Open. You only need to do that once -- afterward, launching Brackets the normal way will work also.
* [#2272](https://github.com/adobe/brackets/issues/2272): Windows Vista may not allow the Brackets installer to run (you may not see _any_ error message). To work around this, right-click the installer file, choose Properties, and click the Unblock button.
* [#3458](https://github.com/adobe/brackets/issues/3458): Quick View does not yet support the latest CSS gradient syntax (using `to` keyword, unprefixed `radial-gradient`, `repeating-linear-gradient` or `repeating-radial-gradient`).
* [#3570](https://github.com/adobe/brackets/issues/3570): Mac only - Quick View popover may not appear after resizing window or going fullscreen. Move the mouse to the top of the screen to fix.
(https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.
* [#3207](https://github.com/adobe/brackets/issues/3207): If you use a Sprint 21 or earlier build after using this build at least once, a few preferences such as Recent Projects may get reset. (You can back up your [[cache folder]] if you're concerned about this).
* _Debug > Run Tests_ is disabled in the installer/DMG distributions of Brackets, because the unit test code is not included. To run unit tests, [pull Brackets from GitHub](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#wiki-getcode) instead.


Community contributions to Brackets
-----------------------------------
* [Improving the Dialog API](https://github.com/adobe/brackets/pull/3086) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Added support for closing files on middle click](https://github.com/adobe/brackets/pull/3901) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper)
* [Update to jQuery 2.0.1, LESS 1.3.3 and Bootstrap 2.3.1](https://github.com/adobe/brackets/pull/4054) by [Bernhard Sirlinger](https://github.com/WebsiteDeveloper) and [Tucker Whitehouse](https://github.com/TuckerWhitehouse)
* [Replace deferred.pipe with deferred.then (jQuery 1.8 Deprecation)](https://github.com/adobe/brackets/pull/4028) by [Tucker Whitehouse](https://github.com/TuckerWhitehouse)
* [Resolves #3974 -- Inconsistent header capitalisation](https://github.com/adobe/brackets/pull/4069) by [kieran gorman](https://github.com/kjgorman)
* [Consolidate color matching regex and share across extensions](https://github.com/adobe/brackets/pull/4079) by [Razvan Caliman](https://github.com/oslego)
* [Fixed language manager unit test. Issue #3957](https://github.com/adobe/brackets/pull/4106) by [Daniel Seymour](https://github.com/DaBungalow)
* [Fix #4023: MenuSection object is out of sync with current Brackets Menus](https://github.com/adobe/brackets/pull/4112) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Fix #4091: Order of OK and Cancel buttons is reversed](https://github.com/adobe/brackets/pull/4113) by [Tomás Malbrán](https://github.com/TomMalbran)
* [Remove welcome project duplicate fix](https://github.com/adobe/brackets/pull/3317) by [Albert Xing](https://github.com/albertxing)

Contributions _from_ the Brackets community
-------------------------------------------

* None

Bugs fixed in Sprint 26
-----------------------
For details on the bugs addressed, please refer to [closed sprint 26 bugs](https://github.com/adobe/brackets/issues?labels=&milestone=13&state=closed). A few of the fixed bugs might not be caught by this search query, however.