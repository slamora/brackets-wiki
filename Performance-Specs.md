## Typing Speed (WIP)
### CodeMirror Event Handling
CodeMirror's approach to keyboard event handling uses a hidden textarea for text input <http://codemirror.net/doc/internals.html>. For typing performance, the most significant point to observe is that CodeMirror uses a short polling interval to detect changes to the textarea once the input event has fired.
### Profiling with Web Inspector
Measuring typing performance manually is straightforward with Web Inspector's Timeline panel. The _input_ event appears nested under the _keypress_ event. This _input_ event marks the starting time.
![Web Inspector Timeline](https://github.com/adobe/brackets/wiki/screenshots/performance-typing-webinspector-thumb.png)
There are multiple milestones in CodeMirror's event handling. DOM changes to the line structure happen early. Other DOM changes/inspection happen later such as scrolling, gutter updates, etc. Often the typed character(s) will get repainted by the browser quickly, however, related changes to the DOM cause following keyboard input processing to lag.
### Measurement
Our current approach 
Here are the tools we have for measuring typing speed in CodeMirror. Note that the tools I'm considering are suitable for automated performance tests.

1. CodeMirror onKeyEvent - DOM keydown/keypress events
2. DOM input event - triggered by CodeMirror's hidden textarea
3. CodeMirror onUpdate event - triggered when all DOM updates are complete. This is more than just line patching to display characters, this includes scrolling, updating inlines, etc.
4. webkitRequestAnimationFrame - triggered before each repaint
5. DOM mutation events - triggered by DOM changes. Note that these events are now deprecated.

#### Automation
#### Alternatives and Trade-Offs
### Testing
#### Conditions That Affect Typing Speed
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
* Electric chars
* Auto indenting
* If you are [NJ](https://github.com/njx)

### References
* Typing speed thread: <https://groups.google.com/d/topic/brackets-dev/tXQ0FkHge0s/discussion>