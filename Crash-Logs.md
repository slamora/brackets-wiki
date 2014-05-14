Native crash logs are useful for diagnosing cases where the Brackets window disappears or goes blank suddenly.
(If Brackets is misbehaving in other ways, please see [[Troubleshooting]] for other help).

## Mac

1. In Finder, choose _Go > Go to Folder..._ in the menu
2. Enter `~/Library/Logs/DiagnosticReports`
3. Look for a file that says "Brackets" in the name, with a timestamp matching the time of the crash
4. Post the file's contents in a [Gist](https://gist.github.com) and include the link in your bug report (or zip up the files and share them privately)

## Windows

If you want steps 1-5 done for you automatically, you can [go to this GitHub help page](https://help.github.com/articles/getting-a-crash-dump) and download and run the file from the link that says "this file."

1. On the Start menu, search for and run the "regedit" app
2. Navigate down the tree and select: "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\Windows Error Reporting"
3. If there is no "LocalDumps" folder, choose Edit > New > Key to create it
4. Inside this folder, choose Edit > New > DWORD.  Enter `DumpType` for the name, then double-click it and enter `1` for the value
5. Similarly, choose Edit > New > DWORD with the name `DumpCount` and the value `10`
6. Close regedit and restart your computer (or at least fully log off and log back on, which is almost as much work)
7. Run Brackets until you see the problem again
8. In a Windows folder view, enter ` %LOCALAPPDATA%\CrashDumps` as the path
9. Look for a .dmp file whose timestamp matches when you saw the problem.
10. Upload this file somewhere and include the link in your bug report (or zip up the file and share it privately)