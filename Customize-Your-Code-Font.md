Brackets doesn't officially support a preference for changing the code editor font. But here's a quick hack you can use to work around issues like [lack of Russian/Cyrillic support](https://github.com/adobe/brackets/issues/3465):

* Locate the installed Brackets source:
    * On Mac, right-click Brackets and choose Show Package Contents, then expand the "Contents" folder.
    * On Windows, navigate to your install folder (usually _C:\Program Files (x86)\Brackets_).
* Edit the file <i>www/styles/brackets_theme_default.less</i>
    * On Windows, due to UAC, you may need to copy this file to another location, edit it, then copy it back.
* Find the `.code-font()` rule and change the `font-family` as needed.
* Restart Brackets

**Note:** when you update Brackets, these changes will be lost and you'll have to repeat these steps. For a more permanent solution, you can [write a Brackets extension](https://github.com/adobe/brackets/wiki/How-to-write-extensions) that loads a stylesheet to override the appropriate font styles.