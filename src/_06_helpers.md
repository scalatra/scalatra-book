Helpers
=======

Scalate
-------

[Scalate](http://scalate.fusesource.org/) is a template engine for 
generating text and markup. It is the default rendering engine included
as a helper trait.

Once you have the `ScalateSupport` trait applied you can call 
`templateEngine.layout('index.page')` within your action handlers.

{pygmentize:: scala}
class MyScalatraFilter extends ScalatraFilter with ScalateSupport {
  notFound {
    // If no route matches, then try to render a Scaml template
    val templateBase = requestPath match {
      case s if s.endsWith("/") => s + "index"
      case s => s
    }
    val templatePath = "/WEB-INF/scalate/templates/" + templateBase + ".scaml"
    servletContext.getResource(templatePath) match {
      case url: URL => 
        contentType = "text/html"
        templateEngine.layout(templatePath)
      case _ => 
        filterChain.doFilter(request, response)
    } 
  }
}
{pygmentize}

### Scalate error page

Mixing in ScalateSupport enables the Scalate error page for any uncaught
exceptions.  This page renders the template source with the error highlighted.
To disable this behavior, override `isScalateErrorPageEnabled`:

{pygmentize:: scala}
override def isScalatePageEnabled = false
{pygmentize}

Scentry + Authentication
------------------------

Scentry is a user submitted authentication scheme. Combined 
`ScentrySupport` and `BasicAuthSupport` traits allow you to quickly tie a
User class to the session and Basic Authentication methods.

There is a new authentication middleware in the auth directory, to be documented soon.  See an example at [usage example](http://gist.github.com/660701).
Another [example](https://gist.github.com/732347) for basic authentication can be found

To use it from an SBT project, add the following to your project:

{pygmentize:: scala}
val auth = "org.scalatra" %% "scalatra-auth" % scalatraVersion
{pygmentize}

### User Password

The User Password is the most common form/ajax/mixed strategy used in 
standard application development. 

TODO: Add example

### Remember Me

Remember Me is a common strategy that can be applied to your application.
It allows your users to return to your site already logged into their 
account by validating a secure token.


TODO: Add example

### Basic Authentication

Basic Authentication is the simplest authentication method. Due to the 
constraints of Basic Authentication it is recommended that people utilize
the User Password strategy if they need to have better control over user
interaction.

TODO: Add example




Flash Map
---------

Flash support is included within Scalatra by default. Flash entries are not
normally available within the current request scope. The exception is adding
new flash entries into `flash.now`.


TODO: Add better supporting examples

{pygmentize:: scala}
flash += ("error" -> "An error occurred")
flash.now += ("info" -> "redirect to see the error")
{pygmentize}

File Upload
-----------

Scalatra provides optional support for file uploads with Apache Commons [FileUpload](http://commons.apache.org/fileupload/).

1. Depend on scalatra-fileupload.jar.  In your SBT build:

{pygmentize:: scala}
val scalatraFileUpload = "org.scalatra" %% "scalatra-fileupload" % scalatraVersion
{pygmentize}

2. Extend your application with `FileUploadSupport`:

{pygmentize:: scala}
import org.scalatra.ScalatraServlet
import org.scalatra.fileupload.FileUploadSupport

class MyApp extends ScalatraServlet with FileUploadSupport {
  // ...
}
{pygmentize}

3. Be sure that your form is of type `multipart/form-data`:

{pygmentize:: scala}
get("/") {
  <form method="post" enctype="multipart/form-data">
    <input type="file" name="foo" />
    <input type="submit" />
  </form>
}
{pygmentize}

4. Your files are available through the `fileParams` or `fileMultiParams` maps:

{pygmentize:: scala}
post("/") {
  processFile(fileParams("file"))
}
{pygmentize}

Anti-XML integration
--------------------

Scalatra provides optional [Anti-XML](http://anti-xml.org/) integration:

1. Depend on scalatra-anti-xml.jar.  In your SBT build:

{pygmentize:: scala}
val scalatraAntiXml = "org.scalatra" %% "scalatra-anti-xml" % scalatraVersion
{pygmentize}

2. Extend your application with `AntiXmlSupport`

{pygmentize:: scala}
import org.scalatra.ScalatraServlet
import org.scalatra.antixml.AntiXmlSupport
import com.codecommit.antixml._

class MyApp extends ScalatraServlet with AntiXmlSupport {
  // ...
}
{pygmentize}

3. Actions results of type `com.codecommit.antixml.Elem` will be serialized
to the response body, and a content type of `text/html` will be inferred if
none is set.

{pygmentize:: scala}
get("/") {
  XML.fromString("""<foo bar="baz"></foo>""")
}
{pygmentize}

URL Support and Reverse Routes
------------------------------

UrlSupport provides two instances that provide you with relative url's. UrlSupport.url will return a string that can be used in your output or a redirect statement.

1. Page relative url
{pygmentize:: scala}
get("/"){
  // This will redirect to http://<host>/page-relative
  redirect(url("page-relative")) 
}
{pygmentize}

2. Context relative url
{pygmentize:: scala}
get("/"){
  // This will redirect to http://<host>/<context>/context-relative
  redirect(url("/context-relative")) 
}
{pygmentize}

3. Mapped params
{pygmentize:: scala}
get("/") {
  // This will redirect to http://<host>/<context>/en-to-es?one=uno&two=dos
  redirect( url("/en-to-es", Map("one" -> "uno", "two" -> "dos")) )
}
{pygmentize}

TODO: add reverse routing

AkkaSupport
---------------------------------------------

Provides a mechanism for adding Akka futures to your routes.

{pygmentize:: scala}
import _root_.akka.dispatch._
import org.scalatra.akka.AkkaSupport

class MyAppServlet extends ScalatraServlet with AkkaSupport {
  get("/"){
    Future {
      // Add other logic here
      
      <html><body>Hello Akka</body></html>
    }		
  }
}
{pygmentize}

