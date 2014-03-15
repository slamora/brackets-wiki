## Stats for the sprint about to ship

#### Commits & files

* **Number of commits** - use a link of the form https://github.com/adobe/brackets/compare/sprint-XX...master (and do the same for brackets-shell, summing the number of commits)
* **Number of files touched** - same as above

#### Pull requests

1. Install Peter's "Brackets Reports" extension / make sure you have the latest
2. Find the last PR in the _previous_ sprint - `git show sprint-XX` should give you its merge commit, which says the PR #. (Don't forget to check -shell too in case it's newer -- you want the more recent of the two).
3. Go to that PR on GitHub
4. Find the "merged ... 20 days ago" (or whatever) label at bottom and inspect it in the Dev Tools
5. Copy the value of the `datetime` attribute
6. In the extension, edit _GitHubReports.js_ and change the value of `END_LAST_SPRINT` - paste in the 'datetime' value from step 5
7. Reload Brackets
8. Run _View > Brackets Reports > Pull Requests in Sprint_

After a few seconds, you'll get a dialog with a big report, which includes the following info:

* **Number of pull requests merged (total)** (note that this does _not_ count PRs that were closed without being merged)
* **Number of external pull requests merged**
* **Number that came from external committers**
* **Total number of external contributors this sprint**

#### Locales

To see if there were any **new locales**, compare the set of folders under `src/nls` in this sprint vs. the previous sprint.

#### Bugs

We normally don't report the number of issues fixed, because (a) it's hard to distinguish legacy bugs vs. bugs that were both opened _and_ closed within the one sprint's lifecycle, and (b) we often forget to tag every bug with a sprint milestone, leading to varying degrees of undercount.

But you can get a _rough_ number via a "closed issues" milestone guery on GitHub -- something of the form https://github.com/adobe/brackets/issues?labels=&milestone=10&state=closed. (Note that the milestone number is _not_ equal to the sprint number - it's typically `(sprint number - 13)`).


## Stats for the sprint about to be EOL'ed

#### Downloads

* **Total downloads** - sum [sprint download stats](http://download.brackets.io/report.cfm) across all 3 platforms
* **Platform breakdown (%)** - divide each platform by the total
    * Note: this may understate Linux users, since we know there are some alternative distribution channels that don't hit our download site

#### Extensions - current

1. _View > Brackets Reports > Save Extension Registry Snapshot_
2. Name the file something like "registry - end of Sprint XX lifespan.json" (where XX is the _old_ sprint that's about to be EOL'ed)
3. _View > Brackets Reports > Extension Registry Report_ and select the file you just saved

This gives you:

* **Total available** - listed at top
* **Total number of authors** - sum the internal & external totals
* **Number of external authors**

#### Extensions - changes over time

To compare with previous sprints, you'll need a similar JSON file saved from an earlier sprint (Peter has an archive of these if needed).

1. _View > Brackets Reports > Extension Registry Diff_
2. Choose the older JSON file first (e.g. if we're about to ship Sprint 35, choose Sprint _33_'s file)
3. Choose the JSON file you've just generated (e.g. if we're about to ship Sprint 35, choose Sprint _34_'s file)

Scroll to the bottom of the report for these totals:

* **Number of new extensions added**
* **Number of existing extensions updated**
* **Number of existing extensions deleted** (this is normally zero since there's no easy way to delete deprecated extensions)


## Other Stats

* **[GitHub traffic report](https://github.com/adobe/brackets/graphs/traffic)**
* **[Top-starred repos](https://github.com/search?l=&q=stars%3A%3E10000&type=Repositories)** (Brackets is #16 as of March 2014)
* **[Top-starred JS repos](https://github.com/search?l=JavaScript&q=stars%3A%3E10000&type=Repositories)** (Brackets is #8 as of March 2014)