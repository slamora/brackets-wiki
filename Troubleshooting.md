Brackets not working for you? Maybe the following will help:

* [Having trouble with Live Preview?](#livedev)
* [Having trouble installing Brackets?](#installation)
* [Brackets not launching?](#launching)
* [Having trouble installing extensions?](#extensions)
* [Is Brackets running slowly?](#slow)
* [Error reading preferences file message](#prefs37)

Other information that may help you:

* [What are Brackets' system requirements?](#requirements)
* [How do I associate an extension with a type of file?](#fileassoc)

[Question not yet answered?](#notsolved)

## <a name="requirements"> </a>System Requirements

* Mac OSX 10.6.8 or newer.
    * Mountain Lion (OS X 10.8) by default will not allow Brackets to run since it's not being digitally signed yet.  To work around this, right click the Brackets app and choose Open, then click Open on the dialog that appears.  You only need to do that once -- afterward, launching Brackets the normal way will work also.
* Windows Vista/7/8/8.1 (x32) and (x64) (with administrator access)
* WinXP w/ Service Pack 2 (with administrator access)
* Linux Ubuntu 12.04 or newer (x32) and (x64).
* Debian Linux 8 or newer (Debian 7 compatibility will be restored once [#4816](https://github.com/adobe/brackets/issues/4816) is resolved).
* You should have at least 2 GB of RAM to do Live Development.


## <a name="installation"> </a>Can't Install Brackets
### 1. Download the Right Package

Make sure you download one of the "brackets-sprint-XX.dmg" (Mac) or "brackets-sprint-XX.msi" (Windows) installers from the [Downloads page](http://download.brackets.io). 

### 2. Windows Vista: Nothing happens when launching installer

Some Windows Vista computers will block installers downloaded from the Internet, so nothing at all happens when you try to run the installer. To work around this: right-click the installer file, choose Properties, and click the Unblock button.

### 3. Windows error: "Installation directory must be on a local drive"

This can happen on some Windows machines. To work around this, try executing the installer from an elevated command prompt:

1. Open an elevated command prompt using one of the techniques on this page: http://www.sevenforums.com/tutorials/783-elevated-command-prompt.html
2. `cd` to the folder containing the installer.
3. Run the installer using msiexec, e.g.: `msiexec /i "brackets-sprint-xx-WIN.msi"` (where "xx" is the sprint number)


## <a name="launching"> </a>Can't Launch Brackets

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


## <a name="livedev"> </a>Live Preview Isn't Working

### CSS, HTML, and JavaScript Editing
Currently, Live Development works differently for different types of files:

* For CSS, all changes are applied in the browser immediately as you type, without reloading the page.
    * CSS preprocessors are not currently supported - they are treated as "other file types" below
* For HTML, _most_ changes are applied in the browser immediately as you type. Updating pauses when the page is syntactically invalid (e.g. after you type '<' for a new tag but before you type the closing '>'), and the line number and Live Preview icon turn red. Brackets will resume pushing changes to the browser when syntax becomes valid again.
   * HTML live updating is disabled if you've specified a custom server URL in Project Settings.
* For JavaScript and other file types, when you save your changes, the page is reloaded to reflect your changes.

See **[How to Use Brackets](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#wiki-live-preview)** for more details.

### Files should be in Current Project
You can use `File > Open` to open any file on your computer, but Brackets' definition of a _project_ are the files in the folder opened using `File > Open Folder...`. Some (but not all) Live Development features require a node server, which means being in the current project, so make sure the files that you want to use with Live Development are in the current project.

### Disable Extensions
The Theseus extension is known to cause problems with Live Preview, and other extensions could potentially interfere also. Use [`Debug > Reload Without Extensions`](#wiki-disable-all-extensions) to quickly see if the problem is being caused by an extension.

### HTML Page is not Updating in Browser as you Type
Brackets pauses sending updates to browser when it detects an HTML Syntax Error. In this case, it should color the line number in red (but this can be scrolled out of view) so scroll through entire page to verify that there are no highlighted line numbers.

The Live Preview lightning bolt icon should be also colored red and have a tooltip of "Live Preview (not updating due to syntax error)" in this case, but there's a known bug being tracked as [issue #7126](https://github.com/adobe/brackets/issues/7126) where this sometimes doesn't happen. See [issue #7126](https://github.com/adobe/brackets/issues/7126) for an illustrated description including the line number and icon coloring.

There is [an issue](https://github.com/adobe/brackets/issues/5338) regarding "Live Preview can never update if initially launched with syntax error", so after fixing a syntax error, try stopping and restarting Live Preview.

### Using a Local Server
To use a local server, you need to specify a Base URL in the `File > Project Settings...` dialog (see [How to Use Brackets](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#wiki-live-preview) for details).

If you're using a local server and are seeing these messages such as "Oops! Google Chrome could not connect to localhost:[port]" (in Chrome) or "Unable to load Live Development page" (in Brackets), verify that your local server has been started.

As noted above, live HTML updating is disabled when using your own custom local server.

### Install Chrome For Multiple User Accounts (Windows Only)
If you get the error ``An error occurred when launching the browser. (error 2)`` when doing Live Development, installing [Chrome for multiple user accounts](http://support.google.com/chrome/bin/answer.py?hl=en&answer=118663) may solve the issue.
 
### Restart Your Computer
If you keep getting errors when trying to launch Chrome, or if you keep getting prompted to restart Chrome, try rebooting your machine. Rebooting has resolved many odd issues with Live Development.

### Keep Chrome Open
Try leaving an extra blank tab open in the instance of Chrome that is launched by Live Preview. This prevents Chrome from shutting down and restarting between each file, so Live Preview will launch faster; this may reduce some intermittent errors.

### Check Windows Registry
If Brackets cannot launch the Chrome browser on your Windows system, check the Registry setting here:

* HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths\chrome.exe

This is the file path that Brackets uses to launch Chrome. If this is not correct, then try reinstalling the Chrome browser at this location.

### Re-install Brackets

On Windows, you may run into issues starting Live Preview if you installed Chrome after installing Brackets. In that case, re-installing Brackets should fix the problem.


## <a name="slow"> </a>Brackets is Running Slow
This section discusses some of the features that can affect performance and possible solutions.

### Activity Monitor
On Mac OS 10.9 (Mavericks), Activity Monitor will say the Brackets Helper process is "Not Responding" even when it is working normally ([bug #5794](https://github.com/adobe/brackets/issues/5794)). You can safely ignore this unless Brackets is actually failing to respond when you click or type text.

### Extensions
Most Brackets extensions don't impact performance, but some may slow down Brackets (for example Show Whitespace can cause slow typing performance). Try [`Debug > Reload Without Extensions`](#wiki-disable-all-extensions) to quickly check if the problem is being caused by an extension.

### File Searching
Using "Find in Files" and "JS Code Hinting" can be slow because of the number of files that are searched. You can try installing the [experimental Exclude Folders extension](https://github.com/gruehle/exclude-folders) to limit the number of folders that are searched.

### Highlight Active Line
This feature can negatively impact scrolling performance, so try turning it off with: View > Highlight Active Line

### JavaScript Code Hinting
Collecting the information required to build the JS code hint lists can slow down Brackets. Start by reading the [Configuration section of the JavaScript Code Hints guide](https://github.com/adobe/brackets/wiki/JavaScript-Code-Hints#configuration).

You can disable the hints by moving the JavaScriptCodeHints folder out of www/extensions/default (installed version) or src/extensions/default (Git source) folder and into the extensions/disabled folder, and restarting Brackets.

## <a name="extensions"> </a>Having trouble installing extensions?

If your computer needs to use a proxy to get to the web, you'll need to configure the "proxy" setting. Use Open Preferences File from the Debug menu and then add a proxy variable to the JSON file. The value should be the URL of your proxy server.

(Note: this is new in Brackets 37.)


## Other: Brackets Is Acting Weird

### Disable All Extensions
Use `Debug > Reload Without Extensions` to quickly check whether the problem is being caused by an extension. To re-enable your extensions, just quit and relaunch Brackets, or choose Debug > Reload _With_ Extensions.

If this fixes the problem, you can identify the problematic extension by re-enabling extensions one-by-one:

1. Start with all extensions disabled: Choose _Help > Show Extensions Folder_ and move all extensions from the "user" folder into the "disabled" folder (this is similar to what Debug > Reload Without Extensions does).
2. Move one extension back into the "user" folder, then quit and re-launch Brackets.
3. Check if the problem has come back. If not, repeat step 2.

Alternative: Select _Debug > Show Developer Tools_ and look at the console.  If an error message is present, it may have a link to the code that is failing, which in turn may point out a specific extension.

### Debug with Developer Tools
Choose `Debug > Show Developer Tools` to open an instance of the Developer Tools for Brackets. If you've used the Developer Tools in Chrome this will look familiar. Check the Console tab for errors.

### Can't Paste Text Into Brackets
There's a [known issue](https://github.com/adobe/brackets/issues/2531) with the Webroot Identity Shield software blocking Paste in Brackets. If you're running Webroot, try the [workarounds on their support page](https://community.webroot.com/t5/Webroot-SecureAnywhere-Antivirus/Cut-Copy-or-Paste-Problems-Character-Entry-Issues-Scripting/ta-p/18396#.UhKfpmR4b9F). (Fully up-to-date Webroot should no longer treat Brackets as "unknown"; try [reinstalling](https://github.com/adobe/brackets/issues/2531#issuecomment-23747772) to update your copy).

## <a name="fileassoc"> </a>How do I associate an extension with a type of file?

Let's say you want ".inc" files to be treated like ".php" files. The easiest option today is to install the "Brackets Language Switcher" extension via the Extension Manager. You can also write [a really tiny extension](https://groups.google.com/forum/#!msg/brackets-dev/FDb_zq5isF0/rczgi9wccY8J) to Brackets.

There will likely be a preference for this soon (follow [this GitHub issue](https://github.com/adobe/brackets/issues/6831), if you're interested in that) and possibly a [language switching feature in Brackets core soon](https://github.com/adobe/brackets/pull/6409) as well.

## <a name="prefs37"> </a>Error reading preferences file (release 37)

In Brackets release 37, there's a known issue in which [an empty preferences file could cause Brackets to display an error message on startup](https://github.com/adobe/brackets/issues/7220). If you see this error, Brackets will still start and will open the (empty) preferences file into the editor. Follow these steps, and you'll be all set:

1. Type this into the editor:
    ```javascript
    {}
    ```
2. Save
3. Quit and restart Brackets

Starting with Brackets 36, we've added a bunch of preferences that let you tailor how Brackets works for you. Read more on the [How To Use Brackets](https://github.com/adobe/brackets/wiki/How-to-Use-Brackets#preferences) page.

The bug described here is fixed in Brackets 38.

## <a name="notsolved"> </a>Still Having a Problem?

* **[File an issue](https://github.com/adobe/brackets/wiki/How-to-Report-an-Issue)** - be sure to include as many details as possible (see link for specific guidelines)
* **Contact us** via one of the channels mentioned in the [README](https://github.com/adobe/brackets/blob/master/README.md#i-want-to-keep-track-of-how-brackets-is-doing)