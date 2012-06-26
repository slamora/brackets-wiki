Brackets not working for you? Consider the following tips.

##Permissions

If Brackets won't launch, check the permissions of the main executable files. On Mac OS X and Cygwin on Windows, they should be:
    drwxr-xr-x

Some archiving programs, such as [Keka](http://www.kekaosx.com/en/) don't appear to preserve file permissions when unarchiving zip files.

If you run into this issue on Mac, try to unarchive the zip file by using Finder.

## Cache
If you had previous used Brackets, your cache may have information that is conflicting with the most recent version. [Find your cache folder](https://github.com/adobe/brackets/wiki/Cache-Folder) and delete the cache.

## Command Line
Next, try running Brackets from the command line. Open up a Terminal (or Command Prompt in Windows), navigate to the executable, and run Brackets. Did an error appear?

## Developer Tools
If Brackets opens, but behaves incorrectly, don't forget you can open the Developer Tools. Under the Debug Menu, select "Show Developer Tools" to open an instance of the Developer Tools for Brackets. If you've used the Developer Tools in Chrome this will look familiar. Ensure the Console tab is open and see if any errors show up there.