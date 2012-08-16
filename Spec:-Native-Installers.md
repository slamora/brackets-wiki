# Native Installer Proposal

This describes the plans for Bracket's _**initial**_ pass at implementing native installers. See these users stories: [Mac DMG](https://trello.com/card/2-brackets-installer-osx/4f90a6d98f77505d7940ce88/394) and [Win installer](https://trello.com/card/3-brackets-installer-win/4f90a6d98f77505d7940ce88/597). This spec may evolve as we refine the Brackets install experience down the road.

### General

* Separate downloads per platform
* Folder structure of the `brackets` repo part of the installed files is identical to the folder structure in Git.
* Future versions install _alongside_ older versions (they do not update/overwrite the old version).  All versions share the same preferences store, but users must manually copy over any extensions they had installed in the older version.
* Uninstalling deletes any extensions the user has installed.  (Unless they used _ln_ on Mac, which some extensions do recommend).
* New menu item that opens the extensions/user folder in Finder/Explorer.
* Unlike first launch today, brackets-shell _doesn't_ prompt for the /src folder.
* On first launch, Brackets opens an HTML readme file (instead of opening its own source code, as today).  The source code of the HTML file acts as a human-readable intro.
* All text in installer / EULA is localized for French, based on OS locale.

### Mac

* Download is a DMG file (disk image) created with the DropDMG tool
* Opening the DMG displays a localized EULA
* _All_ Brackets files are contained in the .app package (so the DMG contains only one item).
* User installs the standard way, dragging the .app onto /Applications.  A shortcut and background image in the DMG make this easier.
* The .app package name includes the release number, to ensure it doesn’t overwrite older versions (and any extensions therein) -- see bullet above.
* Uninstalling works the usual Mac way: just drag the .app out of /Applications into the trash.  The preferences store (which lies outside the .app) are not cleaned up, just like most other Mac apps.
* Visual assets required: DMG icon; DMG background image

### Win

* Download is an MSI generated with [Wix](http://wix.sourceforge.net/) (open-source, XML-driven installer generation technology from Microsoft)
* Installer shows EULA; no other options required
* Adds a Start menu shortcut
* Uninstall via OS Add/Remove Programs dialog.  Removes shortcut and entire install folder.
* Installed folder structure is similar to today's ZIP distros
* Users will get a UAC prompt when manually dragging files into the extensions folder, and extension source code will not be editable in situ (due to permissions).
* Visual assets required: installer .ico; uninstaller .ico (optional); vertical 2-point background gradient for installer UI (optional)

### Out of scope / future

* Installer/app _is **not** signed_
    * On Mountain Lion, users will be _unable to run Brackets_ without changing an OS setting or performing a one-time workaround (right-click, choose Open, confirm warning dialog).
    * On Windows, users will see a slightly more prominent warning dialog than usual at install time.
    * Both of these issues exist with our current plain-ZIP distros too -- not installer-specific.
* Extensions will be located in the install folder.  In the future, we should use a user-specific folder.
* Installer built from raw source code (no additional build/merge steps)
* Brackets will _not_ warn users if run directly out of the DMG (on Mac).  Some apps do this, or even offer to automatically copy themselves to /Applications.
* No rigorous build/version numbering required -- custom name/location for each release is enough.  (May be needed for the [update notification feature](https://trello.com/card/3-new-version-notification/4f90a6d98f77505d7940ce88/579), however).