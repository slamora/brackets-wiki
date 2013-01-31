1. Install Node 0.8 http://nodejs.org/download/
2. Run ``npm install`` from the root of the brackets git repo
3. Run ``grunt``

# Task details

* ``grunt jshint`` Run JSHINT on all ``/src`` and ``/test`` files as well as the ``Gruntfile.js``
* ``grunt jasmine`` Run headless Jasmine tests
* ``grunt test`` Run JSHINT and Jasmine if JSHINT completes without errors
* ``grunt watch`` Watch for file changes, then run JSHINT and Jasmine