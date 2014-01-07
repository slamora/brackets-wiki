Brackets doesn't officially support a preference for changing the code editor font. But here's a quick hack you can use to work around issues like [lack of Russian/Cyrillic support](https://github.com/adobe/brackets/issues/3465):

## Approach A: Write an extension

[Write a Brackets extension](https://github.com/adobe/brackets/wiki/How-to-write-extensions) using this code as the basis for your _main.js_:

```
define(function (require, exports, module) {
    "use strict";

    var ExtensionUtils = brackets.getModule("utils/ExtensionUtils");

    ExtensionUtils.addEmbeddedStyleSheet(".CodeMirror { font-family: 'Comic Sans MS'; }");
});
```

...except pick a less horrible font! :-)

> Note: There's no need to write a package.json file since this extension is just for yourself; package.json is only needed if you want to publish the extension for others to use. 

If you store this extension in the location given by Help > Show Extensions Folder, then you can freely upgrade to newer Brackets versions and your extension will remain in place.

_Caveat: This approach only works for fonts installed on your OS._ If you want to use a web font (loaded via `@font-face`), you need to edit the Brackets core source code.


## Approach B: Edit Brackets source

1. [Set up a Brackets dev environment](https://github.com/adobe/brackets/wiki/How-to-Hack-on-Brackets#setting-up-your-dev-environment) (you can omit the fork step and clone the official repo directly, however)
2. In the cloned source, edit the file <i>src/styles/brackets_theme_default.less</i>
3. Add your `@font-face` import directive if needed 
4. Find the `.code-font()` rule and change the `font-family` as desired
5. Restart Brackets

_Caveats:_ In order to keep up to date with Brackets updates, you'll need to to manually pull down source code updates (this can be tricky if you're not familiar with Git). Brackets loads slightly slower in a dev environment because the source isn't minified.