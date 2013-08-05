# HTML DOM Data Structure Research #

Status: Implementation underway (Sprint 29)

# Implementation Notes #

## Diff/Patch ##

There is some complexity around diff/patch that is not accounted for in the original research prototype. We basically threw out comments, for example. Comments, like text nodes, cannot be identified and individually located easily in the browser. However, our purpose here is to allow people to preview their work. Comments are not visual at all, so previewing is not important. What if we just ignore comments – both when generating our SimpleDOM structure or when applying a patch?

Here's an example:

```html
<div data-brackets-id="100">Hi there<!-- silly person -->, good neighbor!</div>
```

That `<div>` contains three nodes: text, comment, text. If "good neighbor" was changed to "charitable fool" the code would have to go through some contortions to reflect that the "good neighbor!" text node was the one that was deleted and "charitable fool" is being inserted.

If we treat the comments as not even worthy of attention, we end up with a diff that looks like this:

```javascript
{
    parentID: 100,
    type: "textReplace",
    content: "Hi there, foul cretin"
}
```

On the browser side, it just replaces *all* of the text nodes inside of that parent div with one text node with the new content. In this case, it doesn't really matter where it goes, because the comment does not display.

How about something a little more complicated?

```html
<div data-brackets-id="100">
    This is <!-- need a good adjective here --> <em data-brackets-id="101">the ultimate</em> doohickey for
    <strong data-brackets-id="102">kibitzing</strong> on the internets.<!-- remove this next bit? It seems... wrong? --><span style="display: none" data-brackets-id="103"> All of them.</span>
</div>
```

An operation like this one is straightforward:

```javascript
{
    parentID: 100,
    beforeID: 101,
    type: "textReplace",
    content: "\n    This was"
}
```

It replaces the "is" with "was". There's also a space character between the end of the comment and the `em` tag. That character will be deleted. Whether the text node ends up before or after the comment is irrelevant.

```javascript
{
    parentID: 100,
    type: "textReplace",
    content: "\n    This was"
}
```

would be illegal in this case, because there are child nodes to consider.

```javascript
{
    parentID: 100,
    afterID: 102,
    beforeID: 103,
    type: "textReplace",
    content: " on the internet."
}
```

This change makes it very clear which text we're talking about because it is completely bounded by the before and after elements. How about two changes coming together that might have been confusing to the prototype implementation?

```javascript
{
    tagID: 101,
    type: "elementDelete"
}
{
    parentID: 100,
    afterID: 101,
    beforeID: 103,
    type: "textReplace",
    content: " doohickey for chatting on the internet."
}
```

We have deleted the `<strong>` tag. The text node siblings that surrounded that tag will get merged together when the textReplace happens (all of the text nodes between 101 and 103 are deleted and the new text is put in its place).

# Initial Research Results #

