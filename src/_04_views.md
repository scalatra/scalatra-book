Views
=====

Views can either be constructed within your Scala classes with HTML or using the ScalateSupport helper.

Inline HTML
-----------

{pygmentize:: scala}
def get("/") {
  contentType="text/html"

  <html>
  <head><title>Test</title></head>
  <body>Test Body for {uri("/")}</body>
  </html>
}
{pygmentize}

ScalateSupport
--------------

Including a basic template to be returned to the browser.

Basic usage
{pygmentize:: scala}
def get("/") {
  contentType="text/html"

  layoutTemplate("/WEB-INF/views/index.ssp")
}
{pygmentize}

Choosing a different template
{pygmentize:: scala}
def get("/") {
  contentType="text/html"

  layoutTemplate("/WEB-INF/views/index.ssp",("layout" -> "/WEB-INF/layouts/app.ssp"))
}
{pygmentize}

Each possible kind of Scalate template (mustache, scaml, jade, ssp) has a corresponding 
helper which can be used to find the template file, without a suffix, and without the 
"WEB-INF" part of the path. The above example can be written as:

{pygmentize:: scala}
def get("/") {
  contentType="text/html"

  ssp("/index",("layout" -> "/layouts/app"))
}
{pygmentize}

Passing parameters

Parameters may be passed to your templates using a Seq(String, Any) after the path 
to the template file. The simplest example might look like this:

{pygmentize:: scala}
def get("/") {
  contentType="text/html"

  layoutTemplate("/WEB-INF/views/index.ssp", "foo" -> "uno", "bar"->"dos")
}
{pygmentize}

Putting it all together, in a scaml example (alternatively use mustache, ssp, or jade):
{pygmentize:: scala}
def get("/") {
  contentType="text/html"

  scaml("/index", "layout" -> "/layouts/app", "foo" -> "uno", "bar"->"dos")
}

The "layout" key is somewhat special, as it's used by scalate to identify the 
layout file. 

If you want, you can set off your "layout" parameter from the others, perhaps by doing 
something like this (in ssp this time):

{pygmentize:: scala}
def get("/") {
  contentType="text/html"

  ssp("/index",("layout" -> "/layouts/app"), "foo" -> "uno", "bar"->"dos")
}
{pygmentize}
