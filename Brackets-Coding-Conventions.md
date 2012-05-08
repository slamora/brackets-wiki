Brackets uses some specific coding conventions. All of the pull requests that come in should adhere to the following rules:
## Basics ##
* Line length is 80
* Must pass JSLint

## Naming and Syntax ##
* variable and function names must use camelCase (not under_scores)
* use $ prefixes on variables referring to jQuery objects
* use _ prefixes on private variables/methods
* classes and id's in HTML use dashes (-) not camelCase or under_scores
* use semicolons
* use double quotes in JavaScript

## Code Structure ##
* Closure vars should go at the top of the code block they're scoped to
* use of Array.forEach() instead of $.each()
* 4-space indents (soft tabs)

## Comments ##
* All comments should be C++ single line style //comment.
* Even multiline comments should use // at the start of each line
* Use C style /* comments */ for notices at the top and bottom of the file
* Annotations should use the /** annotation */
* Annotate all functions