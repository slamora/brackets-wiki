# HTML DOM Data Structure Research #

Status: Research Starting (Sprint 27)

This page will collect the results of the [HTML DOM Data Structure research story](https://trello.com/card/5-research-data-structure-for-html-dom-edit-mapping/4f90a6d98f77505d7940ce88/844).

## Test Scenarios ##

Each scenario is numbered for ease of reference, but the order makes no difference.

These scenarios use the [Getting Started sample file](https://github.com/adobe/brackets/blob/f482326997f4b6e09a2640c770dbd915c81851a3/samples/root/Getting%20Started/index.html). That link points to a specific SHA of the file to ensure that these scenarios keep working even if the file changes.

Each scenario starts with a fresh copy of the file, though many could work as a straight run-through.

### 1: Text node manipulation

* Insert text at the beginning of a text node
    * Line 13: insert "YOU SHOULD BE " immediately after the `<h1>` open tag
* Alter text in the middle of a text node
    * Line 13: Replace the text "GETTING STARTED WITH" with "ALREADY USING"

That was easy, wasn't it?

### 2: Attributes

* Add an attribute
    * Line 13: after "h1", type ' class="main"'.
* Change the contents of an attribute
    * Line 61: Change "showing" to "displaying"
* Delete an attribute
    * Line 61: Delete the `alt` attribute entirely
    * Use backspace starting from the "s" in `src`
* Alternative: do the same case, but select the entire attribute first and then hit delete. (Should be easier, but needs to be tested.)

Still kind of easy, but there are some invalid states while editing.

### 3: Nesting/Across Lines

* Adding a tag
    * Line 75: Surround `"save/reload dance"` with a `<span class="save-reload">` tag
    * Start with the caret before the opening "
    * Type in the new tag
    * Navigate to just after the closing "
    * Type `</span>`
* Deleting a tag (but not its text)
    * Line 60: Delete the surrounding `<a>` tag
    * Start with the caret at the end of the opening `<a>` tag
    * Delete it using backspace
    * Navigate to just after the matching `</a>` tag on line 62
    * Delete it using backspace
    * Ideally, the `<img>` tag would be reparented instead of being destroyed/recreated.
* Alternative: Do the previous case, but delete the closing tag first.
* Sloppily adding a tag
    * Line 74: Add `id` and surround paragraph with `<span>`
    * Put the caret after the "p"
    * Type `id="foo"<span>` (note that `p` tag was not closed properly)
    * Navigate to line 84, before the `</p>` tag
    * Type `</span>`
    * Put the caret just before the opening `<span>` on line 74
    * Type `>`
    * Navigate to the end of the line and delete the extra `>`
    * At this point, the paragraph is properly set up

This tests invalid states with nested tags and adding new tags across lines.

### 4: Lists

* Add a new list item
    * Line 141: Adding a new item after this one
    * Navigate to the end of the line
    * Hit enter to create a new line at 142
    * Type `<li>Sometimes read about us on `
    * Note that the state is valid as soon as the `>` is typed
    * Type `<a href="http://news.ycombinator.com">Hacker News</a></li>`

### 5: Tables

* Adding a table
    * Go to line 150
    * Type:

```html
<h3>Ratio of Brackets Goodness</h3>
<table>
    <thead>
        <tr><th>Item</th><th>Brackets Superiority Ratio</th></tr>
    </thead>
    <tbody>
        <tr><td>Apple Pie</td><td>27</td></tr>
        <tr><td>"edlin" editor</td><td>2,567</td></tr>
        <tr><td>Bacon</td><td>2</td></tr>
    </tbody>
</table>
```

### 6: Copy/paste

* Cut/paste, same parent
    * Select the `<li>` on line 142
    * Cut it
    * Move to the beginning of line 141
    * Paste. This should result in the DOM node (and its children) moving without being recreated.
* Cut/paste, different parents
    * Select the `<img>` on line 61
    * Cut it
    * Move to the text at the beginning of line 65 (inside the `<p>` tag)
    * Paste. This should result in the DOM node moving without being recreated.
* **TODO:** For copy/paste, it's harder to know what to do since you're actually creating a new node. Is there any expectation that you'll copy the state of the original node somehow, or should we just treat it as if you retyped the text?

### 7: Changing tags

* Changing end tag first
    * Go to line 24 (closing `p` tag)
    * Delete tag
    * Type `</div>`
    * Go to line 20
    * Backspace over the tag
    * type `<div>`
* Change open tag first
    * Go to line 14
    * Replace the "2" in `h2` with "3" in the opening tag
    * Go to the ending tag
    * Replace the "2" in `/h2` with a "3"

### 8: Big file editing

The [W3C Packaged Web Apps Spec](http://www.w3.org/TR/2012/REC-widgets-20121127/) is a reasonably large document (344KB, 4400+ lines) with a good deal of structure to it. Once the basic tests are in place, performing similar tests against this large document will help find performance concerns.

**Note**: I just tried loading this document into Brackets and starting Live Development on it. Brackets froze for probably a minute (with Brackets Helper taking 100% of CPU). Eventually, Brackets became responsive again, but the highlighting wasn't working.

### 9: Commenting/uncommenting

* Commenting out code
    * At the beginning of line 114, type `<!--`
    * Move the cursor to line 121 and type `-->`
    * Ideally, the DOM nodes after line 121 would reappear with all their original state. (That sounds hard, but if we use the marked range info in the document, perhaps we could be try to be smart about this case and cache the DOM nodes that were removed after the first step, then reinsert them after the second step.)
* Uncommenting code
    * After doing the previous case, delete the `<!--` from line 114, then delete the `-->` from line 121.
    * Ideally, the original DOM node for the `<p>` would reappear (but at the very least, we should create a new `<p>`).

### 10: Undo/Redo ###

* Delete + undo
    * Go to line 20
    * Select to the closing paragraph tag on line 24
    * Delete
    * Undo
    * Note: it should actually be possible to to keep that DOM node across the delete/undo, but it is probably not worthwhile to invest the effort

## Operational Transforms vs. Simple Events ##

Wikipedia offers basic information about [Operational Transformation (OT)](http://en.wikipedia.org/wiki/Operational_transformation).

Sending events from the editor to the previewing browser should be reliable. However, my concern is that we could have changes to the DOM on the browser side as well. Ultimately, it *could* look like a collaboration between the browser-side code and the editor. As an added bonus, if we ever did implement actual real time collaboration, we would be able to reuse this work in that context.

I spent a little time looking at the [ShareJS code](https://github.com/share/ShareJS). From a quick survey of the code, it looks like their implementation of OT wraps a straightforward event stream with a bit of extra bookkeeping on each side to ensure that operations are applied successfully. They even have a CodeMirror adapter which uses CM events that are mildly adapted to ShareJS.

If we start with a simple event model, it seems like it should be possible to later migrate to OT. Diff/Patch would basically work with OT as well (because a Patch would be an operation).

One final note: for dealing with synchronization with the browser DOM, it would be possible to use [MutationObservers](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver). These appear to be available in Chrome today. I would imagine that listening to the whole document for mutations is likely expensive, but it may be okay for our use (authoring work).