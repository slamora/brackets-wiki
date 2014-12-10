Circular dependencies between RequireJS modules can be tricky. Here are some tools for analyzing dependencies within the Brackets codebase.

## Visualize the whole dependency graph

```
npm install -g dependo
dependo --format cjs --exclude "/node_modules/|/extensions/dev/|/jquery-ui/|/unittest-files/|/tests/|/test/|/spec/" <<path_to_brackets_src>> > dependencyView.html
```

Then open the HTML file in a browser and wait a minute for the graph positioning to stabilize.

You'll notice two big disconnected islands -- this is because there's no hard dependency from the main core code to anything in the core extensions (`src/extensions/default`) tree (and all of the extension "main" modules get lumped into one node, because dependo doesn't understand that each extension has a separate RequireJS context root).


## Find dependency tree of a single module

TODO

## Find all circular dependencies

TODO