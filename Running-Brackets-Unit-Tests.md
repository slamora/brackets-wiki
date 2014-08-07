# Running Brackets Unit Tests

Unit Tests can only be run from a cloned version of Brackets -- not from an installer build.

Use `Debug > Run Tests` to invoke the Jasmine SpecRunner dialog which looks something like this:

![Jasmine SpecRunner dialog](http://i.imgur.com/ZzozdSA.png)

You can try to run "All" test from the "All" tab, but due to memory constraints,
usually all of the tests don't pass. Here's a more reliable recipe:

1. Switch to "Unit" tab and click "All" to run all tests
2. Switch to "Integration" tab and run "All"
3. Close SpecRunner dialog, shutdown/restart Brackets, `Debug > Runs Tests`
4. Switch to "Live Preview" tab and run "All"
5. Switch to "Performance" tab and run "All"
6. Switch to Extensions" tab and run "All"

If any tests fail, then try running only that suite of tests.
If all tests for that suite pass, then that's OK -- this is most likely a timing problem.
If any tests consistently fail, open an Issue with High Priority.
