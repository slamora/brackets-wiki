The [Brackets fork of CodeMirror](http://github.com/adobe/CodeMirror2) has a number of fixes and features that we've added in order to support Brackets. We're working with Marijn Haverbeke to get these into the [main CodeMirror master](http://github.com/marijnh/CodeMirror2). 

Merged features
===============

* **Block selection.** Originally, if you selected multiple lines in CodeMirror, the right edge of the selection highlight was "ragged", following the right edge of each selected line. We submitted a [pull request](https://github.com/marijnh/CodeMirror2/pull/322) that makes block selection extend all the way to the right edge of the editor window as in other code editors, and it was merged into master (thanks Marijn!).

We've also had a few smaller bug fixes merged in.

Upcoming features
=================

* **Flicker-free scrolling.** CodeMirror has virtualized scrolling, meaning that it only renders the visible portion of the document (plus some amount of buffer above and below). Currently, this scrolling is flickery in some browsers. We've modified it to reduce flicker, and have [submitted a pull request](https://github.com/marijnh/CodeMirror2/pull/495) based on an improved implementation suggested by Marijn; once that's merged, we'll pull it back into the Brackets fork.

* **Inline widgets.** In order to support the Quick Edit feature in Brackets, we added a way to insert "inline widgets" into a CodeMirror editor. Inline widgets are attached to a particular line, and move/scroll as the document is edited/scrolled. We're planning to submit a pull request for this as soon as the flicker-free scrolling code has been merged.

We've also made a number of smaller bug fixes and changes that we'll submit once those two big ones are out of the way. Going forward, we intend to keep in much closer sync with CodeMirror master.

Future features
===============

One other major feature we'd like to see in CodeMirror is a separation between the model and the view. In Brackets, we can't easily separate our document and editor architectures because the two are unified in the CodeMirror editor; being able to create a text model without view instantiation would be very helpful.