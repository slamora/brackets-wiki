## Typing Speed (WIP)
### CodeMirror Event Handling
CodeMirror's approach to keyboard event handling uses a hidden textarea for text input <http://codemirror.net/doc/internals.html>. 
### Measurement
Here are the tools we have for measuring typing speed in CodeMirror. Note that the tools I'm considering are suitable for automated performance tests.
1. CodeMirror onKeyEvent - DOM keydown/keypress events
2. DOM input event - triggered by CodeMirror's hidden textarea
3. CodeMirror onUpdate event - triggered when all DOM updates are complete. This is more than just line patching to display characters, this includes scrolling, updating inlines, etc.
4. webkitRequestAnimationFrame - triggered before each repaint
5. DOM mutation events - triggered by DOM changes. Note that these events are now deprecated.
Measurement can also be done manually through Web Inspector.

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
* If you are <https://github.com/njx>