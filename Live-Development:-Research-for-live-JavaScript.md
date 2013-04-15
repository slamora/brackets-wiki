This page documents research results for building out live JavaScript development. We're starting with work [using the APIs built into Chrome](https://trello.com/card/2-research-chrome-implementation-for-live-javascript/4f90a6d98f77505d7940ce88/837).

## Issues with Feature Behavior ##

* Using the [TodoMVC](http://todomvc.com) jquery example
    * The footer blinks when the ScriptAgent is turned on (even when no edits have been made to the script). Even more noticeable with the Backbone example
    * The Live Development icon looks like it's turned off
* Using a [canvas demo](https://github.com/chrislongo/html5-canvas-demo)
    * The framerate does *not* appear to be affected by having the ScriptAgent loaded (I had a theory about the blinking in TodoMVC. Turned out to be false)
    * not surprisingly, making a change to `initPalette` live has no effect
    * changing `smooth`, on the other hand, works fine
    * changing a script in the HTML page has no effect
    * adding code outside of a function block in the script file has no effect
