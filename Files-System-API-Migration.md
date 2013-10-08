Here are the API changes that will result from the [File System Evolution](File System) plan:

<table>
<thead>
<tr><td>Old API</td><td>New API</td><td>Suggested action</td><td>Usage</td></tr>
</thead>

<tr><td>FileUtils.readAsText(fileEntry)</td><td>FileUtils.readAsText(file)</td><td>Already drop-in compatible</td><td></td></tr>
<tr><td>FileUtils.writeText(fileEntry)</td><td>FileUtils.writeText(file)</td><td>Already drop-in compatible</td><td></td></tr>
<tr><td>FileIndexManager.getFileInfoList("all")<br>FileIndexManager.getFileInfoList("...")</td><td>filesystem.getFileList()<br>filesystem.getFileList(filter)<br>_Note: returns an array of actual Files, but they provide the same properties as the old FileInfos)_</td><td>Shim with deprecation warning</td><td></td></tr>
<tr><td>FileIndexManager.getFilenameMatches("...", filename)</td><td>filesystem.getFileList(filter)</td><td>Shim with deprecation warning</td><td></td></tr>
<tr><td>ProjectManager.shouldShow(entry)</td><td>filesystem.shouldShow(fullPath)</td><td>Leave old API in place permanently</td><td></td></tr>
<tr><td>ProjectManager.isBinaryFile(name/fullPath)</td><td>???</td><td>Shim with deprecation warning. New API on LanguageManager.</td><td></td></tr>

<tr><td>new NativeFileSystem.FileEntry(fullPath)<br>new NativeFileSystem.DirectoryEntry(fullPath)</td><td>filesystem.getFileForPath(fullPath)<br>filesystem.getDirectoryForPath(fullPath)</td><td>Shim with deprecation warning</td><td></td></tr>
<tr><td>DirectoryEntry.getFile(relpath)</td><td>filesystem.resolve(path)</td><td>Do not shim (callers will break right away)</td><td></td></tr>
<tr><td>Concatenation: folderPath + "/" + filename</td><td>(same)</td><td>(no change)</td><td></td></tr>
<tr><td>Concatenation: folderPath + filename</td><td>folderPath + "/" + filename</td><td>**Open question**</td><td></td></tr>
<tr><td>Substrings: relpath = fullPath.slice(dirFullPath.length)</td><td>relpath = fullPath.slice(dirFullPath.length + 1)</td><td>**Open question**</td><td></td></tr>
<tr><td>entry.isFile, entry.isDirectory</td><td>entry.isFile(), entry.isDirectory()</td><td>Change API to use a read-only property (like fullPath)</td><td></td></tr>
<tr><td>NativeFileSystem.showOpenDialog()<br>NativeFileSystem.showSaveDialog()</td><td>filesystem.showOpenDialog()<br>filesystem.showSaveDialog()</td><td>Shim with deprecation warning _(maybe?)_</td><td>**TODO**</td></tr>
<tr><td>fileEntry.getMetadata()</td><td>file.stat()<br>_TODO: document change in data structure too_</td><td>Do not shim (callers will break right away)</td><td></td></tr>
<tr><td>fileEntry.createWriter()... writer.write(text)</td><td>file.write(text)</td><td>Do not shim (callers will break right away)</td><td></td></tr>
<tr><td>fileEntry.file()... new NativeFileSystem.FileReader().readAsText(fileResult)</td><td>file.readAsText()</td><td>Do not shim (callers will break right away)</td><td></td></tr>
<tr><td>directoryEntry.createReader().readEntries()</td><td>directory.getContents() ???</td><td>Do not shim (callers will break right away)</td><td>**TODO**</td></tr>

<tr><td>old</td><td>new</td><td>action</td><td></td></tr>
</table>