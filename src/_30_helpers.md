Helpers
=======

Helpers exist as traits in Scalatra that can applied to your base class. 

[scalate]: http://scalate.fusesource.org
[views]: http://www.scalatra.org/stable/book/#Views

### HttpServletRequest

The request is available through the `request` variable.  The request is 
implicitly extended with the following methods:

1. `body`: to get the request body as a string
2. `isAjax`: to detect AJAX requests
3. `cookies` and `multiCookies`: a Map view of the request's cookies
4. Implements `scala.collection.mutable.Map` backed by request attributes

### HttpServletResponse

The response is available through the `response` variable. If you override 
the Scalatra handling and write directly to the response object 
(Ex: response.getOutputStream), then your action should return Unit() to 
prevent a conflict with multiple writes.

### HttpSession

Scalatra has session handling built into the framework by default. There are 
no modules or traits that you need to include. 

However, sessions are *off* until the `session` method is called. 
Once `session` is called, a cookie-based session will be created. 

Then you will be able to use the default cookie based session handler in your
application:

{pygmentize:: scala}
get("/") {
  if(session.contains("counter")) session("counter") = 0
  session("counter") = session("counter").toInt + 1
  "You've hit this page %s times!" format session("counter").toInt
}
{pygmentize}

The `session` implicitly implements `scala.collection.mutable.Map` backed by 
`session` attributes.  

There's also a `sessionOption` method, which returns a `session` if one already 
exists, and `None` if it doesn't. This is a good way to avoid creating a 
`session`.

If you don't need a `session` in your application, e.g. if you're building out
a stateless RESTFul API, never call the `session` method, and don't mix in 
`FlashMapSupport` (which also creates a session). 

The default session in Scalatra is cookie-based, but the cookie is used only 
as a session identifier (session data is stored server-side). If you are 
building out a shared-nothing architecture, this is something to be aware of.

### ServletContext

The servlet context is available through the `servletContext` variable.  The 
servlet context implicitly implements `scala.collection.mutable.Map` backed 
by servlet context attributes.

### Configuration

The environment is defined by:

1. The `org.scalatra.environment` system property. 
2. The `org.scalatra.environment` init parameter.
3. The default is `development`.

If the environment starts with "dev", then `isDevelopmentMode` returns true.  
This flag may be used by other modules, for example, to enable the Scalate 
console.

You can set the application's environment in the following ways:

1. As a system property: this is most commonly set with a `-D` option on the
command line: `java -Dorg.scalatra.environment=development`
2. As an init-param in web.xml:

{pygmentize:: xml}
<servlet>
  <init-param>
    <param-name>org.scalatra.environment</param-name>
    <param-value>development</param-value>
  </init-param>
</servlet>
{pygmentize}

In development mode, a few things happen.

1. In a ScalatraServlet, the notFound handler is enhanced so that it dumps the 
effective request path and the list of routes it tried to match. This does not 
happen in a ScalatraFilter, which just delegates to the filterChain when no 
route matches.
2. Meaningful error pages are enabled (e.g. on 404s, 500s).
3. The [Scalate console][console] is enabled.

[console]: http://scalate.fusesource.org/documentation/console.html

### Scalate error page

Mixing in ScalateSupport enables the Scalate error page for any uncaught
exceptions.  This page renders the template source with the error highlighted.
To disable this behavior, override `isScalateErrorPageEnabled`:

{pygmentize:: scala}
override def isScalatePageEnabled = false
{pygmentize}

### Scentry + Authentication

Scentry is a user submitted authentication scheme. Combined 
`ScentrySupport` and `BasicAuthSupport` traits allow you to quickly tie a
User class to the session and Basic Authentication methods.

