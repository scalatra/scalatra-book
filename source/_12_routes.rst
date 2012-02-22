Routes
======

In Scalatra, a route is an HTTP method paired with a URL matching
pattern.

.. code-block:: scala

    get("/") { 
      // show something 
    }

    post("/") { 
      // submit/create something 
    }

    put("/") { 
      // update something 
    }

    delete("/") { 
      // delete something 
    }

Route order
-----------

The first matching route is invoked. Routes are matched from the bottom
up. *This is the opposite of Sinatra.* Route definitions are executed as
part of a Scala constructor; by matching from the bottom up, routes can
be overridden in child classes.

Path patterns
-------------

Path patterns add parameters to the ``params`` map. Repeated values are
accessible through the ``multiParams`` map.

Named parameters
~~~~~~~~~~~~~~~~

Route patterns may include named parameters:

.. code-block:: scala

    get("/hello/:name") {
      // Matches "GET /hello/foo" and "GET /hello/bar"
      // params("name") is "foo" or "bar"
      <p>Hello, {params("name")}</p>
    } 

Wildcards
~~~~~~~~~

Route patterns may also include wildcard parameters, accessible through
the ``splat`` key.

.. code-block:: scala

    get("/say/*/to/*) {
      // Matches "GET /say/hello/to/world"
      multiParams("splat") // == Seq("hello", "world")
    }

    get("/download/*.*) {
      // Matches "GET /download/path/to/file.xml"
      multiParams("splat") // == Seq("path/to/file", "xml")
    }

Regular expressions
~~~~~~~~~~~~~~~~~~~

The route matcher may also be a regular expression. Capture groups are
accessible through the ``captures`` key.

.. code-block:: scala

    get("""^\/f(.*)/b(.*)""".r) {
      // Matches "GET /foo/bar"
      multiParams("captures") // == Seq("oo", "ar") 
    }

Rails-like pattern matching
~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default, route patterns parsing is based on Sinatra. Rails has a
similar, but not identical, syntax, based on Rack::Mount's Strexp. The
path pattern parser is resolved implicitly, and may be overridden if you
prefer an alternate syntax:

.. code-block:: scala

    import org.scalatra._

    class RailsLikeRouting extends ScalatraFilter {
      implicit override def string2RouteMatcher(path: String) =
        RailsPathPatternParser(path)

      get("/:file(.:ext)") { // matched Rails-style }
    }

Path patterns in the REPL
~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to experiment with path patterns, it's very easy in the
REPL.

.. code-block:: scala

    scala> import org.scalatra.SinatraPathPatternParser
    import org.scalatra.SinatraPathPatternParser

    scala> val pattern = SinatraPathPatternParser("/foo/:bar")
    pattern: PathPattern = PathPattern(^/foo/([^/?#]+)$,List(bar))

    scala> pattern("/y/x") // doesn't match 
    res1: Option[MultiParams] = None

    scala> pattern("/foo/x") // matches
    res2: Option[MultiParams] = Some(Map(bar -> ListBuffer(x)))

Alternatively, you may use the ``RailsPathPatternParser`` in place of
the ``SinatraPathPatternParser``.

Conditions
----------

Routes may include conditions. A condition is any expression that
returns Boolean. Conditions are evaluated by-name each time the route
matcher runs.

.. code-block:: scala

    get("/foo") {
      // Matches "GET /foo"
    }

    get("/foo", request.getRemoteHost == "127.0.0.1") {
      // Overrides "GET /foo" for local users
    }

Multiple conditions can be chained together. A route must match all
conditions:

.. code-block:: scala

    get("/foo", request.getRemoteHost == "127.0.0.1", request.getRemoteUser == "admin") {
      // Only matches if you're the admin, and you're localhost
    }

No path pattern is necessary. A route may consist of solely a condition:

.. code-block:: scala

    get(isMaintenanceMode) {
      <h1>Go away!</h1>
    }

Actions
-------

Each route is followed by an action. An Action may return any value,
which is then rendered to the response according to the following rules:

.. raw:: html

   <dl>
     <dt>

``Array[Byte]``

.. raw:: html

   </dt>
     <dd>

If no content-type is set, it is set to ``application/octet-stream``.
The byte array is written to the response's output stream.

.. raw:: html

   </dd>

.. raw:: html

   <dt>

``NodeSeq``

.. raw:: html

   </dt>
     <dd>

If no content-type is set, it is set to``text/html``. The node sequence
is converted to a string and written to the response's writer.

.. raw:: html

   </dd>

.. raw:: html

   <dt>

``Unit``

.. raw:: html

   </dt>
     <dd>

This signifies that the action has rendered the entire response, and no
further action is taken.

.. raw:: html

   </dd>

.. raw:: html

   <dt>

Any

.. raw:: html

   </dt>
     <dd>

For any other value, if the content type is not set, it is set to
``text/plain``. The value is converted to a string and written to the
response's writer

.. raw:: html

   </dd>

.

.. raw:: html

   </dl>

This behavior may be customized for these or other return types by
overriding ``renderResponse``.
