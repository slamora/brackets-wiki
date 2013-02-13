CodeMirror recently introduced the notion of document linking, where the documents underlying multiple views can be linked together, and some documents can be "subsets" of other documents. This feature is intended for use in cases like Brackets's inline editors, as well as things like split views.

I recently did some testing around integrating these linked documents into Brackets, using CodeMirror as of marijnh/CodeMirror@3b37d7a. The prototype is in the `nj/doc-view-research` branch. 

## tl;dr

* Integrating the new linked document/subdocument features was pretty straightforward. The API seems to be very usable, and migrating to linked documents has some benefits:
  * The primary user-facing benefit is that undo/redo behave in a much more sane fashion between inline and full editors. 
  * There is also a perceptible performance benefit when creating inline editors that refer to small portions of large files, since we're no longer creating an editor with the entire contents of the file and then hiding most of the lines.
  * Additionally, there are a number of internal architectural benefits, because we can rip out a couple of somewhat fragile hacks that we had previously put in for edit synchronization.
* However, documents themselves don't raise change events (filed as marijnh/CodeMirror#1238), so we can't eliminate our `_masterEditor` hack. This is probably not a major blocker for migrating to the linked document stuff, because we get other benefits out of it anyway.
* There are a few serious bugs that need to be isolated and filed upstream: 
  * Undoing changes from a linked subdocument in the main document can lead to corruption.
  * Tokenization in subdocuments seems to be somewhat broken, which breaks code hinting in inline editors.
  * Exception in matchbrackets related to linked documents.
* There are also a handful of other unit test failures that need investigating, but they don't seem scary overall.
* Otherwise, most things seem fine.

## Detailed notes

* While CodeMirror's documents and views are now somewhat separated, documents themselves don't raise change events. This means we can't really use them as a true model class (completely separate from editors). So, for now, we would need to keep our current scheme, which maintains a "master editor" for each document.
  * In general, it seems that the real benefit of this isn't going to be in document/view separation per se (which is what we were originally thinking)--but the linked documents do help us get rid of some major hacks, and the master editor hack isn't too bad (and has been working well for awhile), so it's probably okay if this doesn't get addressed in the short term.
* It was quite easy to replace our current (hacky and occasionally buggy) cross-editor synchronization code with CodeMirror's linked documents. 
  * The shared history feature (which makes undo/redo work properly across linked documents) seems to work well--in my basic manual testing, undo/redo worked across different editors (both inline and full editors) as I would expect, and the dirty bit was properly synchronized.
  * However, the InlineEditorProviders unit tests initially failed due to a stack overflow, probably because of infinite recursion. I spent some time trying to track this down but wasn't able to figure it out, as the debugger doesn't actually seem to break on stack overflow exceptions or return a useful stack.
  * After changing the code to use subdocuments (as in the next bullet), the infinite recursion problem went away, revealing a number of unit test failures.
* It was also easy to replace our current scheme for showing a portion of a document in an inline editor (which relies on CodeMirror's "collapsed spans" feature) with subdocuments. Most things seemed to work properly:
  * Adding lines in a full document above a subdocument's range correctly updated the subdocument's line numbers (both in the subdocument itself and elsewhere in the inline editor, because we separately track text ranges ourselves as well).
  * Deleting the range in a full document corresponding to a subdocument correctly collapsed the inline editor (again because we track the text ranges ourselves).
  * However, I found a data corruption bug in this case: if you type in the subdocument, then go to the main document and undo, you can get duplicate lines in the main document. This needs to be isolated and filed against CodeMirror.
* After both these changes, I ran the inline editor unit tests. As mentioned above, the infinite recursion problem went away after making both sets of changes (which is a bit scary). At this point there were a fairly large number of failures.
  * Most of the failures were due to two assumptions the unit tests were making: that editors were representing visible ranges by line hiding, and that inline editors contained the full text of the main document instead of just the visible portion. Rewriting the tests to fix these assumptions eliminated a lot of failures.
  * There were many exceptions in matchbrackets.js, apparently due to trying to access text that didn't exist. Turning off matchbrackets resolved almost all the remaining unit test failures. This needs further investigation.
  * After those fixes, there were two remaining unit test failures related to undo/redo from inline editors. These seemed consistent with the bug noted above.
* I also ran the rest of the unit tests. There was one failure in Document related to cleaning up editors, and a few failures in EditorCommandHandlers related to tests in editors with visible ranges. My guess is that the latter are probably due to similar issues with changed assumptions as in the inline editor failures. Needs more investigation.
* Finally, I noted that tokenization seems to be somewhat messed up in inline editors in this branch, which affects code hinting as well. For example, if I open an inline CSS editor and hit return after a property, it doesn't indent, and the color-coding of the previous property name changes; and if I try to bring up code hints in that context, they don't come up. However, if I then delete the contents of the editor and type a new rule, code hints work within that rule--but the selector is color-coded as an error. I suspect there's some issue with tokenizers in subdocuments getting confused. This needs isolation and filing upstream.
  * Despite the above, I was able to verify that (when code hints work) we can remove the hacks due to #1688.

I also found a bug in master: if you have two inline editors open on the same file, with one referring to a rule earlier in the file than the other, and you add lines to the first editor, the line number in the header of the second editor doesn't update (though its line numbers and text ranges do). This should be easy to fix (and isn't really related to the CodeMirror stuff, but needs to be filed).
