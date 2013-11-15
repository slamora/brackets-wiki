## References

* Upgrade Guide http://codemirror.net/4/doc/upgrade_v4.html
* User Manuals [v4 manual](http://codemirror.net/4/doc/manual.html) and [v3 manual](http://codemirror.net/doc/manual.html)
* Multiple cursor demo http://codemirror.net/4/demo/multiselect.html

## Notes

### Nov 15 - Jason


### Nov 14 - NJ

Note that even basic edits get broken with multiple selections if you have JS code hints enabled (or probably any code hinter—I haven’t tried in HTML or CSS). If you remove the JS code hints extension, basic multi-selection edits seem to work. I didn’t try any other Brackets-specific functionality like move line up/down, etc. or extensions.

In general, the existing CM APIs are supposed to act on just the “primary selection” (which is probably either the first or last selection, not sure), so it’s not clear why stuff in Brackets would be badly broken (the only APIs that have special behavior are getSelection()/replaceSelection(), which I don’t think we use much); in general, it seems like Brackets edits should just act on one of the selections. But with code hints on things get corrupted pretty quickly.

Also, aside from figuring out what is/isn’t broken in Brackets, it would be good to do a comparison of Sublime’s behavior with multiple/rectangular selections on various edits with what CM implements (or in the context of Brackets with the new CM) to see if there are any differences. I haven’t really used multiple selections at all in other tools, so I don’t really have an intuitive sense of what users’ expectations are, but I figure if we’re the same as Sublime it’s certainly at least good enough :) (“WWSD?”)