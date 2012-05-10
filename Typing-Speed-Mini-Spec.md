### CodeMirror Event Handling
CodeMirror's approach to keyboard event handling uses a hidden textarea for text input <http://codemirror.net/doc/internals.html>. For typing performance, the most significant point to observe is that CodeMirror uses a short polling interval to detect changes to the textarea once the input event has fired.

### Profiling with Web Inspector
Measuring typing performance manually is straightforward with Web Inspector's Timeline panel. The _input_ event appears nested under the _keypress_ event. This _input_ event marks the starting time.

![Web Inspector Timeline](https://github.com/adobe/brackets/wiki/screenshots/performance-typing-webinspector-thumb.png)

There are multiple milestones in CodeMirror's event handling. Line-patching related DOM changes happen early. Other DOM changes and inspection happen later such as scrolling, gutter updates, etc. Often the typed character(s) will get repainted by the browser quickly, however, related changes to the DOM cause following keyboard input processing to lag.
### Measurement
Our current approach to measurement relies on the following:

1. DOM input event - The same event CodeMirror uses on it's hidden textarea. This is our start time.
2. webkitRequestAnimationFrame - Triggered before each repaint. Since this can vary based on the application structure (e.g. CSS style rules, re-layout, etc.), we record significant repaints relative to CodeMirror's event handling: (1) first repaint after _input_, (2) last repaint before CodeMirror's _onChange_ event and (3) first repaint after CodeMirror's _onChange_ event.
3. CodeMirror onChange event - Triggered when all DOM updates are complete. This is more than just line patching to display characters, this includes scrolling, updating inlines, etc.

#### Alternatives and Trade-Offs
1. Instrumenting CodeMirror internals - To keep Adobe's fork of CodeMirror as close as possible to mainline, we've decided against instrumenting CodeMirror. While this detailed level of instrumentation may be helpful, we are more concerned with the bigger picture, response time between keystrokes. The measurements mentioned above give us that data without any changes to CodeMirror.
2. console.profile() - Again, this is more information than we need. Also, profiling significantly degrades performance. Our event handling and requestAnimationFrame approach has a negligible impact on performance.

#### Conditions That Affect Typing Speed
* Words per minute (WPM) ranging from 20 to 80 (1 word = 5 characters)
* Native OS keyboard settings: repeat rate and delay until repeat
* Size of document
    * Gutter changes to add/remove line numbers
* Size of viewport
    * Text changes that trigger scrolling
    * Window/Editor size
* Line wrapping
* Syntax highlighting
    * Well-formed documents
    * Mixed content
* CodeMirror pollingInterval
* Number of inline widgets
* Number of keyboard shortcuts
* Electric chars (auto indenting matching braces)
* Auto indenting (smartIndent setting)
* If you are [NJ](https://github.com/njx)

### Manual Testing
See https://groups.google.com/d/topic/brackets-dev/hagQVKGGuPM/discussion.

### Automated Testing
Automated tests will be written as Jasmine specs. We don't currently have build automation of any sort. For now, we intend to manually run these tests on dedicated test machines.

To reliably simulate typing, we use setTimer() at an appropriate WPM interval to change the hidden textarea value. This triggers input events for CodeMirror.

#### Test Case Outline

_Note: We'll likely add test cases in phases based on the permutations of the conditions above. The outline below serves as an initial set of test cases include general tests as well as known scenarios that we wish to investigate as of Sprint 8. Test media will be located in TBD._

* General Tests for both full editors and inline CSS editors
    * Typing a single char
    * Typing a sequence of 80 chars (WPM from 20 to 80)
    * Deleting a single char
    * Deleting a sequence of 80 chars (WPM from 20 to 80)

### References
* Brackets typing speed thread <https://groups.google.com/d/topic/brackets-dev/tXQ0FkHge0s/discussion>
* How JavaScript Timers Work <http://ejohn.org/blog/how-javascript-timers-work/>
* Why I Consider setInterval to be Harmful <http://zetafleet.com/blog/why-i-consider-setinterval-harmful>