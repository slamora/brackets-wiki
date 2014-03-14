In Sprint 38, Brackets will migrate to CodeMirror v4, which primarily adds support for multiple selections. This document briefly describes what extension authors should be aware of when updating extensions to work with Sprint 38.

In general, most extensions should continue to work as-is, but extensions that have not been upgraded to work with the new multiple selection APIs will only operate on the "primary" selection (the last selection made by the user) if multiple selections are active. Also, other changes might affect the functioning of your extension.

Here are the main changes to be aware of:

* **CodeMirror is no longer global.** In Brackets, we used to provide CodeMirror as a global. CodeMirror now supports `require()`, so we now include CodeMirror as a module instead. This means you should add `var CodeMirror = brackets.getModule("thirdparty/CodeMirror2/lib/CodeMirror");` at the top of any module that accesses the CodeMirror object. Sprint 38 will still provide the global CodeMirror (which is the same as the module-loaded CodeMirror), but access to it is deprecated and it will be removed in a future release. (Note that this doesn't affect accesses to `editor._codeMirror`, which should work as before.)
* **General CodeMirror v4 changes.** In general, aside from multiple selections, CodeMirror's APIs haven't changed much. However, you should read the [CodeMirror v4 migration guide](http://codemirror.net/4/doc/upgrade_v4.html) to see if any of the changes might affect you. In general, if you're just calling Brackets APIs on Editor or Document, rather than calling CodeMirror functions directly (e.g. on `editor._codeMirror`), these shouldn't affect you.
* **Edit merging.** If you're using the `origin` property of `Document.replaceRange()` in order to merge edits that are nearby in time, and you're also manually setting the selection after each edit, you'll need to group each edit together with the new selection inside a `Document.batchOperation()` in order for the edits to merge together. Otherwise the selection change will force the edits not to merge.
* **Multiple selections.** If your extension looks at or performs edits on the user's current selection, you will want to consider making your edits work with multiple selections by using the new selection APIs on Editor. For more information on this, see [Changes to Editor selection APIs](#editor-APIs) and [Performing edits on multiple selections](#multiple-selection-edits) below.
* **New keyboard shortcuts.** A few new keyboard shortcuts have been added to CodeMirror and Brackets in order to provide functionality related to multiple selections. If you use any of these shortcuts in your extension, you should consider changing them to something that doesn't conflict.

| Shortcut | Function |
| -------- | -------- |
| Alt-Click-Drag | Create a vertical or rectangular selection |
| Ctrl/Cmd-Click / Ctrl/Cmd-Click-Drag | Add a cursor or range to the selection |
| Shift-Alt-Up/Down | Add Line to Selection - adds a cursor on the previous/next line |
| Ctrl/Cmd-Alt-L | Split Selection into Lines - takes a contiguous selection and converts it to a multiple selection, one range for each line |
| Ctrl/Cmd-B | Add Next Match to Selection - adds the next match for the current range or word to the selection |
| Ctrl/Cmd-Shift-B | Skip and Add Next Match - removes the current range from the selection and adds the next match |
| Alt-F3 / Cmd-Ctrl-G | Find All and Select - adds all matches for the current range or word to the selection |
| Ctrl/Cmd-U | Undo Selection - undoes the last selection change or edit |
| Ctrl/Cmd-Shift-U | Redo Selection - redoes the last selection change or edit |
| Esc | Switch to Single Selection |

If you make any changes that require Sprint 38 or CodeMirror v4 (e.g. using the new selection APIs in Editor or using require to load CodeMirror), make sure to set Sprint 38 as the minimum Brackets version for your updated extension by setting `"engines": { "brackets" : ">=0.38.0" }` to your extension's `package.json`. Users of older Brackets versions will still be able to install older versions of your extension.

For a more detailed spec on the new multiple selection functionality, see [Research: Multiple cursors and selections](https://github.com/adobe/brackets/wiki/Research:-Multiple-cursors-and-selections).

### Changes to Editor selection APIs

The existing single-selection APIs in Editor (`getSelection()`, `getCursorPos()`, `setSelection()`, and so on) are still available, but if a multiple selection is active, they only return information about the **primary** selection - that is, the last selection the user added to the current multiple selection. To access the current multiple selection, use `getSelections()`, which returns an array of selection objects, each of which contains these fields:

* `start`: a `{line, ch}` object for the beginning of the selected range
* `end`: a `{line, ch}` object for the end of the selected range - will be equal to `start` if it's a cursor
* `reversed`: true if the head of the range is `start`, false if the head is `end`. (The "head" is the end of the range that would move if the user extends the range via the keyboard or mouse. Most edit operations probably won't care too much about this.)
* `primary`: whether this is the primary selection; exactly one selection will have this set to `true`.

The selections are guaranteed to be sorted in document order and to be non-overlapping.

(Note that this format is different from the native CodeMirror format, which uses `anchor` and `head`. In most cases, edit ops probably care more about the actual ranges involved and want them to be in start/end order, and don't really care which end of a given range is the anchor and which is the head, so we thought it better to optimize for that case. You can still figure out which end is the head by looking at the `reversed` flag.)

`setSelections()` takes exactly the same format that `getSelections()` returns. However, you can omit the `primary` flag; if you leave it off, the last selection in the array will be the primary selection.

Another enhancement to selections is the ability to undo/redo selections using Ctrl/Cmd-U and Ctrl/Cmd-Shift-U. This is handy even without multiple selections; it provides an easy way to move backwards through your selection history. Like edits, you can specify that nearby selections of a similar type should be merged by specifying an `origin`. As mentioned above, however, selection and edit origins are separate; if you want to merge together an edit and an associated selection, you should wrap them in an operation.

For more detailed information on the changes to the selection APIs on Editor, see the table in [Research: Multiple cursors and selections](https://github.com/adobe/brackets/wiki/Research:-Multiple-cursors-and-selections#proposed-editor-api-changes).

### Performing edits on multiple selections

(Once the multiple selection functionality is released in Brackets, this will move to the [Brackets Development How-Tos](https://github.com/adobe/brackets/wiki/Brackets-Development-How-Tos) page.)

In most cases, your edits will be based on the user's current selection. As of Sprint 38, Brackets supports multiple selections, so in most cases you will want to switch to use the new `getSelections()` and `setSelections()` APIs, and determine how your edits will operate on multiple selections.

In the simplest case, where your edits operate on the range of each selection and they're performed in such a way that the selection is properly fixed up automatically by CodeMirror, you can just iterate over all selections and edit each one. However, you must be careful to **re-get the list of selections** after each individual edit, because later selections may shift as a result of your edit. So, your code should look like this:

```javascript
editor.document.batchOperation(function () {
    var sels = editor.getSelections(), i, sel;
    for (i = 0; i < sels.length; i++) {
        sel = sels[i];
        // ... do various edits with editor.document.replaceRange() ...
        sels = editor.getSelections(); // refresh all selections that might have changed
    }
});
```

If you additionally need to adjust the selections that should result from the edit, you can build up a list of ranges to select at the end:

```javascript
editor.document.batchOperation(function () {
    var sels = editor.getSelections(),
        ranges = [],
        i, sel, range;
    for (i = 0; i < sels.length; i++) {
        sel = sels[i];
        // ... do various edits with editor.document.replaceRange() ...
        // ... set range to the selection range that should be set after this edit ...
        ranges.push(range);
        sels = editor.getSelections();
    }
    editor.setSelections(ranges);
});
```

In more complicated cases, you might need to do some more complex processing that isn't strictly one-edit-per-selection. A typical case of this is in line-oriented edits, such as Move Line Up/Down, where there might be multiple selections within a given line, but you only want to process each line once, while still tracking the positions of the various selections that intersect that line. In cases like these, you can use two helper functions:

* `Editor.convertToLineSelections()` takes a multiple selection and converts it to a set of whole-line selections, each of which is paired with the original selection(s) that overlap the range of the whole-line selection.
* `Document.doMultipleEdits()` takes an array of edit descriptors, each of which describes a particular `replaceRange()`-style edit (or set of edits) to do at once, and a set of selections that should be tracked through those edits. The advantage of using this over just directly using `replaceRange()` and fixing up selections yourself is that it allows you to act as if each edit is separate from all the others (so you don't have to worry about fixing up selections for any edit other than the current one), and it handles tracking selections that are related to each edit automatically.

For examples of how to use these APIs, you can look at the line-oriented edits in EditorCommandHandlers, such as `moveLine()` or `openLine()`.