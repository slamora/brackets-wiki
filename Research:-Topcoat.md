Research task to determine the set of work required to enhance and standardize the Brackets user interface with [Topcoat](https://github.com/topcoat/topcoat).

##Criteria
- Plan interface design and implementation work.
- Develop multiple stories to complete interface work.


#Working prototype of Topcoat design applied
[Topcoat Parity Branch](https://github.com/adobe/brackets/tree/garthdb/topcoat-parity)

I did minimal changes to js - here are the remain changes that need to be made.

###Project Panel:
* The triangle on the selected state in the project panel does not show up when the selected file is the final item in the list.
* The resizing works on the panel, but the resizer needs to be adjusted so it changes the padding of the #content div on resize.

###Toolbar:
* Icons should be 16x16.

###Statusbar:
* Clickable items need to be distinct from informational ones.  I put a border around the currently interactive parts to make it slightly more button like.

###Bottom Panel:
* The panels are still in the #content div, it isn't hard to move it out, but the resizer needs to be adjusted again to resize padding the #content div based on the panel height.


#Audit components needed from Topcoat components
Here's what we need now:

* **Modal dialogs** (currently no design, no styles)
* **Buttons**
	* [Call to action](http://topcoat.io/topcoat/src/desktop.html#call-to-action-button)
	* [Standard](http://topcoat.io/topcoat/src/desktop.html#button)
* **Panels** both bottom and right side (currently no design, no styles)
* **[Text input](http://topcoat.io/topcoat/src/desktop.html#text-field)**
* **File Tree** (or just a tree in general)
	- Collapsable 
	- Nested elements

Soon we'll need:

* Form inputs when we get to preferences:
	- **[Checkbox](http://topcoat.io/topcoat/src/desktop.html#checkbox)** (current there is an intial design, no styles)
	- **[Select](http://topcoat.io/topcoat/src/desktop.html#select)**
	- **Text area** (currently no styles)
	- **[Radio](http://topcoat.io/topcoat/src/desktop.html#radio-button)** (currently there is an initial design, no styles)
* **[Breadcrumbs](http://topcoat.io/topcoat/src/desktop.html#breadcrumbs)** (currently there is an initial design, no styles)

#Assess the dependency on Bootstrap
In progress.

#Identify opportunities for animation.

* Animate inline editor open and close
* Animate file tree open close