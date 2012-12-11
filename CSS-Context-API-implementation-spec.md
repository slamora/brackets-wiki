In Brackets we have an HTML utility API ``getTagInfo(editor, cursorPosition)`` to provide the context of an HTML tag and is already used by tag hinting and attribute hinting. However, we do not have a similar CSS utility API to provide CSS rule information. So this document specifies the new CSS utility API for the following goals.
* To avoid duplicate effort in detecting CSS context by individual extension provider.
* To avoid inconsistent or incomplete implementation of CSS context detection.
* To provide a maintainable, scalable implementation of CSS context that has complete coverage of all possible CSS context.

## getRuleInfo ##
The new API will be defined as getRuleInfo(editor, cursorPos). It takes two arguments -- editor object and cursor position, and returns a rule infomation object.
The rule information is defined as follows...
```
{ 
  selector:
      { index: currentIndexInValues,
        values: [selector1, selector2, ...] },
  prop:
      { name: propName,
        index: currentIndexInValues,
        values: [propValue1, propValue2, ...] },
  position:
      { tokenType: tokenType,
        offset: offsetInCurrentToken } 
}
```

```tokenType``` is either an empty string or one of the following values.
 * **PROP_NAME** - will implement in sprint 18 for CSS hinting and font hinting
 * **PROP_VALUE** - will implement in sprint 18 for CSS hinting and font hinting 
 * **SELECTOR** - not plan to implement it in sprint 18
 * **MEDIA** - may support in the future with some modification to the rule info structure
 * **CHARSET* - may support in the future with some modification to the rule info structure

The value of ```tokenType``` is an empty string for the following context.
 * Current cursor position is in a non-css/non-less document
 * Current cursor position is within a not-yet-supported or unsupported context
 * Current cursor position is within an invalid context

###Default values of rule information###
If the cursor is in a non-css/non-less document, or inside the unsupported context of a css/less document a rule info with the following values is returned.
```
{ 
  selector:
	{ index: -1,
      values: [] },
  prop:
    { name: "",
      index: -1,
      values: [] },
  position:
    { tokenType: "",
      offset: 0 } 
}
```

###Examples of Not-Yet Supported CSS Context###
>@charset "UTF-8";
>
>@import url("booya.css") print,screen;
>@import "whatup.css" screen;
>@import "wicked.css";

>@namespace "http://www.w3.org/1999/xhtml";
>@namespace svg "http://www.w3.org/2000/svg";

###Examples of Property Name context###
PROP_NAME Token Type
* div {|
* ```
div {
	|
```
* ```
div {
	|clip:
```
* ```
div {
	clip|:
```
* ```

###Examples of Invalid CSS Context###
*div {
	clip |:
```
*div {
	clip |both
```

###Examples of Property Value context###
*
*
*

