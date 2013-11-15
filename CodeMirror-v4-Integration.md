## References

* Upgrade Guide http://codemirror.net/4/doc/upgrade_v4.html
* User Manuals [v4 manual](http://codemirror.net/4/doc/manual.html) and [v3 manual](http://codemirror.net/doc/manual.html)
* Multiple cursor demo http://codemirror.net/4/demo/multiselect.html

## Notes

### Nov 15 - Jason

* Unit test failures
    * EditorCommandHandlers - 8
    * HTMLUtils Code Hints - 27 (see `selAfter` stack trace)
* Integration test failures
* Extension test failures
* Confirmed that cmv4 fires 2 change events. In my JS file test, typing a single char, the change event shows the insertion (good) and a deletion of the inserted char (wha?)
* JS, CSS and HTML code hints are in various states of broken
    * JS - `change` handler is triggered twice. The first time, the inserted char appears in the editor and a hints popup already appears. The second time, the inserted character was somehow removed.
    * CSS - inserting a style name works fine, but value hinting immediately after is broken. Instead of seeing the value hints popup, the next char becomes selected
    * HTML - tag hinting appears to work, but attribute name hinting will delete the next character, yet still show the hints popup. Attribute value hinting is even worse. Inserting a value `ltr` for `dir` attribute in `<div dir=""></div>` actually produces `<div dir="">ltr</div>`

* `selAfter`

```
TypeError: Cannot set property 'selAfter' of undefined
    at setSelectionAddToHistory (file:///C:/Program%20Files/Brackets/dev/src/thirdparty/CodeMirror2/lib/codemirror.js:2720:36)
    at null.<anonymous> (file:///C:/Program%20Files/Brackets/dev/src/thirdparty/CodeMirror2/lib/codemirror.js:5070:7)
    at null.setValue (file:///C:/Program%20Files/Brackets/dev/src/thirdparty/CodeMirror2/lib/codemirror.js:1520:24)
    at null.setValue (file:///C:/Program%20Files/Brackets/dev/src/thirdparty/CodeMirror2/lib/codemirror.js:5317:40)
    at Editor._resetText (file:///C:/Program%20Files/Brackets/dev/src/editor/Editor.js:721:26)
    at new Editor (file:///C:/Program%20Files/Brackets/dev/src/editor/Editor.js:418:14)
    at null.<anonymous> (file:///C:/Program%20Files/Brackets/dev/test/spec/CodeHintUtils-test.js:46:24)
```

### Nov 14 - NJ

Note that even basic edits get broken with multiple selections if you have JS code hints enabled (or probably any code hinter—I haven’t tried in HTML or CSS). If you remove the JS code hints extension, basic multi-selection edits seem to work. I didn’t try any other Brackets-specific functionality like move line up/down, etc. or extensions.

In general, the existing CM APIs are supposed to act on just the “primary selection” (which is probably either the first or last selection, not sure), so it’s not clear why stuff in Brackets would be badly broken (the only APIs that have special behavior are getSelection()/replaceSelection(), which I don’t think we use much); in general, it seems like Brackets edits should just act on one of the selections. But with code hints on things get corrupted pretty quickly.

Also, aside from figuring out what is/isn’t broken in Brackets, it would be good to do a comparison of Sublime’s behavior with multiple/rectangular selections on various edits with what CM implements (or in the context of Brackets with the new CM) to see if there are any differences. I haven’t really used multiple selections at all in other tools, so I don’t really have an intuitive sense of what users’ expectations are, but I figure if we’re the same as Sublime it’s certainly at least good enough :) (“WWSD?”)