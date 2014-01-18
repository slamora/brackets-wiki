Brackets not working for you? Consider the following tips.

## System Requirements

* Mac OSX 10.6.8 or newer.
    * Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not being digitally signed yet.  To work around this, right click the Brackets app and choose Open, then click Open on the dialog that appears.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* Windows Vista/7/8/8.1 (x32) and (x64).
* WinXP w/ Service Pack 2.
* Linux Ubuntu 12.04 or newer (x32) and (x64).
* You should have at least 2 GB of RAM to do Live Development.

## Can't Install Brackets
### 1. Download the Right Package

Make sure you download one of the "brackets-sprint-XX.dmg" (Mac) or "brackets-sprint-XX.msi" (Windows) installers from the [Downloads page](http://download.brackets.io). 

### 2. Windows Vista: Nothing happens when launching installer

Some Windows Vista computers will block installers downloaded from the Internet, so nothing at all happens when you try to run the installer. To work around this: right-click the installer file, choose Properties, and click the Unblock button.

### 3. Windows error: "Installation directory must be on a local drive"

This can happen on some Windows machines. To work around this, try executing the installer from an elevated command prompt:

1. Open an elevated command prompt using one of the techniques on this page: http://www.sevenforums.com/tutorials/783-elevated-command-prompt.html
2. `cd` to the folder containing the installer.
3. Run the installer using msiexec, e.g.: `msiexec /i "brackets-sprint-xx-WIN.msi"` (where "xx" is the sprint number)

## Can't Run Brackets

### 1. Mac error: "Brackets can't be opened because it is from an unidentified developer"

The Brackets app is not yet signed, so depending on your security settings you might get a message saying you can't run an application from an unknown developer. If so, you'll need to Ctrl-click on the application and choose "Open", then click on the "Open" button in the dialog that comes up.

### 2. Check the File Permissions

If Brackets won't launch, check the permissions of the main executable files (e.g. using `ls -l`). On Mac:
* `bin/mac/Brackets.app` should be `drwxr-xr-x`
* `bin/mac/Brackets.app/Contents/MacOS/Brackets` should be `-rwxr-xr-x`

To fix permissions, use a command like `chmod +x bin/mac/Brackets.app/Contents/MacOS/Brackets`.

Some archiving programs, such as [Keka](http://www.kekaosx.com/en/) don't appear to preserve file permissions when unarchiving zip files. (More info [here](https://github.com/adobe/brackets/issues/1158)). If you run into this issue on Mac, try to unarchive the zip file by using Finder.

### 3. Clear The Cache
If you had previously used Brackets, your cache may have information that is conflicting with the most recent version. [Find your cache folder](https://github.com/adobe/brackets/wiki/Cache-Folder) and delete the cache. _Warning: this will reset all of your Brackets preferences._

### 4. Run Brackets From The Command Line
Next, try running Brackets from the command line. Open up a Terminal (or Command Prompt in Windows), navigate to the executable, and run Brackets. (On Mac, type `open bin/mac/Brackets.app`.). Did an error appear? If so, file an issue or find us on IRC or the mailing list and we'll try to figure it out.


## <a name="livedev"> </a>Live Development Isn't Working

### Live Development (aka Live Preview) Workflow with CSS, HTML, and JavaScript
Currently, Live Development works differently for different types of files:
* For CSS, your changes are applied in the browser immediately as you type, without reloading the page.
* For HTML, most changes are applied in the browser immediately as you type. There are some case when page is syntactically invalid (e.g. after you type '<' for a new tag but before you type the closing '>') when changes are not pushed to browser -- you will see line number and Live Preview icon turn red in these cases. Brackets will resume pushing changes to browser when syntax becomes valid again.
* For JavaScript, when you save your changes, the page is reloaded to reflect your changes.

We plan to add as-you-type Live Development support for JavaScript content in the near future.

### Files should be in Current Project
You can use `File > Open` to open any file on your computer, but Brackets' definition of a _project_ are the files in the folder opened using `File > Open Folder...`. Some (but not all) Live Development features require a node server, which means being in the current project, so make sure the files that you want to use with Live Development are in the current project.

### Disable Extensions
Extensions can cause problems with Brackets core code. The most notable extensions that may cause problems with Live Preview are Theseus and Debugger, so start with those. After that, but try disabling all extensions. Starting with Sprint 36, you can use `Debug > Reload Without Extensions` to quickly see if the problem is being caused by an extension.

### Using a Local Server
To use a local server, you need to specify a Base URL in the `File > Project Settings...` dialog.

If you are using a local server and are seeing these messages:

* Message in Chrome: "Oops!  Google Chrome could not connect to localhost:[port]"
* Message in Brackets Live Preview Error "Unable to load Live Development page".

Then verify that the local server has been started.

Also, Live HTML requires the Brackets built in node server, so it does not work with this configuration.

### Install Chrome For Multiple User Accounts (Windows Only)
If you get the error ``An error occurred when launching the browser. (error 2)`` when doing Live Development, installing [Chrome for multiple user accounts](http://support.google.com/chrome/bin/answer.py?hl=en&answer=118663) may solve the issue.
 
### Restart Your Computer
If you keep getting errors when trying to launch Chrome, or if you keep getting prompted to restart Chrome, try rebooting your machine. Rebooting has resolved many odd issues with Live Development.

Also, if you keep getting prompted to restart Chrome when switching between HTML files, make sure that you have another tab open in Chrome. This prevents Chrome from shutting down and restarting between each file, so it's much faster and smoother.

### Check Windows Registry
If Brackets cannot launch the Chrome browser on your Windows system, check the Registry setting here:

* HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths\chrome.exe

This is the file path that Brackets uses to launch Chrome. If this is not correct, then try reinstalling the Chrome browser at this location.

### Re-install Brackets

On Windows, you may run into issues starting Live Preview if you installed Chrome after installing Brackets. In that case, re-installing Brackets should fix the problem.

## Brackets is Running Slow
This section discusses some of the features that can affect performance and possible solutions.

### Highlight Active Line
This feature can negatively impact scrolling performance, so try turning it off with: View > Highlight Active Line

### Extensions
Most Brackets extensions are quite performant, but there are a few that can slow down Brackets. [Disable all extensions](https://github.com/adobe/brackets/wiki/Troubleshooting#disable-all-extensions) to help isolate the problem. Extensions that are know to be slow include:
* Show Whitespace

### File Searching
Using "Find in Files" and "JS Code Hinting" can be slow because of the number of files that are searched. You can try installing the [experimental Exclude Folders extension](https://github.com/gruehle/exclude-folders) to limit the number of folders that are searched.

### JavaScript Code Hinting
Collecting the information required to build the JS code hint lists can slow down Brackets. Start by reading the [Configuration section of the JavaScript Code Hints guide](https://github.com/adobe/brackets/wiki/JavaScript-Code-Hints#configuration).

You can disable the hints by moving the JavaScriptCodeHints folder out of www/extensions/default (installed version) or src/extensions/default (cloned version) folder and into the extensions/disabled folder, and restarting Brackets.

## Brackets Is Acting Weird

### Disable All Extensions
Choose _Help > Show Extensions Folder_. Rename the "user" folder to "user_disabled," close Brackets entirely, then re-launch Brackets.  (If you are developing your own extensions, also do a similar rename on the src/extensions/dev folder).

If this fixes the problem, you can identify the problematic extension by re-enabling extensions one-by-one: recreate the "user" folder and move extensions back into it one at a time, restarting Brackets each time until the problem comes back.

### Debug w/ The Developer Tools
If Brackets opens, but behaves incorrectly, don't forget you can open the Developer Tools. Under the Debug Menu, select "Show Developer Tools" to open an instance of the Developer Tools for Brackets. If you've used the Developer Tools in Chrome this will look familiar. Ensure the Console tab is open and see if any errors show up there.

### Can't Paste Text Into Brackets
There's a [known issue](https://github.com/adobe/brackets/issues/2531) when running the Webroot firewall where text can't be pasted into Brackets. If you're running webroot you can find [some potential workarounds on their support page](https://community.webroot.com/t5/Webroot-SecureAnywhere-Antivirus/Cut-Copy-or-Paste-Problems-Character-Entry-Issues-Scripting/ta-p/18396#.UhKfpmR4b9F). The team has also reached out to Webroot to try and get Brackets on the "known" list so this isn't a problem in the future.

## Still Having a Problem?
[File an issue](http://github.com/adobe/brackets/issues) or contact us via one of the channels mentioned in the [README](https://github.com/adobe/brackets/blob/master/README.md#i-want-to-keep-track-of-how-brackets-is-doing).   It is best to list the following information:

1.    OS & version
2.    brackets version
3.    extensions / plug-in listing
4.    detailed steps for problem & if it is repeatable
5.    steps for attempts of working around