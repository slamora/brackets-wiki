_citrus completed_ test project: https://github.com/adobe/brackets/tree/master/test/smokes/citrus%20completed

To setup each test, load the _citrus completed_ project via _File > Open folder..._

## File Operations

### Error opening file

1. In the project tree, expand the ``images`` folder.
2. Click on ``events.jpg``
3. Confirm error message ``<error opening file screenshot>``

### Invalid file name

1. Select the ``File > New`` menu
2. ``untitled.js`` will appear editable
3. Rename ``untitled.js`` to ``foo:js``
4. Confirm error message ``<error invalid file name screenshot>``
6. Select ``index.html``
7. Select the ``File > New`` menu
8. Rename ``untitled.js`` to ``index.html``
9. Confirm error message ``<error file already exists screenshot>``