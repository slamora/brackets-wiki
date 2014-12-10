Circular dependencies between RequireJS modules can be tricky. Here are some tools for analyzing dependencies within the Brackets codebase.

## Visualize the whole dependency graph

```
npm install -g dependo
dependo --format cjs --exclude "/node_modules/|/extensions/|/nls/|/node/|/jquery-ui/|/unittest-files/|/tests/|/test/|/spec/" <<path_to_brackets_src>> > dependencyView.html
```

Then open the HTML file in a browser and wait a minute for the graph positioning to stabilize.

Warning: mousing over a node will make the browser tab freeze for a bit.


## Find dependency tree of a single module

TODO

## Find all circular dependencies

TODO