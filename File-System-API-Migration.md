Here are the API changes that will result from the [File System Evolution](File System) plan:

**"Compatibility"** column:

* **Drop-in compatible** - code using the API probably does not need to change at all, though it's _possible_ there are subtle differences in edge cases
* **Deprecated & shimmed** - the API will _temporarily_ continue to behave as before, but will be removed in the near future (under the hood, the old API has been reimplemented in terns of the new APIs, so there may be minor, subtle differences)
* **Removed** - the API is completely gone; code using it will throw exceptions

<table>
<thead>
<tr><td><b>Old API</b></td><td><b>New API</b></td><td><b>Compatibility</b></td><td><b>Usage</b></td></tr>
</thead>

<tr><td>FileEntry<br>DirectoryEntry</td><td>File<br>Directory</td><td>Drop-in compatible (same fields/members)</td><td>(lots)</td></tr>
<tr><td>entry.fullPath, entry.isFile, entry.isDirectory</td><td>(unchanged)</td><td>(unchanged)<br>Properties are now read-only</td><td>fullPath: many<br>isFile/Directory: 9</td></tr>
<tr><td>FileUtils.readAsText(fileEntry)</td><td>FileUtils.readAsText(file)</td><td>Drop-in compatible</td><td>13</td></tr>
<tr><td>FileUtils.writeText(fileEntry)</td><td>FileUtils.writeText(file)</td><td>Drop-in compatible</td><td>5</td></tr>
<tr><td>new NativeFileSystem.FileEntry(fullPath)<br>new NativeFileSystem.DirectoryEntry(fullPath)</td><td>filesystem.getFileForPath(fullPath)<br>filesystem.getDirectoryForPath(fullPath)</td><td>Deprecated & shimmed (constructors return a File/Directory object instead)</td><td>19</td></tr>

<tr><td>DirectoryEntry.getFile(relpath)</td><td>filesystem.resolve(dirFullPath + relpath)</td><td>Deprecated & shimmed</td><td>4</td></tr>
<tr><td>NativeFileSystem.resolveNativeFileSystemPath(fullPath)</td><td>filesystem.resolve(path)</td><td>Deprecated & shimmed</td><td>4</td></tr>
<tr><td>NativeFileSystem.requestNativeFileSystem(fullPath)... fs.root</td><td>filesystem.resolve(fullPath)</td><td>Deprecated & shimmed</td><td>6</td></tr>
<tr><td>Concatenation: folderPath + "/" + filename</td><td>(unchanged)</td><td>(unchanged)<br>Doubled "/"es are normalized out by fs APIs</td><td></td></tr>
<tr><td>Concatenation: folderPath + filename</td><td>(unchanged)</td><td>(unchanged)<br>Directory.fullPath always ends in "/", just like DirectoryEntry.fullPath</td><td>Several</td></tr>
<tr><td>Substrings: relpath = fullPath.slice(dirFullPath.length)</td><td>(unchanged)</td><td>(unchanged)<br>Directory.fullPath always ends in "/", just like DirectoryEntry.fullPath</td><td></td></tr>
<tr><td>NativeFileSystem.showOpenDialog()<br>NativeFileSystem.showSaveDialog()</td><td>filesystem.showOpenDialog()<br>filesystem.showSaveDialog()</td><td>Deprecated & shimmed</td><td>4</td></tr>
<tr><td>fileEntry.createWriter()... writer.write(text)</td><td>file.write(text)</td><td>Removed</td><td>5 (unused in 3)</td></tr>
<tr><td>fileEntry.file()... new NativeFileSystem.FileReader().readAsText(fileObj)</td><td>file.readAsText()</td><td>Removed</td><td>None</td></tr>
<tr><td>directoryEntry.createReader().readEntries()</td><td>directory.getContents() ???</td><td>Removed</td><td>5</td></tr>
<tr><td>DirectoryEntry.getFile(relpath, {create:true})</td><td>filesystem.getFileForPath(fullPath).write("")</td><td>Removed</td><td>2</td></tr>
<tr><td>DirectoryEntry.getDirectory(relpath, {create:true})</td><td>filesystem.getDirectoryForPath(fullPath).create()</td><td>Removed</td><td>2</td></tr>
<tr><td>FileIndexManager.getFileInfoList("all")<br>FileIndexManager.getFileInfoList("...")</td><td>ProjectManager.getAllFiles()<br>ProjectManager.getAllFiles(filter)<br>(Returns an array of Files, but they provide same properties as the old FileInfos)</td><td>Deprecated & shimmed</td><td>7</td></tr>
<tr><td>FileIndexManager.getFilenameMatches("...", filename)</td><td>ProjectManager.getAllFiles(filter)</td><td>Deprecated & shimmed</td><td>None</td></tr>
<tr><td>ProjectManager "projectFilesChange" event</td><td>FileSystem "change" event</td><td>Removed</td><td>2</td></tr>

<tr><td colspan="4"><b>Less common APIs</b></td></tr>

<tr><td>fileEntry.getMetadata()</td><td>file.stat()</td><td>Removed</td><td>1</td></tr>
<tr><td>NativeFileError.*</td><td>FileSystemError.*</td><td>Removed</td><td>1</td></tr>
<tr><td>instanceof NativeFileSystem.InaccessibleFileEntry</td><td>instanceof InMemoryFile</td><td>Removed</td><td>1</td></tr>
<tr><td>entry.remove()</td><td>entry.moveToTrash()</td><td>Removed</td><td>None</td></tr>
<tr><td>entry.filesystem</td><td>n/a</td><td>Removed</td><td>None</td></tr>
<tr><td>NativeFileSystem.Encodings.*</td><td>(none)</td><td>Removed</td><td>None</td></tr>
<tr><td>NativeFileSystem.isRelativePath()</td><td>(none)</td><td>Removed</td><td>None</td></tr>
<tr><td>brackets.fs.*()</td><td>(various)</td><td>These low-level APIs continue to exist, but have never been officially supported. They may break at any time.</td><td>Several</td></tr>

</table>

"Usage" = how many extensions currently use the API

### Extensions that will break

The following 7 extensions use at least one API on the "Removed" list:

* angularui.angularjs -- (FileEntry.getMetadata())
* bsirlinger.github-access -- (DirectoryEntry.getFile({create}), DirectoryEntry.getDirectory({create}), FileEntry.createWriter())
* xunit -- (DirectoryEntry.getFile({create}), DirectoryEntry.getDirectory({create})
* zaggino.brackets-git (won't crash, but notable bug) -- ("projectFilesChange" event)
* variousimprovements (won't crash, minor bug) -- ("projectFilesChange" event)
* pflynn.brackets.editor.nav (won't crash, very minor bug) -- (InaccessibleFileEntry)
* interactive-linter (no effect on behavior) -- (NativeFileError)

We will file bugs with all extensions that are using removed APIs to make sure authors are aware of the change.