Brackets has a pretty good automated test suite that you can run from Debug > Run Tests, but that doesn't always cover issues with the overall UI and integrated functionality, or visual/layout issues that are only obvious if you're actually looking at the product. This is a set of manual tests intended to make sure we haven't broken the basic overall workflows of the product. The intention is to keep it quick--if it takes more than 5 minutes on a given platform it's too long.

There are also [Brackets server smoke tests](Brackets-Server-Smoke-Tests).

If you have trouble running through it or something is unclear, please post to the [brackets-dev mailing list](http://groups.google.com/group/brackets-dev).

Setup
=====

1. Make sure your ```git status``` is clean **and** your user extensions folder and `src/extensions/dev` folders are both empty (git status will _not_ tell you about these folders).
2. Quit and relaunch Chrome if it's open (so it's *not* in remote debugger mode).
3. If you've run the smokes previously, revert any changes you might have made in `brackets/test/smokes/citrus completed`.
4. Delete your [cache folder](Cache-Folder) (Mac:  `~/Library/Application\ Support/Brackets/cef_data`, Win: `%appdata%\Brackets\cef_data`).

Smoke test steps
================

1. Launch Brackets. Verify that the Brackets "Getting Started" folder is visible in the project panel and its index.html file is opened automatically.
1. File > Open Folder and browse to the Brackets source folder.
1. Click on the double-triangle next to the project name. The dropdown should show the "Getting Started" folder and an "Open Folder..." option
1. Switch back to the "Getting Started" folder using the project dropdown, verifying that it switches back to the previous project and shows its index.html.
1. Switch back to the "brackets" folder using the project dropdown.
1. Expand some folders in the brackets project, enough that it has to scroll.
1. Scroll around in the folder area. Verify that the shadows look right (appears at top when not scrolled all the way to the top) and there are no visual glitches.
1. From the Finder/Explorer window, drag the `brackets/test/smokes/citrus completed` folder onto Brackets (note that there's a space in the name; this is intentional). In the Project panel, verify that the folder opened and it contains "css" and "images" folders and an "index.html" file.
1. From the Finder/Explorer window, drag the `brackets/test/smokes/citrus completed/index.html` file onto Brackets. Verify that the file is opened, selected, and added to the working set.
1. File > New
1. File > Save, name the file ``temp.js`` in the current project
1. Type the following code
```
var foo = "", bar = function(){};
foo.
```
1. Verify ``String`` method code hints on ``foo``
1. Insert a new line 2 ``foo = bar``
1. On line 3, verify ``Function`` method code hints on ``foo``
1. In the project tree, right click on ``temp.js`` and choose delete (when prompted discard changes)
1. Open the OS Trash/Recycle Bin and confirm ``temp.js`` was deleted
1. Move mouse over the name of the source of an `<img>` tag (e.g. "images/events.jpg" on line 33). Verify that Hover Preview of the image is displayed properly with the width and height.
1. Set the cursor in the `<body>` tag immediately before the `>`.
1. Enter a space. Verify that a list of attribute hints pops up and you can navigate the list with up/down arrow key.
1. Hit Esc key to dismiss the code hints list, then delete the space so the cursor is after the "y" of "body".
1. Hit Cmd/Ctrl-E. Verify that it shows a single body rule and that everything is laid out properly.
1. In the native shell menu, choose View > Increase Font Size. Verify both the host and inline editors font size increases. The inline editor should not show a vertical scrollbar.
1. Click the lightning bolt in the upper right. If you trashed prefs, you'll get an info dialog explaining how live preview works. Hit OK.
1. You should see the page load in Chrome.
1. On Mac, you should get a dialog saying you need to relaunch Chrome. Click "Relaunch". Chrome should relaunch and open the page.
1. Back in Brackets, edit the background color for the <body> tag in the inline editor (#D90 is a nice color). Verify that the color changes in Chrome as you type. Also verify that the CSS file is added to the working set with the dirty bit set.
1. Hit Cmd/Ctrl-E. Verify that the inline editor closes.
1. Put the cursor after the `<a` in one of the navbar items and hit Cmd/Ctrl-E to open another inline editor.
1. Scroll up and down in the outer editor. Verify that the inline editor scrolls properly with the editor.
1. Resize the window. Verify that the rule list moves properly and there are no visual glitches.
1. File > Extension Manager, Install from URL..., and install the Emmet extension using the GitHub URL ``https://github.com/emmetio/brackets-emmet``
1. Verify the Emmet menu was added
1. Quit the app. Verify that you get a "save changes" dialog for any CSS files you edited through the inline editor, and choose to discard the changes.
1. Restart the app. Verify that the "citrus completed" project shows in the sidebar, and that the working set and current editor are showing the same files as when you quit. Also verify that the changes you had previously made were reverted (`git status` in the smokes folder should show clean).