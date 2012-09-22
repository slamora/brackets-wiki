Brackets not working for you? Consider the following tips.

## System Requirements

* Mac OSX 10.6 or newer.
    * Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not being digitally signed yet.  To work around this, right click the Brackets app and choose Open, then click Open on the dialog that appears.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* Windows Vista/7.
* WinXP w/ Service Pack 2.
* You should have at least 2 GB of RAM to do Live Development.

## Brackets Doesn't Load
### 1. Download the Right Package

Make sure you download one of the "brackets-sprint-XX.dmg" (Mac) or "brackets-sprint-XX.msi" (Windows) installers from the [Downloads page](https://github.com/adobe/brackets/downloads). The "Download as zip/tar.gz" buttons will **not** work.

### 2. Lion/Mountain Lion Security Dialog

The Brackets app is not yet signed, so depending on your security settings, you might get a dialog on Lion or Mountain Lion telling you that you can't run an application from an unknown developer. If so, you'll need to Ctrl-click on the application and choose "Open", then click on the "Open" button in the dialog that comes up.

### 3. Check the File Permissions

If Brackets won't launch, check the permissions of the main executable files (e.g. using `ls -l`). On Mac:
* `bin/mac/Brackets.app` should be `drwxr-xr-x`
* `bin/mac/Brackets.app/Contents/MacOS/Brackets` should be `-rwxr-xr-x`

To fix permissions, use a command like `chmod +x bin/mac/Brackets.app/Contents/MacOS/Brackets`.

Some archiving programs, such as [Keka](http://www.kekaosx.com/en/) don't appear to preserve file permissions when unarchiving zip files. (More info [here](https://github.com/adobe/brackets/issues/1158)). If you run into this issue on Mac, try to unarchive the zip file by using Finder.

### 4. Clear The Cache
If you had previously used Brackets, your cache may have information that is conflicting with the most recent version. [Find your cache folder](https://github.com/adobe/brackets/wiki/Cache-Folder) and delete the cache.

### 5. Run Brackets From The Command Line
Next, try running Brackets from the command line. Open up a Terminal (or Command Prompt in Windows), navigate to the executable, and run Brackets. (On Mac, type `open bin/mac/Brackets.app`.). Did an error appear? If so, file an issue or find us on IRC or the mailing list and we'll try to figure it out.

## Brackets Is Acting Weird
### Debug w/ The Developer Tools
If Brackets opens, but behaves incorrectly, don't forget you can open the Developer Tools. Under the Debug Menu, select "Show Developer Tools" to open an instance of the Developer Tools for Brackets. If you've used the Developer Tools in Chrome this will look familiar. Ensure the Console tab is open and see if any errors show up there.