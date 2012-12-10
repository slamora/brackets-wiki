_**This page describes features that are not yet implemented (or not fully implemented). Comments are welcome!**_

The purpose of this document is to describe the functional specification of code hints for a number of important languages. We don't plan to implement all of the functionality specified below at the same time (and indeed may never end up implementing some of it). Instead, the goal is to have a document that can evolve over time.

At any point in time, this document should represent how we currently believe code hinting should work from a functional/user-facing perspective. We can then use this to drive the design of APIs to support code hinting, as well as guide the implementation of actual code hint providers.

#General functionality (all languages)

* The user should be able to _explicitly_ invoke the code hint dialog at any time using a universal (i.e. the same in every mode) key command. By default, that key command should be Ctrl-Space, but should be configurable.
* If the user tries to _explicitly_ invoke the dialog and no hints are available, the user should be notified in an unobtrusive way (e.g., a temporary message in the status bar).
* There should be a universal (i.e. the same in every mode) key command to dismiss the dialog. By default, that key command should be "ESC", but should be configurable.
* Individual code hint providers (e.g. for a specific editing mode) may bring up the dialog _implicitly_ (i.e. without the user explicitly asking for it) when a particular condition is met. For example, in an HTML file, when the user types a "<" character in a location where an HTML tag is valid, code hints for tag names could automatically be displayed.
* Users should have a way to globally disable the _implicit_ display of code hints.
 * **Open question:** should users be able to override this?
* The dialog should appear below the user's cursor, and should move with the user's cursor as he types. The list should be filtered / sorted after each character is typed, and there should be an indication of how much the user's typing matches each result (e.g. the matching portion of the string could be made bold).
* There should be a universal (i.e. the same in every mode) key command to accept a code hint. By default, this should be both "tab" and "enter", but should be user configurable.
 * **Open question:** should this be configurable?
* If the user types a search string while code hints are being displayed that causes no hints to be available, then the dialog should automatically hide.
 * **Open question:** An alternative would be do display "no hints available". This would make it more straightforward for the user to re-bring-up the list in the event of a typo: they would just hit backspace until they had a valid query. Individual providers could still explicitly close the dialog if they chose to (e.g. after the user successfully typed an entire identifier by hand).

#HTML

* Should support the completion of:
 * tag names
 * attribute names (only listing attributes valid for the current tag)
 * enumerated attribute values (only listing values valid for the current attribute
 * attribute values that correspond to something externally defined (e.g. CSS class and ID names).
* The code hint dialog should be implicitly (automatically) displayed at the following times:
 * the user types a "<" character in a location where a new tag is valid (tag name hints should be displayed)
 * the user types a "space" character after a tag name, attribute value, or attribute name w/o a value (such as "checked" (attribute name hints should be displayed)
 * the user types a "=" character after an attribute name (attribute value hints should be displayed)
* If the user completes an attribute value that requires quotes, and the user has not explicitly typed quotes, the string should be surrounded by double quotes automatically. If the user has already typed a quote (either single or double), the quote type should be preserved.
* When completing strings that optionally have quotes, the query should be robust to the user adding a quote. For example, suppose a file contains ```<div id=``` and the user's IP is after the "=" character. If the user invokes the code hint dialog, it should stay visible if the user next types a quote character.
* Sub-mode autocomplete should work (e.g. CSS hinting should work in ```<style>``` tags)

#CSS

* Should support the completion of:
 * enumerated selectors (i.e. html tag names)
 * class and IDs for selectors where the class or ID appears in an HTML file
 * rule names
 * enumerated rule values
 * font family names from a generally accepted list of safe fonts. Extensions may extend this list.
 * color values used elsewhere in the file
  * **Open Question:** Do we want to provide autocomplete support for pseudo-selectors/pseudo-classes. If so, which ones, and how would it work?
* Code hints should be implicitly (automatically) displayed:
 * After the user types ": " immediately after a rule name. (rule values should be displayed)
 * After the user autocompletes a rule name (which should automatically insert the ": as well) (rule values should be displayed)
 * After the user types ";[return]" after completing a rule (rule name completions should be displayed)
 * After the user types ", " in a comma-separated list of values (like font families) (rule values should be displayed
 * After the user types a space in a space-separated list of values (e.g. border shorthand) (rule values should be displayed)
 * After the user types a ", " in a comma-separated list of selectors (selectors should be displayed)
  * **Open Question:** Should we do this? If so, should we also implicitly display selector hints after the user types "}[return]"
* When the user completes strings that require quotes, they should automatically be added exactly as in HTML.
 * If the user's file already contains ```Helvetica``` (without quotes), and they bring up the completion on Helvetica and choose "Helvetica Neue", quotes should automatically be added.
* **Open Question:** Do we want to provide any support for media queries? If so, how would it work?

#JavaScript

* Should support the completion of:
 * Identifiers in scope
 * Globally defined identifiers (when possible)
 * Property names (when possible)
 * String literals, when they have structure (such as selectors in a jQuery statement) (when possible)
  * Doing this well will be really really hard. :-)
 * **Open Question:** Do we want to support completion of keywords (e.g. "function")? If so, should we do this only _explicitly_ (e.g. when the user types "fun" and then presses ctrl-space)? Or should we also do it _implicitly_ (e.g. the user types "f" in a place where "function" is a valid keyword, so we automatically show  code hints)
* Code hints should be implicitly (automatically) displayed when:
 * The user types any valid start-of-an-identifier character (e.g. [A-Za-z$_, etc.]) in a place where an identifier is valid. (identifiers should be displayed)
 * The user types "." or "[" in a place where a property name is valid (property names should be displayed)
 * The user types special sequences of characters, such as ```$("``` that indicate string literals of a specific type will be entered (e.g. CSS selectors)
  * Doing this well will be really hard
* Code hints should be closed when the user types a character that is NOT a valid part of an identifier/property name. E.g., if ```foo``` is in scope, and the user types "f" (in a place where an identifier is valid), then hints should be automatically displayed and should contain "foo". But, if the user presses space (thus deciding on "f" as an identifier) then the code hint list should be hidden
 * **Open Question:** In the above situation, if the user then presses backspace, should the list reappear?
* Search for identifiers/properties should be "fuzzy", much like quick open. For example, if there is an identifier named ```getValue``` and the user types ```val```, then "getValue" should appear in the list (possibly ranked lower than other literals that start with "val").
* **Open Question:** We should consider how it would work to provide hints for function call parameters for known library functions. For example, what could we offer if the user has ```$.ajax(``` before his IP?

# Feature-specific Code Hinting

In addition to providing the general language support above, extension authors may wish to provide more "targeted" code hinting for a specific part of a language. For example, the user might want to install a code hint plug in that provides color hints in CSS taken from their [Kuler](https://kuler.adobe.com/) profile.  So that we can better define our APIs to enable this, we detail a few functional specs here.

## [Edge Web Fonts](https://github.com/adobe/brackets-edge-web-fonts) Code Hints

* When the user is typing font family values, this provider should override the default CSS hinter to provide Edge-Web-Fonts-specific suggestions.
 * **Open Question:** Should we merge the results from the specific and general providers? Joel votes "no" -- the specific provider should always win if it offers any suggestions. Ultimately, we could imagine allowing users to specify precedence of providers, but we should probably wait until this becomes a problem to design a solution.
* At the bottom of the code hint list, there should be a link to browse for additional fonts. This should open up a dialog to select new fonts.

## Other possible needs

* Specific providers may want to put other "types" of items in the list. For example, they may want to put image previews that autocomplete to image paths.