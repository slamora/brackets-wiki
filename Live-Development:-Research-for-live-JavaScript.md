This page documents research results for building out live JavaScript development. We're starting with work [using the APIs built into Chrome](https://trello.com/card/2-research-chrome-implementation-for-live-javascript/4f90a6d98f77505d7940ce88/837).

## Issues with Feature Behavior ##

* Using the [TodoMVC](http://todomvc.com) example
    * The footer blinks a lot when the ScriptAgent is turned on (even when no edits have been made to the script)