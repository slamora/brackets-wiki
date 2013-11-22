Help for using the [Brackets Extension Registry](http://brackets-registry.aboutweb.com/)

## How do I write an extension? ##

Take a look at the [How to write extensions](https://github.com/adobe/brackets/wiki/How-to-Write-Extensions) wiki page.

## How do I publish my extension in the registry? ##

First, make sure you have your extension in the [extension package format](https://github.com/adobe/brackets/wiki/Extension-package-format) which is basically a .zip file with a package.json file in it. If your extension is on GitHub, you'll find a link to a zip file in the navigation elements on the right-hand side of your repository.

1. Go to the [Registry site](https://brackets-registry.aboutweb.com/)
2. Log in with your GitHub credentials. We never see your GitHub password and your extension itself doesn't need to be on GitHub (see below).
3. Drag and drop your zip file onto the big upload square, or click the square to browse to your zip file

## How do I update my extension? ##

Just follow the exact same steps you used to upload your extension in the first place. Make sure that you've increased the version number in package.json. The Registry will detect that you're updating an extension.

## Do I have to have my code on GitHub? ##

No. Right now, the Registry requires a GitHub account in order to identify you to the registry. You can use any source repository and zip utility to create the extension package file that you upload.

## Is the Registry open source? ##

Yes! It has [its own repository](https://github.com/adobe/brackets-registry) on GitHub.

## How do I remove an extension from the Registry? ##

At present, there is no (automated) way to remove an extension from the registry. Contact the Brackets core team if you need to remove something completely from the listing.

As a quick workaround, you could upload an update to your extension that contains an empty main.js so that your extension basically does nothing (and change the package description to indicate it's obsolete).

## I need some other help! ##

If you have questions that are not answered here, feel free to ask on the [email list](https://groups.google.com/forum/#!forum/brackets-dev). If you have a security or other private issue, feel free to email brackets-registry (which is at aboutweb.com – the company that is hosting the Brackets Registry).