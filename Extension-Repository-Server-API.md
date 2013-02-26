Server API requirements to support [the workflows](https://github.com/adobe/brackets/wiki/Extension-Manager-Workflows).

# Requirements #

## General ##

* A REST+JSON API
  * Focused on use within Brackets
  * There may be a web-facing interface of some sort later
  * Clients other than Brackets/Edge Code are *not* important
  * Consequently, level 2 on the [Richardson Maturity Model](http://martinfowler.com/articles/richardsonMaturityModel.html) is likely about right
* Everything a user needs to be able to find and install extensions should be available without authentication
* Authenticated APIs **must** use https
  * Server certificate should be verified to avoid man in the middle attacks (in other words, don't turn this feature off)
* Authentication information **must not** be sent over http with unauthenticated requests

## Listing ##

* Provides everything the Extension Manager needs to display the list of extensions
* Includes all extensions with an "active" state
* Fields needed:
  * ID of extension (used for constructing download URL, for example)
  * display name of extension
  * name of the extension author
  * date of last update
  * download count (possibly total and "recent" (last week, perhaps) to help with sorting)
  * version number
  * min/max Brackets API version
  * peer dependency requirements (if peer dependencies are being implemented)
  * a short description
  * rating
  * thumbnail URL (if available)
  * homepage URL
  * support URL
  * a flag for "has been disabled"
  * dependencies
* if an installed extension is marked "has been disabled", then it will be turned off automatically
* unauthenticated

## Download ##

* Downloads the extension package
* Allows downloading older extension versions
* unauthenticated

## Detail ##

* Allows the user to retrieve more detailed information about an extension
* Fields needed:
  * Long description
  * URLs for additional screenshots if any
  * changelog
  * Ratings breakdown (number of ratings at each count of stars)
  * Reviews
* unauthenticated

## Review ##

* Submits a rating (and possibly review) for a an extension
* Replaces a previous rating/review by the same user if there was one
* **authenticated**

## Report ##

* Used to report that an extension is malicious or causing significant harm
* User can provide comments with further detail about the abuse
* Will send email to the Brackets team
* Sets the state to "pending disable"
  * extensions that are "pending disable" do not appear in listings
  * "pending disable" does *not* set the "has been disabled" flag in the listings
  * someone from the Brackets team will investigate and change the state to disabled if they determine the extension causes harm
  * "disabled" extensions are turned off on end user computers automatically
* **authenticated**

## Publish ##

* Enables an extension developer to upload an extension
* Can update an existing extension that the developer owns
* Uploads a package with metadata (package format TBD)
* **authenticated** and **authorized** only for the owner of the extension (or any user, if it's a new extension name)

## Manage ##

* Allows Brackets team members to change the settings for an extension
* Can set the state (between active, unlisted, pending disable and disabled)
  * pending disable and unlisted both result in the extension not being listed to users
  * the difference is that we may want to be able to remove crufty old extensions from the listing, but that is not representative of abuse
* Can set the Brackets API max version
* **authenticated** and **authorized** only for Brackets team members

# Strawman APIs #

These notes were to help think through the API requirements. They may or may not reflect the desired approach once implementation starts, and were written before the requirements above so they are certainly incomplete.

The overall idea is a typical REST+JSON approach.

## Unauthenticated API ##

### GET list ###

`list` is called once per day (or when the user forces a refresh) by Brackets. It provides a list of extensions with all of the data needed to provide:

1. update notifications
2. listings for the Find tab of the Extension Manager

There may come a time when this is too much data for the client to request every day. With the data gzipped, however, we can go for some time before this would be an issue.

Output example:

```javascript
[
    {
        "name": "HoverPreview",
        "title": "Hover Preview",
        "description": "Shows a preview when the cursor is hovered over certain items",
        "version": "0.4",
        "screenshot": "http://s3.aws.blah.blah/8675309jennyabcdef",
        "rating": 4.5,
        "downloadsAll": 352122,
        "downloadsRecent": 1021
    },
    {
        "name": "html-templates",
        "title": "HTML Templates",
        "description": "Insert a chosen HTML template into the current file",
        "version": "1.2",
        "rating": 4.0,
        "downloadsAll": 121211,
        "downloadsRecent": 522
    }
]
```

### GET extension/{{name}}/latest/download ###

Downloads the package for the latest version of the named extension. (See package format for details about the response.)

### GET extension/{{name}}/moreinfo ###

Retrieves detailed info about the extension.

```javascript
{
    "name": "HoverPreview",
    "title": "Hover Preview",
    "description": "Shows a preview when the cursor is hovered over certain items",
    "longDescription": "Long, *markdownable* description...",
    "version": "0.4",
    "screenshot": "http://s3.aws.blah.blah/8675309jennyabcdef",
    "moreScreenshots": ["http://url". "http://anotherurl"]
    "rating": 4.5,
    "ratingDetail": [20, 1, 3, 150, 225]
    "downloadsAll": 352122,
    "downloadsRecent": 1021,
    "reviews": [
        {
            "rating": 4,
            "title": "Finally!",
            "review": "I've been waiting for this day for the past 25 years."
        }
    ],
    "homepage": "https://github.com/gruehle/HoverPreview",
    "support": "https://github.com/gruehle/HoverPreview/issues",
    "recentChanges": [
        {
            "version": "0.4",
            "description": "Less soup."
        },
        {
            "version": "0.3",
            "description": "Rockets? Yeah, it's got rockets."
        }
    ]
}
```

## Authenticated API ##

**All access to authenticated APIs will be via HTTPS.**

Access to the APIs in this section require the user to be logged in.

Ideally, users will be able to use existing logins rather than learning a new password (GitHub, Twitter, Facebook, Google), but starting with a username/password is reasonable. Note: [Passport](http://passportjs.org/guide/) may be one implementation strategy.

As far as authentication goes, we can start as simple as basic auth and move onward to oauth ourselves (user authorizes the copy of Brackets and a token is sent that is kept by the client). oauth "scopes" would provide a standardized mechanism for us to authorize clients to rate/review extensions without the ability to upload extensions. oauth is not the only way to do this, but it is a standard.

### POST extension/{{name}}/review ###

Logs a review for the given extension. If the user has already posted a review for this extension, it is replaced with the new one. The POST body is a JSON object with the review information. Example:

```javascript
{
    "rating": 4,
    "title": "Fish!",
    "review": "It ate my fish. I didn't like that fish anyway, though."
}
```

`rating` is a required integer from 1 to 5. `title` is an optional review title and `review` is the optional review text. A review can include only a `rating` and `title`, but if `review` is present, `title` must be present, too.
