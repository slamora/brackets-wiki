This document summarizes several options for getting simple support for LESS in various core Brackets features, assessing the implementation effort, robustness and level of feature support.

Note that we're considering LESS first since it seems likely to be easier to support initially than Sass/SCSS because there is a JavaScript processor for LESS. However, this document will also describe whether these solutions can be easily repurposed for Sass or SCSS.

### Scope of investigation

The following Brackets features were considered for support in this investigation:

* CSS Quick Edit - find selectors in LESS files that match the current tag/class/id
* CSS Quick Edit - ensure that New Rule works properly in LESS files
* Quick Find Definition - allow jumping to selectors in LESS files (would need to decide if we show the full selector, or just the raw nested selector)
* Live Preview - update the browser as you edit in LESS files
* Live Preview Highlight - highlight DOM nodes that match the LESS rule the cursor is in

The following are out of scope and not being considered yet:

* LESS-specific code hints (e.g. mixins, variables)
* LESS-specific workflows for things like New Rule (e.g. letting you pick an existing rule to nest within)

Note that the following features already work with LESS (as well as SCSS - **TODO** check on whether they work with Sass):

* Code hints for CSS properties/values in LESS files
* Quick Docs in LESS files
* LESS code coloring
* Color Quick Edit in LESS files
* Quick View for colors/images in LESS files
* Timing Curve Quick Edit

### Summary of options

Roughly speaking, there are two groups of functionality:

* CSS Quick Edit, Quick Find Definition, Live Preview Highlight
    * These rely on the ability to determine where selectors are in the source file and what their final form will be in the generated CSS. In the case of LESS and Sass/SCSS, this means finding nested selectors and determining how they combine to form a final CSS selector.
* Live Preview
    * This relies on the ability to recompile the source LESS/Sass/SCSS file into its final CSS stylesheet form and have the new compiled version of the stylesheet be hot-reloaded in the browser in place of the previous compiled stylesheet.

There are four options considered here, summarized in the table below. Detailed descriptions of each option follow the table. Note that these are not necessarily mutually exclusive - for example, we could combine option 1 with options 2 or 4 in order to provide a complete solution.

| Option | Effort | User setup required | Robust? | CSS Quick Edit | Quick Find Definition | Live Preview | Live Preview Highlight | Extends to Sass/SCSS? | Notes |
| ------ | ------ | ------------------- | ------- | -------------- | --------------------- | ------------ | ---------------------- | --------------------- | ----- |
| (1) Simple nested selector parsing | Low | No | Medium | Yes | Yes | **No** | Yes | Yes (SCSS); Sass would be nontrivial work | |
| (2) Run LESS compiler (no source maps) | Medium | **Some?** | High | **No** | **No** | Yes | **No** | Would require libsass/ruby for SCSS | Would likely combine this with (1) |
| (3) Run LESS compiler with source maps | Medium-High | **Some?** | High | Yes | Yes | Yes | Yes | Would require libsass/ruby for SCSS | Likely slower than option (2), but would be guaranteed to give correct results. |
| (4) Use user's own autocompile process | Low-Medium | No | High | **No** | **No** | Yes | **No** | Yes | Would likely combine this with (1). |

### (1) Simple nested selector parsing

This would be a simple extension of the existing CSSUtils parsing code to find nested selectors and determine how to combine them with parent selectors. The effort should be fairly small, and should generalize easily to SCSS (but not Sass, which would require us to parse using CodeMirror's Sass mode). However, it wouldn't necessarily be robust, because we wouldn't be relying on the real LESS parser.

To be more specific, we would support:
* nested rules
* "&" references to parent selectors

For the initial implementation, we likely would *not* support:
* "extend" functionality (:extend() in LESS, @extend in SCSS)

This might be a good first thing to implement, since it gives us everything but Live Preview.

### (2) Run LESS compiler (no source maps)

In this option, we would run the LESS compiler (either within Brackets or in Node) to generate the compiled CSS stylesheet (either in memory or in a temporary location on disk), then load the recompiled stylesheet in the browser in place of the previous version. This would require us to match up source LESS files with their generated stylesheets so that we can find the appropriate style tag in the browser and replace it. If the user is directly including the LESS file in HTML, we might be able to do this automatically, but if not, we might need to guess the mapping based on filename, or let the user configure it somehow.

Note that this only gives us live preview. We would likely want to combine it with option (1) in order to get the other features.

We would need to do some work to determine the performance hit of regenerating the compiled stylesheet. Depending on the performance, we might not want to update it on every keystroke, but only update it every so often.

Extending this to Sass/SCSS would require us to ship Ruby/Sass or libsass (which would only give us SCSS).

### (3) Run LESS compiler with source maps

This is like option (2), but by adding source maps to the mix, we could scan the resulting CSS stylesheet for final-form selectors, and then map them back to their locations in the original LESS file in order to support Quick Edit, etc.

This would be the most robust option of all of these, but it might also be a more significant performance hit (compared to (1) + (2)) for the non-Live Preview features, because we'd need to regenerate the stylesheet/source maps in addition to scanning the compiled stylesheet.

The performance implications for Live Preview are similar to (2). Also, similar to (2), extending this to Sass/SCSS would require us to ship Ruby/Sass or libsass (which would only give us SCSS).

Implementation cost is a bit more than (2) since we have to deal with the source maps, but Jason has already done some prototyping around this.

### (4) Use user's own autocompile process

This is similar to (2), but rather than us running the compilation ourselves, we could rely on the user's own existing compilation setup, assuming that the user has an automatic compilation workflow (e.g. via a watch process). In this world, we would basically just watch for changes to the compiled version of the file, and then reload it in the browser when we detect the change.

This option might not need to require the user to do any setup at all. We would first determine what CSS files are loaded by the browser, and how they map to style tags in the HTML (we already know how to do this mapping because of remote debugger events). Then, any time one of those CSS files changes, we would simply reload the new version and replace the existing stylesheet in the live browser.

The disadvantage of this is that it would only update the stylesheet on save, rather than as the user types. But that would be better than what they have with Brackets today.

Like (2), this only gives us live preview. Most likely we would want to combine it with (1). However, even if we do (2) or (3), we might want to consider implementing this anyway, since it seems like an easy win for users that already have a compilation workflow. Also, it would work for Sass/SCSS (or any other preprocessor) as well as LESS.

_\[PF: This \*should\* already work, using file watchers. So effort might actually be near-zero...\]_

### Proposal

My initial thought is that we should implement (1) and (4) before 1.0. 

(1) is easy and gives us all the features except Live Preview in a performant and reasonably robust way. It also gives us both LESS and SCSS.

(4) gives us Live Preview, and has the advantages that it's reliable and works with any preprocessor technology (including Sass and SCSS). Its disadvantages are that it only updates the browser on save, and that it relies on the user already having an automated compilation setup. But presumably any user who is using a preprocessor has *some* compilation setup already, whether via command line tools or via something like CodeKit. And it's not that bad to hit Cmd-S every so often to see your changes - it's still better than reloading the whole page in the browser, because it would only reload the affected stylesheet.

Note that (4) might already be working now that we have file watchers - if so, we should just test it and make sure it works correctly, and then promote its use.