There is a new authentication middleware in the auth directory, to be 
documented soon.  See an example at 
[usage example](http://gist.github.com/660701).
Another [example](https://gist.github.com/732347) for basic authentication 
can be found

#### Dependency

{pygmentize:: scala}
// Put this in build.sbt:
"org.scalatra" %% "scalatra-auth" % "2.0.4"
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

### Flash Map

Flash support, allowing you to store information across requests and expire
it immediately after a redirect, is included within Scalatra by default. 

Flash entries are not normally available within the current request scope. 
The exception is adding new flash entries into `flash.now`.

In order to enable flash support, you'll need to extend your servlet class
with `FlashMapSupport`. You can set the flash like this:

{pygmentize:: scala}
class FooServlet extends ScalatraServlet with FlashMapSupport {
 post("/articles/create") {
    // watch out: this creates a session!
    flash("notice") = "article created succesfully"
    redirect("/home")
  }

  get("/home") {
    ssp("/home")
  }
}
{pygmentize}

and then you can use it in your view, to let the user know what happened. 
`home.ssp` might look like this:

{pygmentize:: html}
<html>
  <body>
    <p>Here is the flash: <%= flash.get("notice") %></p>
  </body>
</html>
{pygmentize}

You can add more than one entry to the `FlashMap`, using `+=`:

{pygmentize:: scala}
flash = ("notice" -> "Hello from your application")
flash += ("error" -> "An error occurred")
flash.now += ("info" -> "redirect to see the error")
{pygmentize}

`flash.now` operates pretty much in the same way as `flash` does, except that 
it sets a value for the current request only.  It will be removed before the 
next request unless explicitly kept.

Please note: extending your ScalatraServlet with `FlashMapSupport` triggers 
a session, which is why it's an optional mixin. 

### File Upload

Scalatra provides optional support for file uploads with Apache Commons 
[FileUpload](http://commons.apache.org/fileupload/).

#### Dependency

{pygmentize:: scala}
// Put this in build.sbt:
"org.scalatra" %% "scalatra-fileupload" % "2.0.4"
{pygmentize}

Extend your application with `FileUploadSupport`:
{pygmentize:: scala}
import org.scalatra.ScalatraServlet
import org.scalatra.fileupload.FileUploadSupport

class MyApp extends ScalatraServlet with FileUploadSupport {
  // ...
}
{pygmentize}

Be sure that your form is of type `multipart/form-data`:
{pygmentize:: scala}
get("/") {
  <form method="post" enctype="multipart/form-data">
    <input type="file" name="thefile" />
    <input type="submit" />
  </form>
}
{pygmentize}

Your files are available through the `fileParams` or `fileMultiParams` maps:
{pygmentize:: scala}
post("/") {
  processFile(fileParams("thefile"))
}
{pygmentize}

### Anti-XML integration

Scalatra provides optional [Anti-XML](http://anti-xml.org/) integration:

#### Dependency

{pygmentize:: scala}
// Put this in build.sbt:
"org.scalatra" %% "scalatra-anti-xml" % "2.0.4"
{pygmentize}

Extend your application with `AntiXmlSupport`:
{pygmentize:: scala}
import org.scalatra.ScalatraServlet
import org.scalatra.antixml.AntiXmlSupport
import com.codecommit.antixml._

class MyApp extends ScalatraServlet with AntiXmlSupport {
  // ...
}
{pygmentize}

Actions results of type `com.codecommit.antixml.Elem` will be serialized
to the response body, and a content type of `text/html` will be inferred if
none is set.

{pygmentize:: scala}
get("/") {
  XML.fromString("""<foo bar="baz"></foo>""")
}
{pygmentize}

### URL Support and Reverse Routes

UrlSupport provides two instances that provide you with relative URLs. 
`UrlSupport.url` will return a string that can be used in your output or a 
redirect statement.

#### Page relative url:
{pygmentize:: scala}
get("/"){
  // This will redirect to http://<host>/page-relative
  redirect(url("page-relative")) 
}
{pygmentize}

#### Context relative url:
{pygmentize:: scala}
get("/"){
  // This will redirect to http://<host>/<context>/context-relative
  redirect(url("/context-relative")) 
}
{pygmentize}

#### Mapped params:
{pygmentize:: scala}
get("/") {
  // This will redirect to http://<host>/<context>/en-to-es?one=uno&two=dos
  redirect( url("/en-to-es", Map("one" -> "uno", "two" -> "dos")) )
}
{pygmentize}

#### Reverse routes:

It is possible to save your routes as variables so that they have convenient 
handles:

{pygmentize:: scala}
class MyApp extends ScalatraServlet with UrlGeneratorSupport {
  // When you create a route, you can save it as a variable
  val viewUser = get("/user/:id") {
     // your user action would go here
   }

  post("/user/new") {
    // url method provided by UrlGeneratorSupport.  Pass it the route
    // and the params.
    redirect(url(viewUser, "id" -> newUser.id))
  }
}
{pygmentize}

There's also a `ScalateUrlGeneratorSupport`.  It reflectively finds all
members of your app of type Route (e.g., viewUser above) and makes them 
available in your templates.  You should then be able to do something like this 
right in your templates:

{pygmentize:: scala}
  url(viewUser, "id" -> 1) 
{pygmentize}

### AkkaSupport

TODO: this needs more documentation, re: is there support for Akka 1.x and/or 2.x?

TODO: add SBT dependencies. 

Provides a mechanism for adding [Akka][akka] futures to your routes. Akka support
is only available in Scalatra 2.1 and up. 

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

[akka]: http://akka.io/


