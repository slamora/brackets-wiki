Brackets uses some specific coding conventions. All of the pull requests that come in should adhere to the following rules:

* Line length is 80
* Must pass JSLint
* variable and function names must use camelCase (not under_scores)
* use $ prefixes on variables referring to jQuery objects
* use _ prefixes on private variables/methods
* 4-space indents (soft tabs)
* use of Array.forEach() instead of $.each()
* classes and id's in HTML use dashes (-) not camelCase
* use semicolons
* use single quotes, except for writing JSON

## Comments ##
* All comments should be C++ single line style //comment.
* Even multiline comments should use // at the start of each line
* Use C style /* comments */ for notices at the top and bottom of the file
* Annotations should use the /** annotation */
* Annotate all functions