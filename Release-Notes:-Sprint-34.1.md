Release 34.1 addresses two high-priority bugs in the [original Sprint 34 build](https://github.com/adobe/brackets/wiki/Release-Notes:-Sprint-34). No other functionality was added or changed.

* **Fix freeze when opening JavaScript files**: Opening a JavaScript file caused Brackets to freeze if the same folder contained certain other files (non-JavaScript files containing certain text, or large binary files). [See bug #6067](https://github.com/adobe/brackets/issues/6067).
* **Fix UI problems with certain LESS files**: Opening a LESS file that begins in a tag selector (with no header comment, etc. before it) caused the editor UI to go blank or display other rendering glitches. [See bug #6057](https://github.com/adobe/brackets/issues/6057).

_Full change logs:_ [brackets](https://github.com/adobe/brackets/compare/sprint-34...sprint-34-hotfix#commits_bucket) and [brackets-shell](https://github.com/adobe/brackets-shell/compare/sprint-34...sprint-34-hotfix#commits_bucket)