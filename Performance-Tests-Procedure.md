Sprint DoD has a task for logging performance tests results. We have a small set of performance tests that run within the unit test window. Eventually these tests will be automated. For now, here are the instructions for logging the sprint performance results:

1. Confirm you have edit access to [Brackets Performance Tests](https://docs.google.com/spreadsheet/ccc?key=0Aras0diokeHxdEc5RGtOeVI0V0xGU3FPUXBuX3ZYTlE#gid=0) (Google Spreadsheet). If not, ask @jasonsanjose.
2. Get the performance laptop (get key from @pflynn's desk)
3. Uninstall any existing Brackets and delete the [[Cache Folder]].
4. Install Brackets and use ``tools\setup_for_hacking``
5. Sync the brackets repo on the machine to match the SHA of the installed build (look in Help > About to find it)
6. Reboot the computer
7. Run Brackets
8. Debug > Show Performance Data
9. Log cold startup time under "Startup > Cold Startup (first run, no prefs)"
10. Debug > Run Tests, Performance, All
11. Log "Performance Tests File open performance" under "Cold - Performance Tests File open performance"
12. Log the first set of "JavaScript Inline Editor Creation" results under "Cold - JSQuickEdit Performance suite should open inline editors."
13. Log the last set of "JavaScript Inline Editor Creation" results under "Warm - JSQuickEdit Performance suite should open inline editors."
14. Exit and restart Brackets
15. Debug > Show Performance Data
16. Log warm startup time under "Startup > Warm Startup"