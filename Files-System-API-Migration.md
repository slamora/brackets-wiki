Here are the API changes that will result from the [File System Evolution](File System) plan:

<table>
<thead>
<tr><td>Old API</td><td>New API</td><td>Suggested action</td><td>Usage</td></tr>
</thead>
<tr><td>FileUtils.readAsText(fileEntry)</td><td>FileUtils.readAsText(file)</td><td>Already drop-in compatible</td><td></td></tr>
<tr><td>FileUtils.writeText(fileEntry)</td><td>FileUtils.writeText(file)</td><td>Already drop-in compatible</td><td></td></tr>
<tr><td>new NativeFileSystem.FileEntry(fullPath)</td><td>filesystem.getFileForPath(fullPath)</td><td>Shim with deprecation warning</td><td></td></tr>
<tr><td>new NativeFileSystem.DirectoryEntry(fullPath)</td><td>filesystem.getDirectoryForPath(fullPath)</td><td>Shim with deprecation warning</td><td></td></tr>
</table>