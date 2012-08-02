Typing responsiveness
---------------------
**Summary of test results**

* Brackets responds to keystrokes about half as fast as a typical text editor (169 ms vs. avg 83 ms)
* Brackets responds about 50% slower than a bare instance of CodeMirror
* Our native app shell (CEF) does not slow down typing compared to running in the browser
* File size does not seem to have a strong effect on typing speed _(preliminary data)_
* Having the JSLint panel open slows down typing by 30% (presumably also true of the Find in Files dialog). Unclear whether this is due to layout, dropshadow rendering, or something else.
* Laying out a bare instance of CodeMirror via flex-box slows down typing by ~18%. The slowdown effect _may_ be stronger in the full Brackets UI.
* Brackets processes key events very quickly when a key is held down (key repeat). Unclear why this doesn't translate into low latency in normal typing too.

**Test methodology**

Typing performance in Brackets is bottlenecked mainly by native browser layout and rendering, not by JavaScript code execution speed. This makes performance hard to measure: it's easy to write JS code that automatically times its own execution speed, but JS is too decoupled from the browser's layout/rendering to accurately record how long that phase takes. Our best attempt at monitoring keystroke responsiveness via JS code reported a response time more than 2.5x faster than reality.

So, believe it or not... the way we've gotten more accurate data is by recording the computer screen (and keyboard) with a high-speed camera. This is painstaking, but the results are dramatically more reliable.

Opening files
-------------
**Summary of test results**
* Average-sized files open in about 1/3 sec
* Large files open in about 1 sec
* Files with very long lines (e.g. minified JS) take dramatically longer to load -- about 7x longer than the unminified version of the same code, in one test
* Brackets opens files a bit slower than most text editors, but there's a lot of variation in how fast other editors are. In particular, some editors are relatively slow on small files while some become very slow on large files (in some cases taking up to 4x longer than Brackets).

Unlike keystroke handling, the speed of opening files is affected by a mixture of JS code execution and native layout/rendering. In limited tests with a high-speed camera, the results recorded via JS code were much closer to the camera's numbers: off by 25-30% for small files, and off by 10% for larger files. The summary above is based entirely on measurements recorded via JS.