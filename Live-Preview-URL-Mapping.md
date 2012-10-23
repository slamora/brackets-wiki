# Live Preview URL Mapping

## Introduction

Live Preview currently opens the local page directly in (Chrome) browser using the file protocol, as in file:///path/to/index.html. This limits development to only document-relative (&lt;img src="img/photo.jpg" /&gt;) and absolute URLs (&lt;script src="http://ajax.googleapis.com/ajax/libs/jquery/1.8.1/jquery.min.js" &gt;&lt;/script&gt;) and client-side HTML documents.

This proposal will allow Live Preview to open pages under a local server so paths that have to be interpreted by a server such as site-root-relative (&lt;img src="/img/photo.jpg" /&gt;) and protocol-relative URLs (&lt;script src="//ajax.googleapis.com/ajax/libs/jquery/1.8.1/jquery.min.js" &gt;&lt;/script&gt;) and server-side file extensions (such as .php) can be used in Live Preview. Also, .shtml pages with include files can also be used in Live Preview.

The backlog entry is [in Trello](https://trello.com/card/3-url-mapping-for-live-development/4f90a6d98f77505d7940ce88/664) and also [issue #1920 in github](https://github.com/adobe/brackets/issues/1920).

## Base URL

Base URL maps to the root folder of the project. It is assumed that server path structure mirrors folder structure on disk.

## Preferences

Preferences are project-specific.

A Gear icon will be placed next to Project name (and drop down menu). Clicking this icon opens a preferences dialog. Dialog will have the following fields:

1. Two radio button options (File URL, Custom Base URL) and text field (specify Base URL). The base URL maps to base URL of the web site on local server (e.g. http://localhost/path/to/site/root/). This field can be left blank to indicate using actual path to file.

[The radio buttons do not seem necessary. If field is blank, the path to file is used. If URL is specified, then it is used plus project-relative path to file. Can someone explain usage of radio buttons?]

2. Server-side File Extensions - comma separated list of server-side file extensions to recognize in addition to .htm/.html.

Default set of server-side file extensions is: .sthm,.shtml,.php,.cfm,.cfml

[Is this s reasonable set of defaults? Should we allow wildcards such as .shtm* ?]

## Live Preview

Once base URL specified, Live Development button is clickable while any file extension specified in the Preferences dialog, in addition to .htm/.html.

Message shown if you click Live Development button with file open that has a server-side file extension and a Base URL has not yet been specified.