This page contains the results of the [HTML DOM Data Structure research story](https://trello.com/card/5-research-data-structure-for-html-dom-edit-mapping/4f90a6d98f77505d7940ce88/844).

The fundamental goal of this research was to **figure out a path forward** for HTML live development. We needed to figure out:

* How do we know when to push changes?
* What form do those changes take?
* How do we actually push the changes out to the browser?

We explored two different variations of tracking changes, ultimately finding that the two were linked and also tied directly into knowing when to push changes.

We built a prototype to explore ideas, try things out and reach these conclusions.

## Proposed Stories ##

1. Fix up the prototype
    * More extensive testing
    * Clean up parsing (doesn't handle automatically closed tags, for example)
    * Switch from child number to node-relative positioning everywhere (better for accuracy)
    * Clean up the interaction between Brackets and the Browser (load a library in the browser)
    * Note that there is new third party code (HTMLTokenizer, priority queue and MurmurHash3)
2. Implement moves
    * This reduces churn in the browser
    * Faster
    * Elements will keep their added properties and event handlers
3. Highlight invalid HTML
    * Gives the user an idea of why the page may not be updating
4. Profiling and performance improvements step 1
    * Minimize the impact on typing performance and support "medium sized" files well (20 KB)
5. Profiling and performance improvements step 2
    * Correct the issues with large file editing
6. Possible research into handling browser DOM changes gracefully
    * We may want to see what issues come up in real world usage first.
    * The idea is to automatically detect when a page reload is required to view changes made to the HTML

## The DOM and Diff ##

With CSS live editing, we can just replace all of the CSS in the browser and the browser will correctly apply all of the styles. With HTML, JavaScript may have altered the DOM after the initial HTML was loaded. There can be new properties on DOM objects, event handlers added to elements, new elements and attributes in the DOM, and other changes.

In order to update the HTML while preserving the state in the browser, we want to send just the *differences* between what Brackets initially sent to the browser and the current text in the editor. Quickly computing a small diff is a difficult task. However, we have quite a bit in our favor in approaching this problem:

1. This has been an area of [active research](http://www.scribd.com/doc/14482474/XML-diff-survey) for some time.
2. An algorithm called ["BULD" (for Bottom Up, Lazy Down)](http://www.cs.rutgers.edu/~amelie/papers/2002/diff.pdf), described in a 2002 paper by Cobéna, Abiteboul and Marian, has an efficient solution to a very similar problem to the one we face
3. Using CodeMirror "marks", we can maintain unique IDs that map a region of the editor to elements in the browser. Most diff algorithms have only the plain source text to work from.
4. Diffs generated while a user is typing will typically be small.

For the "live HTML highlighting" feature that we shipped a couple of sprints back, we did some simple parsing of the HTML, coming up with a tag ID for each element in the HTML. This ID was placed in CodeMirror marks and in the HTML sent to the browser during live development. For the prototype, we extended this by adding a more complete HTML tokenizer and a parser that builds a simple document object model (Simple DOM).

We had thought there would be a separate step for knowing when the HTML was valid. The conclusion we came to, however, was that we already needed a parsing step for building the Simple DOM and we could just use that step to also determine the document's validity at the same time.

We hang on to the Simple DOM that is built. When the user types into the editor, we try to build a new Simple DOM. If the HTML they have entered is invalid, then we don't make any changes to the browser. If we can successfully build a Simple DOM, then we diff the new and old DOMs and the diff is sent to the live development code so that the browser can be updated. Currently, the diff supports these operations:

1. Text Insert (add a new text node)
2. Text Replace (replace an entire text node, we don't send portions of text nodes)
3. Text Delete (delete a text node)
4. Element Insert (add an element to the document)
5. Element Delete (delete an element from the document)
6. Attribute Change (change an element attribute's value)
7. Attribute Delete (delete an element's attribute)
8. Attribute Add (add an attribute to an element)

These fine-grained changes allow us to make only small modifications to the page in the browser, maintaining as much state as we can. We also want to add the ability to move a node from one place in the tree to another as another optimization for maintaining the browser state.

## Optimizing through Editor Events ##

We considered a possible approach in which we have editor events that propagate directly out to the browser. We came to realize, however, that we still needed to identify the semantic changes we needed to make (in other words, the diff operations listed in the previous section).

We could, however, use editor events as a trigger for doing updates to only a part of the document. The process looks like this:

1. User makes changes in a portion of the document
2. We identify the earliest point in the document that was changed
3. We use the CodeMirror marks to find the enclosing tag ID for that part of the document
4. We build a new Simple DOM sub-tree for that enclosing tag
5. We diff the new sub-tree and the old sub-tree
6. We replace the sub-tree in the old complete Simple DOM with the new sub-tree

This optimization means that for many changes a user may make to their document, we're only diffing a very small number of elements in the Simple DOM. The diff algorithm we use is an efficient diff, but diffing is still expensive and minimizing the area of interest for the diff is worthwhile (see "Performance" below).

## Sending Changes to the Browser ##

Our prototype sends a custom jQuery build (designed to not interfere with whatever the user has loaded on the page) to the browser. This is used to make manipulation of the browser's DOM easier.

For each of the diff operations, we generate a small bit of JavaScript that is sent to the browser to make the required changes. Some of the JavaScript was getting a bit larger than we wanted, and escaping it made the code a bit ugly. In a production version of this prototype, we would want to send the diff operations directly to the browser as JSON and have a library on that end apply the changes.

## Performance ##

These first tests were performed using a copy of the Getting Started with Brackets document. For each operation, several tests were performed for comparison:

* Base edit: how long Brackets takes to make the change with none of our new code in operation
* Full: how long did the update take using a full diff
* Incremental: how long did the update take using the "Editor Events" optimization described a couple of sections ago

The full and incremental times include the "base edit" time. The times listed are in milliseconds for each of 10 runs. High-resolution timers were used in the testing (`window.performance.webkitNow`).

Additionally, the tests measured the time it takes to build the Simple DOM for the whole document and apply the marks.

* The first test that was executed took just **5.63 ms** to build the Simple DOM.
* Applying the CodeMirror marks took **16.56 ms**.
* Subsequent tests took as little as **1.23 ms** to build the DOM and **6.57 ms** to apply the marks, likely due to JavaScript JIT performance


### Text Replacement ###

This test was equivalent to selecting a range of text and pasting in new text:

```javascript
editor.document.replaceRange("AWESOMER", {line: 12, ch: 12}, {line: 12, ch: 19});
```

<table>
    <thead>
        <tr><th>Test</th><th>Min</th><th>Max</th><th>Median</th></tr>
    </thead>
    <tbody>
        <tr><td>Base edit</td><td>7.49</td><td>15.73</td><td>9.38</td></tr>
        <tr><td>Full</td><td>100.91</td><td>219.74</td><td>108.43</td></tr>
        <tr><td>Incremental</td><td>17.43</td><td>22.45</td><td>19.64</td></tr>
    </tbody>
</table>

On this test, you can see that the incremental updates took about twice as long as the base edit and doing a full update took 5 times longer than that! I have heard it said that anything less than ~60 ms is perceived as instantaneous by the user, so the 20 ms time is quite good.

### Simulated Typing of Text ###

This test has the same result as the previous one, but acts as if the user typed each character individually.

```javascript
editor.document.replaceRange("A", {line: 12, ch: 12});
editor.document.replaceRange("W", {line: 12, ch: 13});
editor.document.replaceRange("E", {line: 12, ch: 14});
editor.document.replaceRange("S", {line: 12, ch: 15});
editor.document.replaceRange("O", {line: 12, ch: 16});
editor.document.replaceRange("M", {line: 12, ch: 17});
editor.document.replaceRange("E", {line: 12, ch: 18});
editor.document.replaceRange("R", {line: 12, ch: 19});
```

The timings below are a total for these 8 edit operations.

<table>
    <thead>
        <tr><th>Test</th><th>Min</th><th>Max</th><th>Median</th></tr>
    </thead>
    <tbody>
        <tr><td>Base edit</td><td>53.38</td><td>76.82</td><td>63.73</td></tr>
        <tr><td>Full</td><td>744.03</td><td>866.29</td><td>755.27</td></tr>
        <tr><td>Incremental</td><td>184.60</td><td>258.96</td><td>200.98</td></tr>
    </tbody>
</table>

These results are similar to the ones in the previous section, though the incremental update got a bit more expensive.

### New Tag ###

This test simulates pasting a new tag into the document.

```javascript
editor.document.replaceRange("<div>New Content</div>", {line: 15, ch: 0});
```

<table>
    <thead>
        <tr><th>Test</th><th>Min</th><th>Max</th><th>Median</th></tr>
    </thead>
    <tbody>
        <tr><td>Base edit</td><td>7.38</td><td>11.97</td><td>7.94</td></tr>
        <tr><td>Full</td><td>92.96</td><td>181.41</td><td>103.84</td></tr>
        <tr><td>Incremental</td><td>494.74</td><td>843.90</td><td>519.87</td></tr>
    </tbody>
</table>

Making this change takes a similar amount of work as pasting in new text for both the base edit and full diff cases. Something is amiss with the incremental test in this case, however. This must be hitting some edge case in the code. I am confident we would be able to fix this.

### Typing a New Tag ###

This test is the same as the previous one, but measures typing of the tag. Note that it simulates the automatic addition of the closing tag.

```javascript
editor.document.replaceRange("<", {line: 15, ch: 0});
editor.document.replaceRange("d", {line: 15, ch: 1});
editor.document.replaceRange("i", {line: 15, ch: 2});
editor.document.replaceRange("v", {line: 15, ch: 3});
editor.document.replaceRange(">", {line: 15, ch: 4});
editor.document.replaceRange("</div>", {line: 15, ch: 5});
editor.document.replaceRange("N", {line: 15, ch: 5});
editor.document.replaceRange("e", {line: 15, ch: 6});
editor.document.replaceRange("w", {line: 15, ch: 7});
editor.document.replaceRange(" ", {line: 15, ch: 8});
editor.document.replaceRange("C", {line: 15, ch: 9});
editor.document.replaceRange("o", {line: 15, ch: 10});
editor.document.replaceRange("n", {line: 15, ch: 11});
editor.document.replaceRange("t", {line: 15, ch: 12});
editor.document.replaceRange("e", {line: 15, ch: 13});
editor.document.replaceRange("n", {line: 15, ch: 14});
editor.document.replaceRange("t", {line: 15, ch: 15});
```

<table>
    <thead>
        <tr><th>Test</th><th>Min</th><th>Max</th><th>Median</th></tr>
    </thead>
    <tbody>
        <tr><td>Base edit</td><td>121.73</td><td>133.53</td><td>126.03</td></tr>
        <tr><td>Full</td><td>1459.80</td><td>1534.03</td><td>1518.74</td></tr>
        <tr><td>Incremental</td><td>921.66</td><td>1070.39</td><td>977.61</td></tr>
    </tbody>
</table>

Here, we see that the incremental updates takes the lead over the full updates again, though the difference has narrowed. The one second spent doing the updates may sound like a lot, but given the time that the user will spend typing that text, it is not as bad as it may seem.

### Big File Test ###

The "Getting Started with Brackets" page is 146 lines long and about 6 KB in size. It is not a very big file. As a stress test, I tried loading up [the W3C Packaged Web Apps](http://www.w3.org/TR/2012/REC-widgets-20121127/) document. This is 4407 lines long and 441 KB in size. I only tried the "Text Replacement" test (the one that simulates selecting a range of text and pasting in new text).

I only did one run of this test, so the JavaScript JIT compiler would not have been able to do some of its optimizations.

The base edit took **108.73 ms** which means that there would be a slightly perceptible delay as the user pastes in the text, but it would not be too bad.

Despite the document's size, the initial DOM build took only **179.40 ms**. This is not surprising, as building the Simple DOM would increase roughly linearly with the size of the file. Performing a full diff on each keystroke would take this much time *plus* the time to run the diff, which is more expensive than building the Simple DOM.

The surprising part was that marking the text in CodeMirror took more than **23 seconds**! There is clearly something in there that is not scaling linearly and even our live highlighting feature would have a problem with this. I confirmed on the current master that loading this document and turning on Live Development causes a very long freeze before there is a Live Development error and then Brackets finally becomes responsive again.

The incremental edit took **293.79 ms**.

## Performance Conclusions ##

The code implemented today is a prototype and has not been profiled. For modestly sized files, it already performs acceptably well and we should be able to make the performance acceptable on larger files.

The current, shipping Live Development has a performance issue that we need to address if we want people to be able to use Live Development on large files.

Using incremental, sub-tree updates is a worthwhile optimization that already generally performs much faster than running a full diff. We can tweak how we decide when to perform the diff and batch up edits to make sure that editing performance remains good while updating the browser reasonably quickly.

## Changes in the Browser ##

At this phase, live HTML editing has no knowledge about changes that are occuring in the browser's DOM. This means that edits made in Brackets could be invalid as far as the current DOM is concerned. We can detect cases where the user makes an edit to an element that is no longer on the page (because there won't be an element with the Brackets-assigned ID available in the browser DOM). However, we couldn't detect cases where a button label had been changed, for example.

For the most part, our approach to creating a diff and updating the browser should be fairly resilient to changes that JavaScript makes in the browser DOM. We could consider using something like [Mutation Observers](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver) to make it clear to the user when they would need to reload the page in order to see changes that they're making.

## Prototype Status ##

Prototypes are generally considered throwaway code. The code in the [ResearchLiveHTML](https://github.com/adobe/brackets/tree/ResearchLiveHTML) branch needs a good deal of work, but should be considered a good starting point rather than a complete throwaway.

# Old Content #

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

In the context of this feature, "well-formed HTML" means markup and content that can be parsed/diffed in order for us to calculate live edits. In general, our goal is to be as lax as possible and handle HTML that isn't strictly "valid" in the spec sense, but has a clean enough structure that we can parse it.

Modern browsers are _very_ forgiving in the markup they render. They ignore what the don't understand and subsequent markup and content is generally not affected.

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

We could also consider being even laxer. For example, if an open tag has no matching close tag, we could "guess" where it should be based on the parentage rather than bailing. However, that might lead to undesirable side-effects as the user continues editing. That said, we should at least handle those cases for tags that the spec allows to be implicitly closed, like `<li>` and `<p>`; we aren't currently handling those in the prototype.

## Pushing changes to the browser

Recommendations: Use (1) a custom build of jQuery limited to DOM manipulation or (2) write our own DOM manipulation APIs. 

This [pull request](https://github.com/adobe/brackets/pull/4237) loads the full jQuery library (into a private namespace) in the inspected page via ``RemoteAgent``. We use this method to support CSS rule and HTML element highlighting features in Live Preview.

The prototype exposes a subset of jQuery DOM manipulation methods to support the text editing use cases we've outlined above for basic CRUD operations on a DOM tree (attr, removeAttr, before, after, append, prepend, text, detach, remove).

**[nj]** It turns out that some edits are not straightforward to do with just the basic jQuery functions. In particular, jQuery doesn't make it easy to select text nodes; you have to get the `contents()` of the parent to even see them (`children()` doesn't include text nodes), so I ended up falling back on standard DOM APIs anyway. My suggestion is that we get rid of jQuery and just use the standard DOM APIs on the browser side. Also, we can tune the edit descriptors generated by the differ to whatever makes it easiest for us to implement on the browser side.

# Ancient content

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