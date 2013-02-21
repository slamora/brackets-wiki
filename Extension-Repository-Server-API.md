Proposal for the server API to support [the workflows](https://github.com/adobe/brackets/wiki/Extension-Manager-Workflows).

The overall idea is a typical REST+JSON approach.

# Unauthenticated API #

## GET list ##

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

## GET extension/{{name}}/latest/download ##

Downloads the package for the latest version of the named extension. (See package format for details about the response.)

## GET extension/{{name}}/moreinfo ##

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

# Authenticated API #

Access to the APIs in this section require the user to be logged in. Users will be able to use existing logins rather than learning a new password (GitHub, Twitter, Facebook, Google). (Note: [Passport](http://passportjs.org/guide/) may be one implementation strategy.)