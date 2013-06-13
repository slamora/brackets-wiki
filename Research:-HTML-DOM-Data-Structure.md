# HTML DOM Data Structure Research #

Status: Research Starting (Sprint 27)

This page will collect the results of the [HTML DOM Data Structure research story](https://trello.com/card/5-research-data-structure-for-html-dom-edit-mapping/4f90a6d98f77505d7940ce88/844).

## Test Scenarios ##

Each scenario is numbered for ease of reference, but the order makes no difference.

These scenarios use the [Getting Started sample file](https://github.com/adobe/brackets/blob/f482326997f4b6e09a2640c770dbd915c81851a3/samples/root/Getting%20Started/index.html). That link points to a specific SHA of the file to ensure that these scenarios keep working even if the file changes.

Each scenario starts with a fresh copy of the file, though many could work as a straight run-through.

### 1: Text node manipulation

* Line 13: insert "YOU SHOULD BE " immediately after the `<h1>` open tag
    * Expected `<h1>YOU SHOULD BE GETTING STARTED WITH BRACKETS</h1>`
* Line 13: Replace the text "GETTING STARTED WITH" with "ALREADY USING"
    * Expected `<h1>YOU SHOULD BE ALREADY USING BRACKETS</h1>`

That was easy, wasn't it?

### 2: Attributes

* Line 13: after "h1", type ' class="main"'.
    * Expected `<h1 class="main">GETTING STARTED WITH BRACKETS</h1>`
* Line 61: Change "showing" to "displaying"
    * Expected `<img alt="A screenshot displaying CSS Quick Edit" src="screenshots/quick-edit.png" />`
* Line 61: Delete the `alt` attribute entirely
    * Use backspace starting from the "s" in `src`
    * Expected `<img src="screenshots/quick-edit.png" />`

Still kind of easy, but there are some invalid states while editing.

### 3: Nesting/Across Lines

* Line 75: Surround `"save/reload dance"` with a `<span class="save-reload">` tag
    * Start with the caret before the opening "
    * Type in the new tag
    * Navigate to just after the closing "
    * Type `</span>`
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

* Line 141: Adding a new item after this one
    * Navigate to the end of the line
    * Hit enter to create a new line at 142
    * Type `<li>Sometimes read about us on `
    * Note that the state is valid as soon as the `>` is typed
    * Type `<a href="http://news.ycombinator.com">Hacker News</a></li>`

### 5: Tables

* Line 150: Adding a table
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

* Cut/paste, same parent: Select the `<li>` on line 142, cut it, then move to the beginning of line 141 and paste. This should result in the DOM node (and its children) moving without being recreated.
* Cut/paste, different parents: Select the `<img>` on line 61, cut it, then move to the text at the beginning of line 65 (inside the `<p>` tag) and paste. This should result in the DOM node moving without being recreated.
* **TODO:** For copy/paste, it's harder to know what to do since you're actually creating a new node. Is there any expectation that you'll copy the state of the original node somehow, or should we just treat it as if you retyped the text?

### 7: Changing tags

### 8: Changes to HEAD

### 9: Big file editing

