This page documents research results for building out live JavaScript development. We're starting with work [using the APIs built into Chrome](https://trello.com/card/2-research-chrome-implementation-for-live-javascript/4f90a6d98f77505d7940ce88/837).

## Issues with Feature Behavior ##

* Using the [TodoMVC](http://todomvc.com) jquery example
    * The footer blinks when the ScriptAgent is turned on (even when no edits have been made to the script). Even more noticeable with the Backbone example
    * The Live Development icon looks like it's turned off
    * Why does reload take a long time when I save a .js file vs. when I save the HTML file?
    * Adding a method to the object does not work (can't find the method)
    * Adding a function to the closure does not work (can't find the function)
        * further, the error displayed on the console didn't even reflect the full name of the function it couldn't find. It's as if the engine stopped updating.
        * Just tried this again with a function that already existed... and it chopped off the last character of the function name both times!
    * needs more looking:
        * moved some functionality into a function
        * commented out the call to that function
        * alert call showed that it was still calling that function
    * changing a variable's value set in a closure does not work (`var foo = 1000;` changed to `var foo = 10;`)
* Using a [canvas demo](https://github.com/chrislongo/html5-canvas-demo)
    * The framerate does *not* appear to be affected by having the ScriptAgent loaded (I had a theory about the blinking in TodoMVC. Turned out to be false)
    * not surprisingly, making a change to `initPalette` live has no effect
    * changing `smooth`, on the other hand, works fine
    * changing a script in the HTML page has no effect
    * adding code outside of a function block in the script file has no effect
