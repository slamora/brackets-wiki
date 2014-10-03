Status: Implemented in Sprint 30

# HTML Diff/Patch Implementation #

To enable Live Development of HTML files, we looked for a way to:

1. quickly
2. change relatively small parts of the page

as the user types. It turns out that trying to quickly generate minimal diffs of XML files has been an area of research over the past few years. That research has generally been focused on one-time processing of two versions of a file that are sitting on disk. There turned out to be a number of differences between that kind of processing and diffing real time changes to a document where we can lean on the editor for some help in identifying specific parts of the document.

In this document, I'll cover the basics of how our diffing and patching works in Sprint 30. As of this sprint, most of the code of interest lives in `src/language/HTMLInstrumentation.js`, though the code responsible for applying the patches lives in `src/LiveDevelopment/Agents/RemoteFunctions.js`.

At the highest level, live HTML works like this:

1. Read each token from the HTML text and turn it into a "Simple DOM" data structure (just plain JavaScript objects)
2. Compare the previous version of the Simple DOM with the newly generated one, producing a set of edits
3. Send the edits to the browser
4. Apply the edits in the browser so that the user sees the changes made in Brackets

## Tokenization and the Simple DOM ##

`src/language/HTMLTokenizer.js` contains a simple HTML tokenizer, adapted from the ["htmlparser2" library](https://github.com/fb55/htmlparser2). As each token is read from the file, a "Simple DOM" structure is built up. An element in the Simple DOM looks something like this:

```javascript
{
  tagID: 12345,
  tag: "div",
  parent: (another object),
  attributes: { style="color: red" },
  children: [ (list of child nodes) ],
  attributeSignature: (a hash),
  subtreeSignature: (another hash),
  childSignature: (a third hash)
}
```

The interesting code for generating this structure is in `SimpleDOMBuilder.prototype.build`.

HTML Comments are thrown away and text nodes are given a similar, but simpler structure:

```javascript
{
  tagID: "12345.0",
  parent: (the object above),
  content: "Hello, world!",
  textSignature: (yet another hash)
}
```

For text nodes, the tagID is derived from the element preceding it. The tag ID of a text node is not important and could even go away in a future iteration.

For elements, the tag ID is initially assigned as a simple ascending integer (stored as a module global variable). Once the ID is assigned, though, it is used in two ways:

1. it is stored in a CodeMirror mark that starts at the beginning of the tag in the text and ends at the end of the tag
2. it is automatically added to the page that the static server sends to the browser (each element is given a data-brackets-id="tagID" attribute)

As the user types, CodeMirror automatically adjusts the boundaries of the marks. This makes it possible for us to identify the portion of the DOM that the user has changed with their typing. Using this information, we have an "incremental" mode. If the user is typing text within a tag, we tokenize and build a DOM for only that tag and then run the diff algorithm on only that part of the tree. This can make things a lot more efficient when users are just editing text or attribute values.

This incremental mode can also be fragile. If the user types anything that can make structural changes to the document (characters like `<` and `>`, for example), then we simply reparse the whole file rather than trying to predict the kinds of changes that were made.

I'll also note that if the tokenizer runs into any problems or if the Simple DOM builder spots mismatched tags, we stop updating and keep watching for the file to get back to a good state.

The root node of the Simple DOM contains a `nodeMap` property that maps from tagID directly to the matching node in the tree.

## Generating Diffs ##

Once we have two Simple DOMs in hand, we can generate a diff, which I'll also refer to as a list of edits. The edits are a set of operations that will mutate the old DOM into the new one when applied in order.

Since all of our elements have IDs on them and because we are not going for the absolute minimum diff possible, we are able to generate the list of edits with a single pass through just some of the nodes of the DOM.

The `domdiff` function is responsible for generating the list of edits. In Sprint 30, this function is 500 lines long, though it has a bunch of much smaller functions defined inside of it. At the bottom of the `domdiff` function is a `do {} while` loop that is responsible for stepping through the tree and calling the other functions which generate the edits.

Starting at the root of the new tree, each element is pulled from the old tree by tag ID. The various `signature` properties are used to minimize how much comparison needs to be done.

* `attributeSignature` tells us in a single integer comparison whether any of the attribute key/value pairs have changed
* `childSignature` tells us (again, in a single integer comparison) if any of the text nodes or child elements have changed just for the element we're looking at. If the `childSignature` has changed, we call `generateChildEdits` which is responsible for most of the edit operations you'll see
* `subtreeSignature` is like `childSignature` but for the entire subtree under the current element. `subtreeSignature` also includes attributes, because it determines whether we need to look at subtree elements at all. If `subtreeSignature` has changed, then we add all of the children to the queue of elements to examine

Using these signatures, we can quickly eliminate most of the tree from comparisons.

## The Edits (Diffs) ##

We currently have 11 change operations defined: 

 * elementInsert
 * elementDelete
 * elementMove
 * elementReplace
 * textInsert
 * textDelete
 * textReplace
 * attrDelete
 * attrChange
 * attrAdd
 * rememberNodes (a special instruction that reflects the need to hang on to moved nodes)

The `attr*` edits are generated by the `generateAttributeEdits` function when the `attributeSignature`s don't match. They're very straightforward:

```javascript
{
    type: "attrChange",
    tagID: 12345,
    attribute: "style",
    value: "color: blue"
}
```

The other edits are generated via `generateChildEdits`. They are generated by simultaneously stepping through the old children and the new and looking for differences. If the tagID of the old and new elements vary, we look to see if an element appears in the new tree but not the old (thus it's an `elementInsert`) or if the element appears in the old tree but not the new (an `elementDelete`).

The edits are expressed in terms of other child elements, in a way that is generally straightforward for the browser to implement. In the case of text changes, we *have* to express those changes in terms of child elements because text nodes do not have IDs that we can easily look up in the browser. To determine where an insert or text change will happen, we use one of the following properties of the edit object:

* firstChild: true
* lastChild: true
* beforeID: 12345 (just before the element with that tagID)
* afterID: 12345 (immediately following this element)
* afterID: 12344, beforeID: 12345 (between these two elements. This form is generally used for `textDelete` or `textReplace`)

`elementMove` is special in that it's designed to handle cases where an element has moved from one parent to another. This happens more often than you might think (delete a closing tag followed by an open tag, merging two tags together and you can make this happen). An `elementMove` can occur when a parent node is disappearing from the tree. In other words, when the move itself is supposed to occur, the element we're moving may have been deleted from the tree because its parent was deleted from the tree! For that reason, all of the elements that are being moved are listed in a `rememberNodes` operation that appears at the beginning of the edit list.

`elementReplace` is another special operation that is used when the tag name changes. The browser DOM does not offer a way to change tag names, so an `elementReplace` will be accompanied by insert operations for all of that element's children.

## Applying the Edits ##

The `RemoteFunctions` module steps through the edits in order and performs straightforward DOM manipulations to make the edited part of the browser DOM match up with the Simple DOM. At this point, nothing is watching for changes that happen in the browser DOM that could cause the browser DOM to be out of sync with the Simple DOM in Brackets.

`RemoteFunctions` has a set of unit tests. Additionally, the `HTMLInstrumentation` tests also load `RemoteFunctions` and have implemented just enough of the browser DOM methods in order to:

1. Take the previous DOM
2. Make some changes to the CodeMirror document
3. Use `domdiff` to generate edits between the DOMs
4. Use `RemoteFunctions` to apply those edits to a copy of the previous DOM
5. Do an exact comparison between the patched previous DOM and the new DOM

This allows us to test both diff generation and patch application in one quick step.
