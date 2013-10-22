Here are the API changes that will result from the [File System Evolution](File System) plan:

<table>
<thead>
<tr><td><b>Old API</b></td><td><b>New API</b></td><td><b>Suggested action</b></td><td><b>Usage</b></td></tr>
</thead>

<tr><td>FileUtils.readAsText(fileEntry)</td><td>FileUtils.readAsText(file)</td><td>Already drop-in compatible</td><td>13</td></tr>
<tr><td>FileUtils.writeText(fileEntry)</td><td>FileUtils.writeText(file)</td><td>Already drop-in compatible</td><td>5</td></tr>
<tr><td>FileIndexManager.getFileInfoList("all")<br>FileIndexManager.getFileInfoList("...")</td><td>filesystem.getFileList()<br>filesystem.getFileList(filter)<br>_Note: returns an array of actual Files, but they provide the same properties as the old FileInfos)_</td><td>Shim with deprecation warning</td><td>7</td></tr>
<tr><td>FileIndexManager.getFilenameMatches("...", filename)</td><td>filesystem.getFileList(filter)</td><td>Shim with deprecation warning</td><td>None</td></tr>
<tr><td>ProjectManager.shouldShow(entry)</td><td>filesystem.shouldShow(fullPath)</td><td>Leave old API in place permanently</td><td>None?</td></tr>
<tr><td>ProjectManager.isBinaryFile(name/fullPath)</td><td>(unchanged)</td><td>(unchanged)<br>But eventually, should move to LanguageManager.</td><td>None</td></tr>

<tr><td>new NativeFileSystem.FileEntry(fullPath)<br>new NativeFileSystem.DirectoryEntry(fullPath)</td><td>filesystem.getFileForPath(fullPath)<br>filesystem.getDirectoryForPath(fullPath)</td><td>Shim with deprecation warning</td><td>19</td></tr>
<tr><td>DirectoryEntry.getFile(relpath)</td><td>filesystem.resolve(dirFullPath + relpath)</td><td>TODO: Consider offering same/similar API on Directory</td><td>4</td></tr>
<tr><td>NativeFileSystem.resolveNativeFileSystemPath(fullPath)</td><td>filesystem.resolve(path)</td><td>Shim with deprecation warning</td><td>4</td></tr>
<tr><td>NativeFileSystem.requestNativeFileSystem(fullPath)... fs.root</td><td>filesystem.resolve(fullPath)</td><td>Shim with deprecation warning</td><td>6</td></tr>
<tr><td>entry.fullPath, entry.isFile, entry.isDirectory</td><td>(unchanged)</td><td>Properties are now immutable</td><td>fullPath: many<br>isFile/Directory: 9</td></tr>
<tr><td>Concatenation: folderPath + "/" + filename</td><td>(unchanged)</td><td>Paths are normalized on ingest, eliminating doubled "/"es</td><td></td></tr>
<tr><td>Concatenation: folderPath + filename</td><td>(unchanged)</td><td>Directory.fullPath will always end in "/", so this remains safe</td><td>Unclear, but at least several</td></tr>
<tr><td>Substrings: relpath = fullPath.slice(dirFullPath.length)</td><td>(unchanged)</td><td>Directory.fullPath will always end in "/", so this remains safe</td><td></td></tr>
<tr><td>NativeFileSystem.isRelativePath()</td><td>???</td><td>???</td><td>None</td></tr>
<tr><td>NativeFileSystem.showOpenDialog()<br>NativeFileSystem.showSaveDialog()</td><td>filesystem.showOpenDialog()<br>filesystem.showSaveDialog()</td><td>Shim with deprecation warning</td><td>4</td></tr>
<tr><td>fileEntry.getMetadata()</td><td>file.stat()<br>_TODO: document change in data structure too_</td><td>Do not shim (callers will break right away)</td><td>1</td></tr>
<tr><td>fileEntry.createWriter()... writer.write(text)</td><td>file.write(text)</td><td>Do not shim (callers will break right away)</td><td>5, but only used in 2</td></tr>
<tr><td>fileEntry.file()... new NativeFileSystem.FileReader().readAsText(fileObj)</td><td>file.readAsText()</td><td>Do not shim (callers will break right away)</td><td>None</td></tr>
<tr><td>directoryEntry.createReader().readEntries()</td><td>directory.getContents() ???</td><td>Do not shim (callers will break right away)</td><td>5</td></tr>
<tr><td>DirectoryEntry.getFile(relpath, {create:true})</td><td>filesystem.getFileForPath(fullPath).write("")<br>Note: as a result, this can fold in writeText() or createWriter()/write() calls that used to immediately follow the getFile() call.</td><td>Do not shim (callers will break right away)<br>TODO: add a cleaner create() API?</td><td>2</td></tr>
<tr><td>DirectoryEntry.getDirectory(relpath, {create:true})</td><td>filesystem.getDirectoryForPath(fullPath).create()</td><td>Do not shim (callers will break right away)</td><td>2</td></tr>
<tr><td>instanceof NativeFileSystem.InaccessibleFileEntry</td><td>instanceof InMemoryFile</td><td>Do not shim (callers will break right away)</td><td>1</td></tr>
<tr><td>entry.remove()</td><td>entry.moveToTrash()</td><td>Do not shim (callers will break right away)</td><td>None</td></tr>
<tr><td>NativeFileError.*</td><td>???</td><td>Do not shim (callers will break right away)</td><td>1</td></tr>
<tr><td>entry.filesystem<br>(not a very useful API)</td><td>n/a</td><td>Do not shim (callers will break right away)</td><td>None</td></tr>
<tr><td>NativeFileSystem.Encodings.*</td><td>???</td><td>Do not shim (callers will break right away)</td><td>None</td></tr>
<tr><td>"projectFilesChange" event (sent from ProjectManager)</td><td>"change" event (sent from FileSystem)</td><td>Do not shim</td><td>2</td></tr>

