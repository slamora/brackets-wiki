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

**[nj]** I think this is right in general, but I have a slightly different way of thinking about this. OT is trying to solve the problem of reconciling edits to the same data structure. In our case, we have two different data structures: on the Brackets side, we have a stream of text, and on the browser side, we have a hierarchical DOM (we're not pushing textual updates directly to the browser). The biggest issue we face is not reconciling two streams of DOM manipulations, but in figuring out how to convert textual updates into DOM manipulations. Once we've figured out what DOM manipulations a given set of textual edits in the code correspond to, then we could use OT to reconcile those with changes in the browser. (I think this might be basically the same thing you're saying, but your comment two paragraphs above about events vs. diff patches confused me; in my mind, OT would happen *after* the point at which we would convert events or diffs to a DOM manipulation stream, so which one we choose has no impact on the input to OT.)

## Notes on Diffs

I did some looking into the prior art of HTML/XML diffing. I found an [XML diff survey](http://www.scribd.com/doc/14482474/XML-diff-survey) paper that walked through the issues and algorithms well. Reading this also crystallized in my mind what is different between the problem that we face and the problem that all of these others have faced.

Existing XML diff solutions are designed to take two static files and compare them, trying to find a small difference between them. This makes it really hard to find a node that has been reparented, for example.

What we have is, potentially, a snapshot of the last state pushed to the browser and markers all over the place for the tags in the document. CodeMirror goes to some amount of effort to ensure that those markers keep referring to the same blocks of code. It's possible that some of the prior art in diffing (computing edit distances, perhaps?) may apply, but much of the problem is different because of the data that we're maintaining. This is a good thing.

## Event-based incremental reparse/diff

As an optimization, we plan to use individual change events from CodeMirror in order to avoid having to do a full reparse/diff on every keystroke. Rather than trying to separately determine the live edits that should correspond to each individual change event, we simply use the location of the change as a hint to the parser/differ to determine a small affected range that needs to be reparsed/diffed. That avoids creating multiple code paths for interpreting changes--all the actual parsing/diffing logic is centralized.

In the prototype implementation, we use the existing marked ranges generated by the instrumentation to determine the immediate parent tag of the change, and assume that entire parent tag's subtree needs to be reparsed/diffed. This could be further optimized to make it so that changes within a start tag only cause a reparse/diff of the tag itself and not its children, or that changes within a text node only cause the text node to be reparsed.

If the start and end of the change are in different marked ranges, the prototype punts and forces a complete reparse/diff of the document. This could be optimized to find the common ancestor tag of the start and end of the change range.

Also, the prototype implementation is limited in that it can only deal with operations for which there's a single change record. If there are multiple change records in an operation, it becomes difficult to determine what the affected ranges are, because we don't have access to the intermediate state of the marks between the change records. For now, it seems fine to just punt and do a complete reparse/diff, since in most cases a multiple change record suggests a large, disruptive edit (like a Replace All) anyway. We can revisit this if there turn out to be common operations with multiple change records.

(A small optimization we could do even in that case is to only reparse the portion of the document after the earliest change location, with the parser stack pre-seeded with the tags that are open at that point. However, in the worst case of an edit near the beginning of the document, that won't help much.)

## Well-formed HTML

In the context of this feature, "well-formed HTML" means markup and content that can be pushed to the browser and expect it to render in a reasonable way. Modern browsers are _very_ forgiving in the markup they render. They ignore what the don't understand and subsequent markup and content is generally not affected.

Well-formed markup just needs to satisfy generic tag format:

* &lt;tag attr="val" attr2&gt;
* &lt;tag attr="val"/&gt;
* &lt;/tag&gt;

It's not critical to require ending tags (i.e. &lt;/tag&gt;) as browsers do a good job of auto-inserting them. Also, Brackets auto-inserts these, anyway, so it's not an issue unless user turns this off.

It's not important to perform "W3C Validation" because it's too strict for many users (and also a moving target).

It's also not important to validate that tag and attribute, names and context are valid. Users want to see results in browser for visual feedback -- not wait for perfect markup.

Here's our current plan:

* On each edit, run the simple DOM builder over the affected range (which is calculated based on where the edit is, as described below in "Event-based incremental update").
* If the DOM builder can parse the document, then store off the current state of the DOM for future comparisons, and do an incremental reparse/diff based on the affected range.
* If the DOM builder finds invalid tag nesting or other issues, abort the parse and set a flag that indicates we're currently in an invalid state. As long as we're in an invalid state, don't do live updates.
* Continue to run the DOM builder on each edit until it succeeds. At that point, do a full re-diff of the entire document to determine what's changed since the last time we were valid, and send live updates accordingly.

This is currently implemented in the prototype. It works well for most cases, but doesn't yet detect some cases--in particular, the case where there's a `<` before text with no matching `>`. It's likely that we'll need to augment the tokenizer to detect some of these cases, and have it send errors up to the parser. I'm confident that it would be straightforward to add detection as necessary.

## Pushing changes to the browser

Recommendations: Use (1) a custom build of jQuery limited to DOM manipulation or (2) write our own DOM manipulation APIs. 

This [pull request](https://github.com/adobe/brackets/pull/4237) loads the full jQuery library (into a private namespace) in the inspected page via ``RemoteAgent``. We use this method to support CSS rule and HTML element highlighting features in Live Preview.

The prototype exposes a subset of jQuery DOM manipulation methods to support the text editing use cases we've outlined above for basic CRUD operations on a DOM tree (attr, removeAttr, before, after, append, prepend, text, detach, remove).

**[nj]** It turns out that some edits are not straightforward to do with just the basic jQuery functions. In particular, jQuery doesn't make it easy to select text nodes; you have to get the `contents()` of the parent to even see them (`children()` doesn't include text nodes), so I ended up falling back on standard DOM APIs anyway. My suggestion is that we get rid of jQuery and just use the standard DOM APIs on the browser side. Also, we can tune the edit descriptors generated by the differ to whatever makes it easiest for us to implement on the browser side.

# Older content

## Comments on valid/invalid states

**[nj]** Some comments:

* In general I think this is right, but I think we have to do some work in order to anticipate how browsers will auto-insert end tags, because we want to accurately reflect the structure the browser would have created if it were to simply load the given HTML.

* Given that, though, it sounds like our starting proposal is that we'll consider the HTML to be well-formed as long as there are no unclosed start or end tags (i.e. as long as the user is in the middle of typing `<tag attr...`, we'll consider it ill-formed, but as soon as they type the `>`, we'll make our best guess as to where the tag closure should be, and manipulate the DOM accordingly); and also we'll consider a start tag invalid if the attribute syntax in it is messed up enough (e.g. `<tag foo=>`). It seems like that's a simple enough heuristic that we could start with it and see how it goes.
**[randy]** Chrome is ridiculously forgiving. The only really bad case (where tag is not rendered at all) I could find is unmatched quotes in an attribute value. A lone "<" renders as text -- kinda dumb, but doesn't affect subsequent tags. Even "<li" is recognized as a list item! But, since we have to handle some cases, we might as well wait for the basic well-formed case before pushing to browser.

* Do we know what the existing code we're using to parse HTML for highlighting (in DOMHelpers.js) does if it encounters an unclosed start or end tag (in the sense of a `<` without a `>`)?

**[randy]** The DOMHelpers.js code will ignore tag started with `<` if there is not a subsequent `>`. But, it searches entire file looking for `>` (i.e. does not detect any other `<` chars while searching. The DOMHelpers.js attribute parsing code needs to be improved to detect invalid attributes -- currently, it ignores anything that does not fit `attr=val` format. It also does not handle valid attribute case where there is optional whitespace around `=`.

## Issues with Inspector DOM API

A quick review of the [Inspector DOM API docs](https://developers.google.com/chrome-developer-tools/docs/protocol/tot/dom) exposes a major weakness: a lack of a node insertion API.

Other notes:

* ``DOM.getDocument()`` only returns a node tree to the depth of ``body``
* Same node is never sent twice, i.e. sequential calls to ``DOM.getDocument()`` will return the same tree but with different ``nodeId`` values
* @jdiehl's prototype work (see ``DOMAgent/DOMNode/DOMHelpers``) maintains a DOM tree representation by listening to a complete set of DOM mutation events. 