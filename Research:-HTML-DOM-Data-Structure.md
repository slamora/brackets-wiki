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
  children: [ (list of child nodes) ]
}
```

Comments are thrown away and text nodes are given a similar, but simpler structure:

```javascript
{
  tagID: "12345.0",
  parent: (the object above),
  content: "Hello, world!"
}
```
