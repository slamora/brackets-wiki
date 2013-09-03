Notes for **[Extensibility API simplification](https://trello.com/c/iSG75qG4/995-research-extensibility-api-simplification)** research story.

## Commands, Menus, Key bindings

## Documents & Editors

* Proxy objects ("CurrentDocument," "ActiveEditor," etc.) that you can attach listeners to once, instead of writing boilerplate code to attach & detach listeners as the current/active Document/Editor changes.
* Events for idle document-change processing (either a debounced delay after typing pauses, or on open/save/refresh as linting works today). Useful for Find in Files results updating, real-time linting, possibly some live development functionality, etc.

## ProjectManager & file structure

## UI: toolbars, icons, panels, etc.

## Inline editors

## Language extensibility

(See also [Language Extensibility umbrella user story](https://trello.com/c/D2L7SAq5/639-language-extensibility-apis-for-extensions-to-add-new-language-syntax-coloring-mode-rich-editing-support))

## Quick Open search

## Find, Replace, Find in Files