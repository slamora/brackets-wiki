CSS Context Utility API Specification

In Brackets we already have an HTML utility API ``getTagInfo(editor, cursorPosition)`` to provide the context of an HTML tag and is already used by tag hinting and attribute hinting. However, we do not have a similar CSS utility API to provide CSS rule information. In the absent of this API we have the following issues for each CSS hint provider.
* Each CSS hint provider has to implement its own code to detect the CSS context.
* Implementation may not consistent and may have some of its own bugs or issues.

## getRuleInfo ##
This API takes two arguments -- editor object and cursor position and it returns a rule infomation object.
The rule information is defined as follows...
```
{ 
  selector:
	{ index: -1,
      values: [selector1, selector2, ...] },
  prop:
    { name: propName || "",
      index: -1,
      values: [propValue1, propValue2, ...] },
  position:
    { tokenType: tokenType || "",
      offset: offset || 0 } 
}
```

```tokenType``` is an empty string if the current cursor position is within an unsupported context location, or an enum that has one of the following string values if the cursor is inside a supported context.
* **PROP_NAME** - will implement in sprint 18 for CSS hinting and font hinting
* **PROP_VALUE** - will implement in sprint 18 for CSS hinting and font hinting 
* **SELECTOR** - not plan to implement it in sprint 18
* **MEDIA** - may support in the future with some modification to the rule info structure
* **CHARSET* - may support in the future with some modification to the rule info structure

###Default values of rule information###
If the cursor is in a non-CSS and non-less document, or inside the unsupported context of a css/less document a rule info with the following values is returned.
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

