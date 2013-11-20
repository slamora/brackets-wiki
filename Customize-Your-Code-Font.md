Brackets doesn't officially support a preference for changing the code editor font. But here's a quick hack you can use to work around issues like [lack of Russian/Cyrillic support](https://github.com/adobe/brackets/issues/3465):

## Quick solution

1. Locate the installed Brackets source:
    * On Mac, right-click Brackets and choose Show Package Contents, then expand the "Contents" folder.
    * On Windows, navigate to your install folder (usually _C:\Program Files (x86)\Brackets_).
2. Edit the file <i>www/styles/brackets_theme_default.less</i>
    * On Windows, due to UAC, you may need to copy this file to another location, edit it, then copy it back.
3. Find the `.code-font()` rule and change the `font-family` as needed.
4. Restart Brackets

**Downside:** When you update Brackets, these changes will be lost and you'll have to repeat these steps.

_Caveat:_ Additional steps are needed if you want to use a web font (loaded via `@font-face`) rather than just referencing by name a font that's installed on your OS.

## More permanent solution

1. [Write a Brackets extension](https://github.com/adobe/brackets/wiki/How-to-write-extensions)
2. In your extension, use `ExtensionUtils.addEmbeddedStyleSheet()` to inject your own custom CSS
3. In your custom CSS, override the `.CodeMirror` style with your new `font-family`

_Caveat:_ Same as above, additional steps are needed if you want to use a web font (loaded via `@font-face`).
