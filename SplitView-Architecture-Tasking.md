Here's a proposed guide to breakup the story into smaller arch pieces:

1. Opportunistic Cleanup
   * _openInlineWidget
   * _toggleInlineWidget
   * _showCustomViewer
   * closeInlineWidget
   * getInlineEditors
   * getFocusedInlineWidget

2. Migrate WorkingSet Management to `EditorManager`
   * Code new WorkingSet APIs in EditorManager
   * Move serialization code from DocumentManager
   * add deprecation warnings to old APIs
   * add bridge to new API from old APIs
   * Write issues for extensions still using old APIs
   * Migrate core and default extension code to new WorkingSet API
   * Notify extension authors
   * Update Unit tests

3. Refactor`File.commands` from `DocumentCommandHandlers` into `FileCommandHandlers` 
   (depends on #2)
   * Move FILE.xxx and call EditorManager functions 

4. Deprecate getWorkingSet 
   (depends on #2)
   * Migrate all core instances (7)
   * Migrate all core extension instances
   * Notify extension authors
   * Add deprecation warning
   * Unit tests

5. Migrate DocumentManager.getCurrentDocument 
   (depends on #2)
   * Notify all extension authors 

6. Implement EditorManager code for 1x2 editors
   * tease apart editor calls directly and move to editor
   * implement basic layout
   * implement WorkingSetView create
   * implement editor create
   * update serialization code
   * working set context and gear menus
   * close others extension
   * test git extension (notfy zaggino)
   * Unit tests

7. Add support for images in working set
   * create custom viewer
   * implement all file based commands to work on images
   * Implement all working set commands for images 
   * Unit tests

8. Implement UI for split view
   (depends on 6)
   * implement core changes to direct splits, etc...

9. Final cleanup and Polish.