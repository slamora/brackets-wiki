Brackets not working for you? Consider the following tips.

## System Requirements

Mac requires OSX 10.6 or newer. Win requires XP Service Pack 2 or newer. You should have at least 2 GB of RAM to do Live Development.

## Download

Make sure you download one of the "brackets-sprint-XX.zip" links from the [Downloads page](https://github.com/adobe/brackets/downloads). The "Download as zip" button will **not** work.

# Brackets Doesn't Load
## 1. Check The File Permissions

If Brackets won't launch, check the permissions of the main executable files (e.g. using `ls -l`). On Mac:
* `bin/mac/Brackets.app` should be `drwxr-xr-x`
* `bin/mac/Brackets.app/Contents/MacOS/Brackets` should be `-rwxr-xr-x`

To fix permissions, use a command like `chmod +x bin/mac/Brackets.app/Contents/MacOS/Brackets`.

Some archiving programs, such as [Keka](http://www.kekaosx.com/en/) don't appear to preserve file permissions when unarchiving zip files. (More info [here](https://github.com/adobe/brackets/issues/1158)). If you run into this issue on Mac, try to unarchive the zip file by using Finder.

## 2. Clear The Cache
If you had previous used Brackets, your cache may have information that is conflicting with the most recent version. [Find your cache folder](https://github.com/adobe/brackets/wiki/Cache-Folder) and delete the cache.

## 3. Run Brackets From The Command Line
Next, try running Brackets from the command line. Open up a Terminal (or Command Prompt in Windows), navigate to the executable, and run Brackets. (On Mac, type `open bin/mac/Brackets.app`.). Did an error appear?

# Brackets Is Acting Weird
## Debug w/ The Developer Tools
If Brackets opens, but behaves incorrectly, don't forget you can open the Developer Tools. Under the Debug Menu, select "Show Developer Tools" to open an instance of the Developer Tools for Brackets. If you've used the Developer Tools in Chrome this will look familiar. Ensure the Console tab is open and see if any errors show up there.