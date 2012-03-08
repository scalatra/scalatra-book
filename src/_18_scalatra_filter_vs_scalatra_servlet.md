ScalatraServlet vs. ScalatraFilter
==================================

There are two base classes you can inherit from in order to make a 
Scalatra application: `ScalatraServlet` and `ScalatraFilter`.


The main difference is the default behavior when a route is not found.
A filter will delegate to the next filter or servlet in the chain (as
configured by web.xml), whereas a ScalatraServlet will return a 404
response.

Another difference is that ScalatraFilter matches routes relative to
the WAR's context path. ScalatraServlet matches routes relative to the
servlet path. This allows you to mount multiple servlets under in
different namespaces in the same WAR.

### Use ScalatraFilter if:

* You are migrating a legacy application inside the same URL space
* You want to serve static content from the WAR rather than a
  dedicated web server

### Use ScalatraServlet if:

* You want to match routes with a prefix deeper than the context path. 