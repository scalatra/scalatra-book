Scalatra Examples
=================

The Scalatra code base contains [examples][examples] which you can easily run
yourself. This is a great way to dissect some running code for common tasks,
including:

[examples]: https://github.com/scalatra/scalatra/tree/develop/example/src/main

* parameters
* form submission
* file uploading
* flash scope
* login / logout actions
* filters
* cookies
* chat (Atmosphere-based Meteor chat)
* chat (Servlet 3.0-based async chat)

To run the code, do the following:

    $ git clone https://github.com/scalatra/scalatra.git
    $ cd scalatra
    $ sbt
    # now you're in the sbt shell!
    > project scalatra-example
    > container:start

You should then get a website with examples running at http://localhost:8080/
(make sure you're not already running your own project on that port!).

Example code can be found in the ```example/src/main/scala/org/scalatra/```
directory. 
