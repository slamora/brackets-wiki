1. Install Node 0.8.x http://nodejs.org/download/
2. Run ``npm install -g grunt-cli`` to install the GruntJS command line interface
3. Run ``npm install`` from the root of the brackets git repo
4. Run ``grunt``

# Development Task Details

* ``grunt jshint`` Run JSHINT on all ``/src`` and ``/test`` files as well as the ``Gruntfile.js``
* ``grunt jasmine`` Run headless Jasmine tests
* ``grunt test`` Run JSHINT and Jasmine if JSHINT completes without errors
* ``grunt watch`` Watch for file changes, then run JSHINT and Jasmine

# Misc. Tasks

* ``grunt write-config`` Automatically run after ``npm install`` to update ``src/config.json``
* ``grunt install`` See ``write-config``