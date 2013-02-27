# Extension Management Research #

In Sprint 21, we had a story to research [Extension Management Workflow and Metadata](https://trello.com/card/3-research-extension-management-workflow-metadata/4f90a6d98f77505d7940ce88/767). This document has a summary of the results and links to the details. If you're interested in diving straight into all of the details, take a look at the "Detailed Documents" section at the end.

In the interest of truly summarizing what we've come up with, this document will leave out background and details, so do be sure to dive in where you are interested in learning more or helping with implementation.

## Finding and Installing Extensions ##

The [Extension Manager Workflows](https://github.com/adobe/brackets/wiki/Extension-Manager-Workflows) document how users will interact with the extension manager in Brackets.

The wireframe below gives an idea of how the extensions may be presented (the content within the sidebar is the relevant bit, a sidebar may very well not be the home of the extension manager).

![Wireframe of Extension Listing](http://behance.vo.llnwd.net/profiles15/2147647/projects/7290337/613d565250285ac4ed3b10a2c48985cb.png)

* Users will be able to quickly see the most popular extensions (based on ratings and downloads)
* Very speedy (entirely client-side) search is right at the top
* Installing an extension just takes a click on the install button
* More information is available to help the user pick the right extension (longer description, reviews)
* Installation occurs in the background, while the user works
* Our goal is to make extensions restartless, but the details and timing need to be worked out in [the extension API research](https://trello.com/card/5-research-extension-api/4f90a6d98f77505d7940ce88/769)

## Managing Extensions ##

This wireframe shows how the user can manage installed extensions:

![Wireframe showing installed extensions](http://behance.vo.llnwd.net/profiles15/2147647/projects/7290337/b5604ca21362f43c33caece19818a5ce.png)

* Users can disable an extension to temporarily turn it off
* Or, if they're sure they no longer want it, they can remove the extension
* They'll have a way to get help if an extension isn't working properly
* In the future, extensions may offer settings of their own which the user can get to from here
* Users will be able to rate and review extensions, after authenticating with the registry
* Brackets will tell the user when updates are available
  * They can get more information about the updates
  * Ideally, updates will happen without a restart of Brackets

## Creating Extensions ##

* We want to support a workflow that makes it easy to get started writing an extension with a template for a new extension
* Testing extensions will be easy as it is today
* In-development extensions will appear in the extension manager
* When the extension developer is ready to publish their extension, they can do so from the extension manager with the click of a button (authentication is required for this)

The [extension package format](https://github.com/adobe/brackets/wiki/Extension-Package-Format) will be familiar for anyone who has used npm, and easy for anyone who has not. All of the information about the extension is located in a `package.json` file, with optional information in some other files. This allows that information to be stored in a version control system and makes publishing a new version a one click step.

Our goal is for [extensions to be as compatible with Brackets and one another as possible](https://github.com/adobe/brackets/wiki/Extension-Dependencies), even as changes are made. We'll do our best to minimize disruption for Brackets users while ensuring that their editor remains stable.

## The Registry Server ##

The registry server will be set up to quickly deliver information about the extensions and the extensions themselves. It will have a straightforward REST+JSON API for submitting ratings and reviews and publishing extensions, and we will seek to make authentication as easy on the users as possible.

There will also be a management API for ensuring that Brackets users get updates as quickly as possible when out of date or potentially damaging extensions are identified.

# Detailed Documents #

The preceding sections were intended as a quick overview of the results of our research. For background and additional details, you can dive in below:

* [Package Manager research](https://github.com/adobe/brackets/wiki/Extension-Package-Manager-Research): a look into what other package managers do and the good ideas we can reuse.
* [Dependency Management research](https://github.com/adobe/brackets/wiki/Extension-Dependencies)
* [User workflow proposals](https://github.com/adobe/brackets/wiki/Extension-Manager-Workflows)
* [Rough initial wireframes](http://www.behance.net/gallery/Brackets-Extension-Manager-rough-wireframes/7290337) (note that the extension manager will likely *not* be in a sidebar)
* [Server API to support user workflows](https://github.com/adobe/brackets/wiki/Extension-Repository-Server-API)
* [Package Format](https://github.com/adobe/brackets/wiki/Extension-Package-Format)

# Open Questions #

* How do parts of an extension run within Node? (Do we defer this until the Extension API research?)
* Management of uploaded extensions (pulling an extension, for example). Just contact Brackets team for now?
* How do we reconcile the module format difference between node/npm and Brackets?
* Added a couple of new things: Brackets Updates with incompatible extensions, and the ability for old Brackets to get old extensions

# Candidate Stories #

These are in descending order of likely priority.

* Extension publishing
  * UI that lists development extensions with "Publish" button
  * Validation of extension
    * Required package.json fields
    * No required files missing
    * Version number different from previously published extension
  * Authentication for server
  * Server side to handle publishing
* Extension install
  * Given a URL to an extension package, install it
  * Checks for compatibility with Brackets and peer dependencies
* Extension Listing
  * Compact view and detailed view
    * Detailed view provides optional links to homepage and bug reporting
  * Secured Markdown formatting of description
  * Search
  * Filter by keyword
  * See more by author
  * Sort by downloads, alphabetical
  * Server side of listing
    * Supports retrieval of extension listing based on Brackets API version
  * One click install
* Extension updates
  * Can automatically disable extensions that are found to be damaging
  * Prompts for restart for extensions that require a restart
  * Displays changelog entries since installed version
  * Warns users of extensions that will be disabled because of compatibility issues
* Ratings and reviews
  * Requires authentication to submit review
  * Integrate ratings/reviews into listings
  * Change default sorting to include ratings along with download popularity
  * Required server side changes
* Improved discoverability
  * Look at languages in project and suggest extensions based on that
* Create an Extension workflow
* Synchronize settings and extensions between Brackets instances
* Share extension via Twitter, Facebook
* Extension collections
  * One click "distributions" of extensions
* View extensions on the web

# Deferred Questions #

The answers to these questions will be resolved at implementation time.

* Will the extension manager live in a sidebar, popover or full frame?
* What will the login workflow be like? does the login need to time out? can we safely implement a flow within Brackets that authenticates with other services?
* Is there enough space for an update count badge on the extension manager icon?
* How do we present extension descriptions securely? (allow Markdown formatting, but escape embedded HTML?)