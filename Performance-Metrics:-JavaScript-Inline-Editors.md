Metrics
-------
* Time to open an inline editor for a function definition--first inline editor opened (cold start)
    * Start: Quick Edit command is invoked
    * End: Editor and related items list are fully rendered
* Time to open an inline editor for a function definition--first inline editor opened (warm start)
    * Start: Quick Edit command is invoked
    * End: Editor and related items list are fully rendered
* Time to open an inline editor for a function definition--same inline editor opened again
    * Same start/end as above
    * Should be faster due to caching
* Time to switch between multiple definitions of a given function
    * Start: Next Match command is invoked
    * End: Editor and related items list are fully updated
* Time to close an inline editor
    * Start: Close Inline Editor command is invoked
    * End: Inline editor is removed, host editor is fully updated

Note: "cold start" means a clean start of Brackets after a reboot. This is to get a timing that's unaffected by OS caching&mdash;not so much to focus on the use case where people are truly cold-starting Brackets, but rather to get a true timing for the first time the user accesses a given set of files, or if files they've already accessed have been flushed from the OS cache.

Note: This doesn't include typing speed, since we'll be handling that separately as part of our typing speed performance instrumentation/optimizations.

*Question:* Do we want to include other kinds of edits, scrolling performance, etc.? I feel like those are better handled in more generic performance stories that cross both inline and regular editors, since the instrumentation for both cases should be similar. In this proposal I've just focused on issues specific to inline editors.

Test cases
----------
These are various axes along which we should vary our test cases:

* Number of JS files in project
* Number of functions in each file
* Number of lines in function definition
* Number of definitions found for the given function name

Also, most projects with nontrivial JS have some kind of framework included, so that needs to be factored in. For now, we'll include the non-minified version of the framework, since our features don't work well with minified versions yet in any case.

Proposed project profiles based on these axes:

* Small
    * 5 JS files in project
    * 10 functions in each JS file
    * 10 lines in each function definition
    * 1 definition for each function name
    * Non-minified version of [Backbone](http://documentcloud.github.com/backbone/) included
* Average
    * 20 JS files in project
    * 25 functions in each JS file
    * 50 lines in each function definition
    * 3 definitions for each function name
    * Non-minified version of [jQuery](http://jquery.com) included
* Large
    * 100 JS files in project
    * 50 functions in each JS file
    * 200 lines in each function definition
    * 10 definitions for each function name
    * Non-minified version of [Dojo toolkit release, dojo folder, uncompressed files only](http://dojotoolkit.org/download/) included
* Stress
    * Like Large, but with 500 JS files

Note that the "stress" case is intentionally unrealistic&mdash;we don't necessarily expect any users to have projects of this size/complexity (or expect it to be fast), but can use it as a stress test to figure out whether there's pathological slowdown somewhere that grows faster than it should with project size/complexity.

*Question:* Should we try to construct artificial cases that match these profiles, or should we instead just look for real-world projects that very roughly span different levels of complexity?

*Question:* Is the stress case useful?