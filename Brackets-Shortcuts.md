# Brackets Shortcuts

This page is to document which Shortcut keys are being used by Brackets. Since documenting a software project which is under development will be hard to keep up-to-date, I wrote a [Brackets Shortcuts Extension](https://github.com/redmunds/brackets-display-shortcuts) to generate a list of the current set of shortcuts defined to Brackets.

This extension displays all shortcuts defined for:
* Brackets
* CodeMirror (which not overridden by Brackets)
* Extensions which are currently installed

So, I loaded up all of the extensions currently listed on the [Brackets Extensions](https://github.com/adobe/brackets/wiki/Brackets-Extensions) page, started up Brackets, opened the Shortcuts Panel (via View > Display Shortcuts), opened up a new page, and the clicked the "Copy to Current Document" to create the table below.

So, if you create a new extension for Brackets, follow the same process and add rows to the table to show which shortcuts you are using.

***

<table>
    <tr class="shortcut-header">
        <td class="shortcut-base"><strong>Base Key</strong></td>
        <td class="shortcut-binding"><strong>Key Binding</strong></td>
        <td class="shortcut-cmd-id"><strong>Command ID</strong></td>
        <td class="shortcut-cmd-name"><strong>Command Name</strong></td>
        <td class="shortcut-orig"><strong>Origin</strong></td>
    </tr>
    <tr>
        <td class="shortcut-base">+</td>
        <td class="shortcut-binding">Ctrl-+</td>
        <td class="shortcut-cmd-id">view.increaseFontSize</td>
        <td class="shortcut-cmd-name">Increase Font Size</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">,</td>
        <td class="shortcut-binding">Ctrl-Shift-,</td>
        <td class="shortcut-cmd-id">com.notwebsafe.select-parent</td>
        <td class="shortcut-cmd-name">Select Parent</td>
        <td class="shortcut-orig">Extension (Notwebsafe)</td>
    </tr>
    <tr>
        <td class="shortcut-base">-</td>
        <td class="shortcut-binding">Ctrl--</td>
        <td class="shortcut-cmd-id">view.decreaseFontSize</td>
        <td class="shortcut-cmd-name">Decrease Font Size</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">.</td>
        <td class="shortcut-binding">Ctrl-Shift-.</td>
        <td class="shortcut-cmd-id">com.notwebsafe.select-child</td>
        <td class="shortcut-cmd-name">Select Child</td>
        <td class="shortcut-orig">Extension (Notwebsafe)</td>
    </tr>
    <tr>
        <td class="shortcut-base">&#x2F;</td>
        <td class="shortcut-binding">Ctrl-&#x2F;</td>
        <td class="shortcut-cmd-id">edit.lineComment</td>
        <td class="shortcut-cmd-name">Comment&#x2F;Uncomment Lines</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">&#x2F;</td>
        <td class="shortcut-binding">Ctrl-Alt-&#x2F;</td>
        <td class="shortcut-cmd-id">pflynn.searchCommands</td>
        <td class="shortcut-cmd-name">Search Commands</td>
        <td class="shortcut-orig">Extension</td>
    </tr>
    <tr>
        <td class="shortcut-base">0</td>
        <td class="shortcut-binding">Ctrl-0</td>
        <td class="shortcut-cmd-id">view.restoreFontSize</td>
        <td class="shortcut-cmd-name">Restore Font Size</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">=</td>
        <td class="shortcut-binding">Ctrl-=</td>
        <td class="shortcut-cmd-id">view.increaseFontSize</td>
        <td class="shortcut-cmd-name">Increase Font Size</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">A</td>
        <td class="shortcut-binding">Ctrl-A</td>
        <td class="shortcut-cmd-id">edit.selectAll</td>
        <td class="shortcut-cmd-name">Select All</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">A</td>
        <td class="shortcut-binding">Ctrl-Shift-A</td>
        <td class="shortcut-cmd-id">AlignAssignments.align</td>
        <td class="shortcut-cmd-name">Align Assignments</td>
        <td class="shortcut-orig">Extension</td>
    </tr>
    <tr>
        <td class="shortcut-base">C</td>
        <td class="shortcut-binding">Ctrl-Shift-C</td>
        <td class="shortcut-cmd-id">file.previewHighlight</td>
        <td class="shortcut-cmd-name">Live Highlight</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">D</td>
        <td class="shortcut-binding">Ctrl-D</td>
        <td class="shortcut-cmd-id">edit.duplicate</td>
        <td class="shortcut-cmd-name">Duplicate</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">D</td>
        <td class="shortcut-binding">Ctrl-Shift-D</td>
        <td class="shortcut-cmd-id">edit.deletelines</td>
        <td class="shortcut-cmd-name">Delete Line(s)</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">E</td>
        <td class="shortcut-binding">Ctrl-E</td>
        <td class="shortcut-cmd-id">navigate.toggleQuickEdit</td>
        <td class="shortcut-cmd-name">Quick Edit</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">E</td>
        <td class="shortcut-binding">Ctrl-Shift-E</td>
        <td class="shortcut-cmd-id">pflynn.searchWorkingSetFiles</td>
        <td class="shortcut-cmd-name">Go to Open File</td>
        <td class="shortcut-orig">Extension</td>
    </tr>
    <tr>
        <td class="shortcut-base">F</td>
        <td class="shortcut-binding">Ctrl-F</td>
        <td class="shortcut-cmd-id">edit.find</td>
        <td class="shortcut-cmd-name">Find</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">F</td>
        <td class="shortcut-binding">Ctrl-Shift-F</td>
        <td class="shortcut-cmd-id">edit.findInFiles</td>
        <td class="shortcut-cmd-name">Find in Files</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">F</td>
        <td class="shortcut-binding">Shift-Ctrl-F</td>
        <td class="shortcut-cmd-id">replace</td>
        <td class="shortcut-cmd-name">replace</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">G</td>
        <td class="shortcut-binding">Ctrl-G</td>
        <td class="shortcut-cmd-id">navigate.gotoLine</td>
        <td class="shortcut-cmd-name">Go to Line</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">G</td>
        <td class="shortcut-binding">Ctrl-Alt-G</td>
        <td class="shortcut-cmd-id">GitHubAccess.init</td>
        <td class="shortcut-cmd-name">Initialize GitHubAccess</td>
        <td class="shortcut-orig">Extension (GitHubAccess)</td>
    </tr>
    <tr>
        <td class="shortcut-base">G</td>
        <td class="shortcut-binding">Shift-Ctrl-G</td>
        <td class="shortcut-cmd-id">findPrev</td>
        <td class="shortcut-cmd-name">findPrev</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">H</td>
        <td class="shortcut-binding">Ctrl-H</td>
        <td class="shortcut-cmd-id">edit.replace</td>
        <td class="shortcut-cmd-name">Replace</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">H</td>
        <td class="shortcut-binding">Ctrl-Shift-H</td>
        <td class="shortcut-cmd-id">view.hideSidebar</td>
        <td class="shortcut-cmd-name">Hide Sidebar</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">L</td>
        <td class="shortcut-binding">Ctrl-L</td>
        <td class="shortcut-cmd-id">convert_lowercase</td>
        <td class="shortcut-cmd-name">To Lower Case</td>
        <td class="shortcut-orig">Extension</td>
    </tr>
    <tr>
        <td class="shortcut-base">L</td>
        <td class="shortcut-binding">Ctrl-Alt-L</td>
        <td class="shortcut-cmd-id">me.drewh.jsbeautify</td>
        <td class="shortcut-cmd-name">Beautify Document</td>
        <td class="shortcut-orig">Extension(Beautify)</td>
    </tr>
    <tr>
        <td class="shortcut-base">M</td>
        <td class="shortcut-binding">Ctrl-M</td>
        <td class="shortcut-cmd-id">minifier.min</td>
        <td class="shortcut-cmd-name">Minify Code</td>
        <td class="shortcut-orig">Extension(Minifier)</td>
    </tr>
    <tr>
        <td class="shortcut-base">N</td>
        <td class="shortcut-binding">Ctrl-N</td>
        <td class="shortcut-cmd-id">file.new</td>
        <td class="shortcut-cmd-name">New File</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">O</td>
        <td class="shortcut-binding">Alt-O</td>
        <td class="shortcut-cmd-id">openFileFromUrl.fullEditor</td>
        <td class="shortcut-cmd-name">Edit File</td>
        <td class="shortcut-orig">Extension</td>
    </tr>
    <tr>
        <td class="shortcut-base">O</td>
        <td class="shortcut-binding">Ctrl-O</td>
        <td class="shortcut-cmd-id">file.open</td>
        <td class="shortcut-cmd-name">Open…</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
    <tr>
        <td class="shortcut-base">O</td>
        <td class="shortcut-binding">Ctrl-Alt-O</td>
        <td class="shortcut-cmd-id">openfolder.containing</td>
        <td class="shortcut-cmd-name">Open Containing Folder</td>
        <td class="shortcut-orig">Extension</td>
    </tr>
        <td class="shortcut-base">O</td>
        <td class="shortcut-binding">Ctrl-Shift-O</td>
        <td class="shortcut-cmd-id">navigate.quickOpen</td>
        <td class="shortcut-cmd-name">Quick Open</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">P</td>
        <td class="shortcut-binding">Ctrl-Alt-P</td>
        <td class="shortcut-cmd-id">file.liveFilePreview</td>
        <td class="shortcut-cmd-name">Live Preview</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">Q</td>
        <td class="shortcut-binding">Ctrl-Q</td>
        <td class="shortcut-cmd-id">file.quit</td>
        <td class="shortcut-cmd-name">Quit</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">R</td>
        <td class="shortcut-binding">Ctrl-Shift-R</td>
        <td class="shortcut-cmd-id">kehrig.ReloadInBrowser.reload</td>
        <td class="shortcut-cmd-name">Reload in browser</td>
        <td class="shortcut-orig">Extension (Reload In Browser)</td>
    </tr>
    <tr>
        <td class="shortcut-base">R</td>
        <td class="shortcut-binding">Shift-Ctrl-R</td>
        <td class="shortcut-cmd-id">replaceAll</td>
        <td class="shortcut-cmd-name">replaceAll</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">S</td>
        <td class="shortcut-binding">Ctrl-Alt-S</td>
        <td class="shortcut-cmd-id">file.saveAll</td>
        <td class="shortcut-cmd-name">Save All</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">S</td>
        <td class="shortcut-binding">Ctrl-S</td>
        <td class="shortcut-cmd-id">file.save</td>
        <td class="shortcut-cmd-name">Save</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">S</td>
        <td class="shortcut-binding">Ctrl-Shift-S</td>
        <td class="shortcut-cmd-id">snippets.hideSnippets</td>
        <td class="shortcut-cmd-name">Show Snippets</td>
        <td class="shortcut-orig">Extension</td>
    </tr>
    <tr>
        <td class="shortcut-base">T</td>
        <td class="shortcut-binding">Ctrl-T</td>
        <td class="shortcut-cmd-id">navigate.gotoDefinition</td>
        <td class="shortcut-cmd-name">Go to Definition</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">U</td>
        <td class="shortcut-binding">Ctrl-U</td>
        <td class="shortcut-cmd-id">convert_uppercase</td>
        <td class="shortcut-cmd-name">To Upper Case</td>
        <td class="shortcut-orig">Extension</td>
    </tr>
    <tr>
        <td class="shortcut-base">V</td>
        <td class="shortcut-binding">Ctrl-Alt-V</td>
        <td class="shortcut-cmd-id">snippets.execute</td>
        <td class="shortcut-cmd-name">Run Snippet</td>
        <td class="shortcut-orig">Extension</td>
    </tr>
    <tr>
        <td class="shortcut-base">W</td>
        <td class="shortcut-binding">Ctrl-Shift-W</td>
        <td class="shortcut-cmd-id">denniskehrig.ShowIndentation.toggle</td>
        <td class="shortcut-cmd-name">Show Indentations</td>
        <td class="shortcut-orig">Extension (Show Indentation)</td>
    </tr>
    <tr>
        <td class="shortcut-base">W</td>
        <td class="shortcut-binding">Ctrl-W</td>
        <td class="shortcut-cmd-id">file.close</td>
        <td class="shortcut-cmd-name">Close</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">Y</td>
        <td class="shortcut-binding">Ctrl-Y</td>
        <td class="shortcut-cmd-id">redo</td>
        <td class="shortcut-cmd-name">redo</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">Z</td>
        <td class="shortcut-binding">Ctrl-Z</td>
        <td class="shortcut-cmd-id">undo</td>
        <td class="shortcut-cmd-name">undo</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">Z</td>
        <td class="shortcut-binding">Shift-Ctrl-Z</td>
        <td class="shortcut-cmd-id">redo</td>
        <td class="shortcut-cmd-name">redo</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">[</td>
        <td class="shortcut-binding">Ctrl-[</td>
        <td class="shortcut-cmd-id">edit.unindent</td>
        <td class="shortcut-cmd-name">Unindent</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">[</td>
        <td class="shortcut-binding">Ctrl-]</td>
        <td class="shortcut-cmd-id">edit.indent</td>
        <td class="shortcut-cmd-name">Indent</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">[</td>
        <td class="shortcut-binding">Ctrl-[</td>
        <td class="shortcut-cmd-id">pflynn.goWorkingSetPrev</td>
        <td class="shortcut-cmd-name">Previous Document in List.</td>
        <td class="shortcut-orig">Extension</td>
    </tr>
    <tr>
        <td class="shortcut-base">]</td>
        <td class="shortcut-binding">Ctrl-]</td>
        <td class="shortcut-cmd-id">pflynn.goWorkingSetNext</td>
        <td class="shortcut-cmd-name">Next Document in List.</td>
        <td class="shortcut-orig">Extension</td>
    </tr>
    <tr>
        <td class="shortcut-base">Backspace</td>
        <td class="shortcut-binding">Ctrl-Backspace</td>
        <td class="shortcut-cmd-id">delWordLeft</td>
        <td class="shortcut-cmd-name">delWordLeft</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">Backspace</td>
        <td class="shortcut-binding">Cmd-Backspace (Mac only)</td>
        <td class="shortcut-cmd-id">probertson.deleteToLineStart</td>
        <td class="shortcut-cmd-name">Delete To Line Start</td>
        <td class="shortcut-orig">Extension (<a href="https://github.com/probertson/brackets-delete-line-start-end">Delete to line start/end</a>)</td>
    </tr>
    <tr>
        <td class="shortcut-base">Backspace</td>
        <td class="shortcut-binding">Alt-Backspace (Win only)</td>
        <td class="shortcut-cmd-id">probertson.deleteToLineStart</td>
        <td class="shortcut-cmd-name">Delete To Line Start</td>
        <td class="shortcut-orig">Extension (<a href="https://github.com/probertson/brackets-delete-line-start-end">Delete to line start/end</a>)</td>
    </tr>
    <tr>
        <td class="shortcut-base">Delete</td>
        <td class="shortcut-binding">Ctrl-Delete</td>
        <td class="shortcut-cmd-id">delWordRight</td>
        <td class="shortcut-cmd-name">delWordRight</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">Delete</td>
        <td class="shortcut-binding">Cmd-Delete (Mac only)</td>
        <td class="shortcut-cmd-id">probertson.deleteToLineEnd</td>
        <td class="shortcut-cmd-name">Delete To Line End</td>
        <td class="shortcut-orig">Extension (<a href="https://github.com/probertson/brackets-delete-line-start-end">Delete to line start/end</a>)</td>
    </tr>
    <tr>
        <td class="shortcut-base">Delete</td>
        <td class="shortcut-binding">Alt-Delete (Win only)</td>
        <td class="shortcut-cmd-id">probertson.deleteToLineEnd</td>
        <td class="shortcut-cmd-name">Delete To Line End</td>
        <td class="shortcut-orig">Extension (<a href="https://github.com/probertson/brackets-delete-line-start-end">Delete to line start/end</a>)</td>
    </tr>
    <tr>
        <td class="shortcut-base">Down</td>
        <td class="shortcut-binding">Alt-Down</td>
        <td class="shortcut-cmd-id">navigate.nextMatch</td>
        <td class="shortcut-cmd-name">Next Match</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">Down</td>
        <td class="shortcut-binding">Alt-Shift-Down</td>
        <td class="shortcut-cmd-id">pflynn.everyscrub.nudge_down</td>
        <td class="shortcut-cmd-name">Decrement Number</td>
        <td class="shortcut-orig">Extension (Everyscrub)</td>
    </tr>
    <tr>
        <td class="shortcut-base">Down</td>
        <td class="shortcut-binding">Ctrl-Down</td>
        <td class="shortcut-cmd-id">goDocEnd</td>
        <td class="shortcut-cmd-name">goDocEnd</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">Down</td>
        <td class="shortcut-binding">Ctrl-Shift-Down</td>
        <td class="shortcut-cmd-id">edit.lineDown</td>
        <td class="shortcut-cmd-name">Move Line(s) Down</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">End</td>
        <td class="shortcut-binding">Ctrl-End</td>
        <td class="shortcut-cmd-id">goDocEnd</td>
        <td class="shortcut-cmd-name">goDocEnd</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">Enter</td>
        <td class="shortcut-binding">Ctrl-Enter</td>
        <td class="shortcut-cmd-id">edit.openLineBelow</td>
        <td class="shortcut-cmd-name">Open Line Below</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">Enter</td>
        <td class="shortcut-binding">Ctrl-Shift-Enter</td>
        <td class="shortcut-cmd-id">edit.openLineAbove</td>
        <td class="shortcut-cmd-name">Open Line Above</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">F2</td>
        <td class="shortcut-binding">F2</td>
        <td class="shortcut-cmd-id">file.rename</td>
        <td class="shortcut-cmd-name">Rename</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">F3</td>
        <td class="shortcut-binding">F3</td>
        <td class="shortcut-cmd-id">edit.findNext</td>
        <td class="shortcut-cmd-name">Find Next</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">F3</td>
        <td class="shortcut-binding">Shift-F3</td>
        <td class="shortcut-cmd-id">edit.findPrevious</td>
        <td class="shortcut-cmd-name">Find Previous</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">F4</td>
        <td class="shortcut-binding">Cmd-F4</td>
        <td class="shortcut-cmd-id">toshsharma.bookmarks.toggleBookmark</td>
        <td class="shortcut-cmd-name">Toggle Bookmark</td>
        <td class="shortcut-orig">Extension (Bookmarks)</td>
    </tr>
    <tr>
        <td class="shortcut-base">F4</td>
        <td class="shortcut-binding">F4</td>
        <td class="shortcut-cmd-id">toshsharma.bookmarks.nextBookmark</td>
        <td class="shortcut-cmd-name">Next Bookmark</td>
        <td class="shortcut-orig">Extension (Bookmarks)</td>
    </tr>
    <tr>
        <td class="shortcut-base">F4</td>
        <td class="shortcut-binding">Shift-Cmd-F4</td>
        <td class="shortcut-cmd-id">toshsharma.bookmarks.clearBookmarks</td>
        <td class="shortcut-cmd-name">Clear Bookmarks</td>
        <td class="shortcut-orig">Extension (Bookmarks)</td>
    </tr>
    <tr>
        <td class="shortcut-base">F4</td>
        <td class="shortcut-binding">Shift-F4</td>
        <td class="shortcut-cmd-id">toshsharma.bookmarks.previousBookmark</td>
        <td class="shortcut-cmd-name">Previous Bookmark</td>
        <td class="shortcut-orig">Extension (Bookmarks)</td>
    </tr>
    <tr>
        <td class="shortcut-base">F5</td>
        <td class="shortcut-binding">F5</td>
        <td class="shortcut-cmd-id">debug.refreshWindow</td>
        <td class="shortcut-cmd-name">Reload Brackets</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">F6</td>
        <td class="shortcut-binding">F6</td>
        <td class="shortcut-cmd-id">project.gruntDefault</td>
        <td class="shortcut-cmd-name">Grunt default</td>
        <td class="shortcut-orig">Extension (brackets-grunt)</td>
    </tr>
    <tr>
        <td class="shortcut-base">F12</td>
        <td class="shortcut-binding">F12</td>
        <td class="shortcut-cmd-id">debug.showDeveloperTools</td>
        <td class="shortcut-cmd-name">Show Developer Tools</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">Home</td>
        <td class="shortcut-binding">Ctrl-Home</td>
        <td class="shortcut-cmd-id">goDocStart</td>
        <td class="shortcut-cmd-name">goDocStart</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">Left</td>
        <td class="shortcut-binding">Alt-Left</td>
        <td class="shortcut-cmd-id">goLineStart</td>
        <td class="shortcut-cmd-name">goLineStart</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">Left</td>
        <td class="shortcut-binding">Ctrl-Left</td>
        <td class="shortcut-cmd-id">goWordLeft</td>
        <td class="shortcut-cmd-name">goWordLeft</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">Right</td>
        <td class="shortcut-binding">Alt-Right</td>
        <td class="shortcut-cmd-id">goLineEnd</td>
        <td class="shortcut-cmd-name">goLineEnd</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">Right</td>
        <td class="shortcut-binding">Ctrl-Right</td>
        <td class="shortcut-cmd-id">goWordRight</td>
        <td class="shortcut-cmd-name">goWordRight</td>
        <td class="shortcut-orig">CodeMirror</td>
    </tr>
    <tr>
        <td class="shortcut-base">Tab</td>
        <td class="shortcut-binding">Ctrl-Shift-Tab</td>
        <td class="shortcut-cmd-id">navigate.prevDoc</td>
        <td class="shortcut-cmd-name">Previous Document</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">Tab</td>
        <td class="shortcut-binding">Ctrl-Tab</td>
        <td class="shortcut-cmd-id">navigate.nextDoc</td>
        <td class="shortcut-cmd-name">Next Document</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">Up</td>
        <td class="shortcut-binding">Alt-Shift-Up</td>
        <td class="shortcut-cmd-id">pflynn.everyscrub.nudge_up</td>
        <td class="shortcut-cmd-name">Increment Number</td>
        <td class="shortcut-orig">Extension (Everyscrub)</td>
    </tr>
    <tr>
        <td class="shortcut-base">Up</td>
        <td class="shortcut-binding">Alt-Up</td>
        <td class="shortcut-cmd-id">navigate.previousMatch</td>
        <td class="shortcut-cmd-name">Previous Match</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">Up</td>
        <td class="shortcut-binding">Ctrl-Shift-Up</td>
        <td class="shortcut-cmd-id">edit.lineUp</td>
        <td class="shortcut-cmd-name">Move Line(s) Up</td>
        <td class="shortcut-orig">Brackets</td>
    </tr>
    <tr>
        <td class="shortcut-base">J</td>
        <td class="shortcut-binding">Ctrl-Shift-J</td>
        <td class="shortcut-cmd-id">katsh.JSONLint</td>
        <td class="shortcut-cmd-name">Parse json file or hilighted string</td>
        <td class="shortcut-orig">Extension (JSONLint)</td>
    </tr>
</table>