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

There are four options considered here, summarized in the table below. Detailed descriptions of each option follow the table. Note that these options are not all mutually exclusive - we could choose to do more than one eventually.

| Option | Implementation cost | User setup required | Robustness | CSS Quick Edit | Quick Find Definition | Live Preview | Live Preview Highlight | Extends to Sass/SCSS? | Notes |
| ------ | ------------------- | ------------------- | ---------- | -------------- | --------------------- | ------------ | ---------------------- | ---------- | ----------------- | ----- |
| 1 - Simple nested selector parsing | Low | No | Medium | Yes | Yes | **No** | Yes | Yes (SCSS); Sass would be nontrivial work | |
| 2 - Nested selector parsing + Run LESS compiler (no source maps) | Medium | No | Medium-High | Yes | Yes | Yes | Yes | Would require libsass/ruby for live preview | Note that this is basically an extension of (1). |
| 3 - Run LESS compiler with source maps | Medium-High | No | High | Yes | Yes | Yes | Yes | Would require libsass/ruby for live preview | Might be less performant than option (2), but would be guaranteed to give correct results. |
| 4 - Use user's own autocompile process | Low-Medium | **Yes** | High | **No** | **No** | Yes | **No** | Yes | This would need to be combined with one of the other options to get features other than live preview. However, we might want to provide it anyway even if we do (2) or (3). |

### 1 - Simple nested selector parsing

TODO

### 2 - Simple nested selector parsing + Run LESS compiler (no source maps)

TODO

### 3 - Run LESS compiler with source maps

TODO

### 4 - Use user's own autocompile process

TODO
