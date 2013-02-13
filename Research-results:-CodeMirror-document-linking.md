CodeMirror recently introduced the notion of document linking, where the documents underlying multiple views can be linked together, and some documents can be "subsets" of other documents. This feature is intended for use in cases like Brackets's inline editors, as well as things like split views.

We recently did some testing around integrating these linked documents into Brackets, using CodeMirror as of marijnh/CodeMirror@3b37d7a. The prototype is in the `nj/doc-view-research` branch. Here's what we've found so far:

* While CodeMirror's documents and views are now somewhat separated, documents themselves don't raise change events. This means we can't really use them as a true model class (completely separate from editors). So, for now, we would need to keep our current scheme, which maintains a "master editor" for each document.
* It was quite easy to replace our current (hacky and occasionally buggy) cross-editor synchronization code with CodeMirror's linked documents. 
  * The shared history feature (which makes undo/redo work properly across linked documents) seems to work well--in my basic manual testing, undo/redo worked across different editors (both inline and full editors) as I would expect, and the dirty bit was properly synchronized.
  * However, the InlineEditorProviders unit tests currently fail due to a stack overflow, probably because of infinite recursion. I spent some time trying to track this down but wasn't able to figure it out, as the debugger doesn't actually seem to break on stack overflow exceptions or return a useful stack. More investigation is necessary.
* It was also easy to replace our current scheme for showing a portion of a document in an inline editor (which relies on CodeMirror's "collapsed spans" feature) with subdocuments. Most things seemed to work properly:
  * Adding lines in a full document above a subdocument's range correctly updated the subdocument's line numbers (both in the subdocument itself and elsewhere in the inline editor, because we separately track text ranges ourselves as well).
  * Deleting the range in a full document corresponding to a subdocument correctly collapsed the inline editor (again because we track the text ranges ourselves).
  * However, I found a data corruption bug in this case: if you type in the subdocument, then go to the main document and undo, you can get duplicate lines in the main document. This needs to be isolated and filed against CodeMirror.

I also found a bug in master: if you have two inline editors open on the same file, with one referring to a rule earlier in the file than the other, and you add lines to the first editor, the line number in the header of the second editor doesn't update (though its line numbers and text ranges do). This should be easy to fix (and isn't really related to the CodeMirror stuff, but needs to be filed).