<tr><td>brackets.fs.readFile()<br>brackets.fs.writeFile()</td><td>file.readAsText()<br>file.write(text)</td><td>Low-level API continues to exist</td><td>2</td></tr>
<tr><td>brackets.fs.stat()</td><td>file.stat()</td><td>Low-level API continues to exist</td><td>1</td></tr>
<tr><td>brackets.fs.readdir()</td><td>directory.getContents() ???</td><td>Low-level API continues to exist</td><td>1</td></tr>
<tr><td>brackets.fs.makedir(fullPath, mode)</td><td>filesystem.getDirectoryForPath(fullPath).create()</td><td>Low-level API continues to exist</td><td>1</td></tr>
<tr><td>brackets.fs.unlink(fullPath)</td><td>entry.unlink()</td><td>Low-level API continues to exist</td><td>None</td></tr>
<tr><td>brackets.fs.rename(oldFullPath, newFullPath)</td><td>entry.rename(newFullPath)<br>(NOTE: Exact semantics of this call are still a bit TBD).</td><td>Low-level API continues to exist</td><td>None</td></tr>
<tr><td>brackets.fs.moveToTrash()</td><td>entry.moveToTrash()</td><td>Low-level API continues to exist</td><td>None</td></tr>
<tr><td>brackets.fs.copyFile()</td><td>???</td><td>Low-level API continues to exist</td><td>1</td></tr>
<tr><td>brackets.fs.isNetworkDrive()</td><td>???</td><td>Low-level API continues to exist<br>TODO: will it be unused now though?</td><td>None</td></tr>
<tr><td>brackets.fs.chmod()</td><td>???</td><td>Low-level API continues to exist</td><td>None</td></tr>
<tr><td>brackets.fs.showOpenDialog()<br>brackets.fs.showSaveDialog()</td><td>filesystem.showOpenDialog()<br>filesystem.showSaveDialog()</td><td>Low-level API continues to exist</td><td>None</td></tr>
<tr><td>new NativeFileSystem.InaccessibleFileEntry(fakePath)</td><td>filesystem.getInMemoryFile(fakePath)</td><td>Low-level API continues to exist</td><td>None</td></tr>
<tr><td>brackets.fs.ERR_*<br>brackets.fs.PATH_EXISTS_ERR<br>brackets.fs.NO_ERROR</td><td>???</td><td>Low-level API continues to exist</td><td>1</td></tr>

</table>

"Usage" = how many extensions currently use the API

### Extensions that will break

The following 12 extensions use at least one API on the "Do not shim" list:

* themes -- (DirectoryEntry.createReader())
* themesforbrackets -- (DirectoryEntry.createReader())
* interactive-linter -- (DirectoryEntry.getFile(), DirectoryEntry.createReader(), FileEntry.createWriter(), NativeFile Error (only minor bug))
* angularui.angularjs -- (FileEntry.getMetadata())
* timburgess.pagesuck -- (DirectoryEntry.getFile())
* ternific -- (DirectoryEntry.getFile(), DirectoryEntry.createReader(), FileEntry.createWriter())
* tomcat-manager -- (DirectoryEntry.getFile(), FileEntry.createWriter())
* simple-js-code-hints -- (DirectoryEntry.createReader())
* bsirlinger.github-access -- (DirectoryEntry.getFile({create}), DirectoryEntry.getDirectory({create}), FileEntry.createWriter())
* xunit -- (DirectoryEntry.getFile({create}), DirectoryEntry.getDirectory({create})
* workspaces -- (FileEntry.createWriter())
* pflynn.brackets.editor.nav -- (won't crash, only minor bug) (InaccessibleFileEntry)

We will file bugs with all extensions that are using removed APIs to make sure authors are aware of the change.