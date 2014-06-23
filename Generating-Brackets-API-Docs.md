This set of commands creates the Brackets API Docs to be published at http://brackets.io/docs/current.

## Use Mac

__Note:__ There is a [known bug](https://github.com/jbalsas/apify/issues/3) the incorrect folder structure and links are generated on Windows, so use Mac to generate documents until that bug is fixed.

## Install node apify package

This command installs the node apify package globally so you can run it from any folder. Note that apify is still being developed, so it's a good idea to re-run this command each time to get the latest package.

```
cd ~
sudo npm install -g apify
```

## Create branch in brackets.io repo

Brackets API documents are posted to the https://github.com/adobe/brackets.io repo, so clone that repo and create a new branch.

```
git clone https://github.com/adobe/brackets.io.git
cd brackets.io
git checkout -b yourname/new-branch-name
```

## Checkout Brackets branch

Checkout the Brackets branch that you want to generate API Docs for. Currently, we only generate docs for the current master, but once we reach version 1.0, we'll want to generate docs for each major and minor release.

```
git pull
```

## Generate Docs

The source folder for API Docs is the `brackets/src` folder. The following commands assumes that you are in the root of the **brackets** repo.

The output folder is `brackets.io/docs/current`. The following command assumes that the **brackets** and **brackets.io** root folders are side-by-side.

```
cd /path_to_brackets
apify -s src/ -o ../brackets.io/docs/current
```

## Submit Pull Request

The API Docs should now be in the **brackets.io** repo in your new branch, so you'll need to submit a pull request and then merge files.
