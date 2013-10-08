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
<tr><td>ProjectManager.isBinaryFile(name/fullPath)</td><td>???</td><td>Shim with deprecation warning. New API on LanguageManager.</td><td>None</td></tr>

<tr><td>new NativeFileSystem.FileEntry(fullPath)<br>new NativeFileSystem.DirectoryEntry(fullPath)</td><td>filesystem.getFileForPath(fullPath)<br>filesystem.getDirectoryForPath(fullPath)</td><td>Shim with deprecation warning</td><td>19</td></tr>
<tr><td>DirectoryEntry.getFile(relpath)</td><td>filesystem.resolve(path)</td><td>Do not shim (callers will break right away)</td><td>4</td></tr>
<tr><td>NativeFileSystem.resolveNativeFileSystemPath(fullPath)</td><td>filesystem.resolve(path)</td><td>Shim with deprecation warning</td><td>4</td></tr>
<tr><td>NativeFileSystem.requestNativeFileSystem(fullPath)... fs.root</td><td>filesystem.resolve(fullPath)</td><td>Shim with deprecation warning</td><td>6</td></tr>
<tr><td>Concatenation: folderPath + "/" + filename</td><td>(same)</td><td>Normalize paths on ingest to allow this</td><td></td></tr>
<tr><td>Concatenation: folderPath + filename</td><td>folderPath + "/" + filename</td><td>Change all directory paths to end in "/" to allow this</td><td>Unclear, but at least several</td></tr>
<tr><td>Substrings: relpath = fullPath.slice(dirFullPath.length)</td><td>relpath = fullPath.slice(dirFullPath.length + 1)</td><td>(Above change removes this diff too)</td><td></td></tr>
<tr><td>entry.isFile, entry.isDirectory</td><td>entry.isFile(), entry.isDirectory()</td><td>Change API to use a read-only property (like fullPath)</td><td>9</td></tr>
<tr><td>NativeFileSystem.showOpenDialog()<br>NativeFileSystem.showSaveDialog()</td><td>filesystem.showOpenDialog()<br>filesystem.showSaveDialog()</td><td>Shim with deprecation warning</td><td>4</td></tr>
<tr><td>fileEntry.getMetadata()</td><td>file.stat()<br>_TODO: document change in data structure too_</td><td>Do not shim (callers will break right away)</td><td>1</td></tr>
<tr><td>fileEntry.createWriter()... writer.write(text)</td><td>file.write(text)</td><td>Do not shim (callers will break right away)</td><td>5, but only used in 2</td></tr>
<tr><td>fileEntry.file()... new NativeFileSystem.FileReader().readAsText(fileResult)</td><td>file.readAsText()</td><td>Do not shim (callers will break right away)</td><td>None</td></tr>
<tr><td>directoryEntry.createReader().readEntries()</td><td>directory.getContents() ???</td><td>Do not shim (callers will break right away)</td><td>5</td></tr>
<tr><td>DirectoryEntry.getFile(relpath, {create:true})</td><td>filesystem.getFileForPath(fullPath).write("")<br>Note: as a result, this can fold in writeText() or createWriter()/write() calls that used to immediately follow the getFile() call.</td><td>Do not shim (callers will break right away)<br>TODO: add a cleaner create() API?</td><td>2</td></tr>
<tr><td>DirectoryEntry.getDirectory(relpath, {create:true})</td><td>filesystem.getDirectoryForPath(fullPath).create()</td><td>Do not shim (callers will break right away)</td><td>2</td></tr>
<tr><td>instanceof NativeFileSystem.InaccessibleFileEntry</td><td>instanceof InMemoryFile</td><td>Do not shim (callers will break right away)</td><td>1(ish)</td></tr>
<tr><td>entry.remove()</td><td>entry.moveToTrash()</td><td>Do not shim (callers will break right away)</td><td>None</td></tr>

<tr><td>brackets.fs.makedir(fullPath, mode)</td><td>filesystem.getDirectoryForPath(fullPath).create()</td><td>Do not shim (callers will break right away)</td><td>1</td></tr>
<tr><td>brackets.fs.unlink(fullPath)</td><td>entry.unlink()</td><td>Do not shim (callers will break right away)</td><td>None</td></tr>
<tr><td>brackets.fs.rename(oldFullPath, newFullPath)</td><td>entry.rename(newFullPath)<br>(NOTE: Exact semantics of this call are still a bit TBD).</td><td>Do not shim (callers will break right away)</td><td>None</td></tr>
<tr><td>brackets.fs.chmod()</td><td>???</td><td>Do not shim (callers will break right away)</td><td>None</td></tr>
<tr><td>new NativeFileSystem.InaccessibleFileEntry(fakePath)</td><td>filesystem.getInMemoryFile(fakePath)</td><td>Do not shim (callers will break right away)</td><td>None</td></tr>

<tr><td>old</td><td>new</td><td>action</td><td></td></tr>
</table>

"Usage" = how many extensions currently use the API

### Extensions that will break

The following extensions use at least one API in the "Do not shim (callers will break right away)" category:

* ...