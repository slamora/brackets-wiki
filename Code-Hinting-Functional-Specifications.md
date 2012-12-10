_**This page describes features that are not yet implemented (or not fully implemented). Comments are welcome!**_

The purpose of this document is to describe the functional specification of code hints for a number of important languages. We don't plan to implement all of the functionality specified below at the same time (and indeed may never end up implementing some of it). Instead, the goal is to have a document that can evolve over time.

At any point in time, this document should represent how we currently believe code hinting should work from a functional/user-facing perspective. We can then use this to drive the design of APIs to support code hinting, as well as guide the implementation of actual code hint providers.

#General functionality (all languages)

* The user should be able to _explicitly_ invoke the code hint dialog at any time using a universal (i.e. the same in every mode) key command. By default, that key command should be Ctrl-Space, but should be configurable.
* If the user tries to _explicitly_ invoke the dialog and no hints are available, the user should be notified in an unobtrusive way (e.g., a temporary message in the status bar).
* There should be a universal (i.e. the same in every mode) key command to dismiss the dialog. By default, that key command should be "ESC", but should be configurable.
* Individual code hint providers (e.g. for a specific editing mode) may bring up the dialog _implicitly_ (i.e. without the user explicitly asking for it) when a particular condition is met. For example, in an HTML file, when the user types a "<" character in a location where an HTML tag is valid, code hints for tag names could automatically be displayed.
* Users should have a way to globally disable the _implicit_ display of code hints. (**Open question:** should users be able to override this?)
* The dialog should appear below the user's cursor, and should move with the user's cursor as he types. The list should be filtered / sorted after each character is typed, and there should be an indication of how much the user's typing matches each result (e.g. the matching portion of the string could be made bold).
* There should be a universal (i.e. the same in every mode) key command to accept a code hint. By default, this should be both "tab" and "enter", but should be user configurable. (**Open question:** should this be configurable?)
* If the user types a search string while code hints are being displayed that causes no hints to be available, then the dialog should automatically hide. (**Open question:** An alternative would be do display "no hints available". This would make it more straightforward for the user to re-bring-up the list in the event of a typo: they would just hit backspace until they had a valid query. Individual providers could still explicitly close the dialog if they chose to (e.g. after the user successfully typed an entire identifier by hand).)  

#HTML


#CSS

#JavaScript