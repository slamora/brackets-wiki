Sprint DoD has a task for logging performance tests results. We have a small set of performance tests that run within the unit test window. Eventually these tests will be automated. For now, here are the instructions for logging the sprint performance results:

1. Confirm you have edit access to [Brackets Performance Tests](https://docs.google.com/spreadsheet/ccc?key=0Aras0diokeHxdEc5RGtOeVI0V0xGU3FPUXBuX3ZYTlE#gid=0) (Google Spreadsheet). If not, ask @jasonsanjose.
2. Get the performance laptop (get key from @pflynn's desk)
3. Uninstall any existing Brackets, and install the new Brackets build
4. Sync the brackets repo on the machine to match the SHA of the installed build (look for the version field in <path to install>\www\package.json) (note: you may need to open the SSH tunnel to update from GitHub)
5. Run ``tools\setup_for_hacking``
6. Delete the [[Cache Folder]]
7. Reboot the computer
8. Run Brackets
9. Debug > Show Performance Data
10. Log "Application Startup" time under "Startup > Cold Startup (first run, no prefs)"
11. Debug > Run Tests, Performance, All
12. Log "Performance Tests File open performance" under "Cold - Performance Tests File open performance"
13. Log the first set of "JavaScript Inline Editor Creation" results under "Cold - JSQuickEdit Performance site should open inline editors."
14. Log the last set of "JavaScript Inline Editor Creation" results under "Warm - JSQuickEdit Performance suite should open inline editors."
15. Exit and restart Brackets
16. Debug > Show Performance Data
17. Log "Application Startup" time under "Startup > Warm Startup"