# Live Preview URL Mapping

## Introduction

Live Preview currently opens the local page directly in (Chrome) browser using the file protocol, as in file:///path/to/index.html. This limits development to only document-relative (<img src="img/photo.jpg" />) and absolute URLs (<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.8.1/jquery.min.js"></script>) and client-side HTML documents.

This proposal will allow Live Preview to open pages under a local server so paths that have to be interpreted by a server such as site-root-relative (<img src="/img/photo.jpg" />) and protocol-relative URLs (<script src="//ajax.googleapis.com/ajax/libs/jquery/1.8.1/jquery.min.js"></script>) and server-side file extensions (such as .php) can be used in Live Preview. Also, .shtml pages with include files can also be used in Live Preview.

The backlog entry is [in Trello](https://trello.com/card/3-url-mapping-for-live-development/4f90a6d98f77505d7940ce88/664).

## Specify Base URL

A Gear icon will be placed next to Project name (and drop down menu). Clicking this icon opens a preferences dialog.

## Preferences Dialog

Dialog will have the following fields:

Two radio button options (file URL, custom base URL) and text field (specify base URL)

1. Base URL - this maps to base URL on local server

default is blank which ...

   * Validate that local file URL exists

2. File Extensions - comma separated list of server-side file extensions to recognize



) and text field (specify base URL)
.shtm/.shtml, .php/.php3/.php4, .cfm/.cfml, .asp/.aspx, .jsp, etc. What is reasonable default list?



Base URL maps to the root folder of the project
   * Assume server path structure mirrors folder structure on disk


It would be helpful to have an option to make Live Preview open up documents with a custom local domain such as localhost, as in http://localhost/path/to/index.html.



* Once base URL specified, Live Development button is clickable while any file open (not just .html)
   * Message shown if you click Live Development button with other file type open suggests defining a base URL for local server



## 