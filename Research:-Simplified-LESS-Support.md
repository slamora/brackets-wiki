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
* Timing Curve Quick Edit - **TODO** this might already work, or if not, should be easy to make it work

### Summary of options

Roughly speaking, there are two groups of functionality:

* CSS Quick Edit, Quick Find Definition, Live Preview Highlight
    * These rely on the ability to determine where selectors are in the source file and what their final form will be in the generated CSS. In the case of LESS and Sass/SCSS, this means finding nested selectors and determining how they combine to form a final CSS selector.
* Live Preview
    * This relies on the ability to recompile the source LESS/Sass/SCSS file into its final CSS form and have the new version of the file be reloaded in the browser in place of the previous compiled version.

There are four options considered here, summarized in the table below. Detailed descriptions of each option follow the table. Note that these are not necessarily mutually exclusive - for example, we could combine option 1 with options 2 or 4 in order to provide a complete solution.

| Option | Effort | User setup required | Robust? | CSS Quick Edit | Quick Find Definition | Live Preview | Live Preview Highlight | Extends to Sass/SCSS? | Notes |
| ------ | ------ | ------------------- | ------- | -------------- | --------------------- | ------------ | ---------------------- | --------------------- | ----- |
| 1 - Simple nested selector parsing | Low | No | Medium | Yes | Yes | **No** | Yes | Yes (SCSS); Sass would be nontrivial work | |
| 2 - Run LESS compiler (no source maps) | Medium | No | Medium-High | **No** | **No** | Yes | **No** | Would require libsass/ruby for SCSS | Would likely combine this with (1) |
| 3 - Run LESS compiler with source maps | Medium-High | No | High | Yes | Yes | Yes | Yes | Would require libsass/ruby for SCSS | Likely slower than option (2), but would be guaranteed to give correct results. |
| 4 - Use user's own autocompile process | Low-Medium | **Yes** | High | **No** | **No** | Yes | **No** | Yes | Would likely combine this with (1). Could provide it as an alternative to (2)/(3). |

### 1 - Simple nested selector parsing

TODO

### 2 - Run LESS compiler (no source maps)

TODO

### 3 - Run LESS compiler with source maps

TODO

### 4 - Use user's own autocompile process

TODO
