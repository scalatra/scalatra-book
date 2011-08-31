Introduction
=============

What is Scalatra?
----------------

Scalatra is a tiny, [Sinatra](http://www.sinatrarb.com/)-like web framework for [Scala](http://www.scala-lang.org/).

Getting Started
---------------
The simplest way to get started with Scalatra is to generate a project with [giter8](http://github.com/n8han/gitter8):

{pygmentize::}
$ g8 scalatra/scalatra-sbt
> organization [com.example]: 
> name ["scalatra-sbt-prototype"]: scalatra-example
> servlet_name [MyScalatraFilter]:
> scala_version [2.9.0-1]:
> version [1.0]:
{pygmentize}

This creates a new Scalatra project in `scalatra-example`.

### Git clone alternative

For those who prefer not to instal giter8, an equivalent prototype project is
maintained on GitHub:

{pygmentize::}
$ git clone http://github.com/scalatra/scalatra-sbt-prototype.git
{pygmentize}

You will need to manually update organization, name, and version in `build.sbt`.

### Dependencies

Your `build.sbt` file declares a few dependencies:

* `"org.scalatra" %% "scalatra" % "2.0.0.RC1"`: This is the core Scalatra module,
  and is required to run the framework.

* `"org.scalatra" %% "scalatra-scalate" % "2.0.0.RC1"`: This integrates with 
  [Scalate](http://scalate.fusesource.org), a template engine supporting multiple
  template formats.  This is optional, but highly recommended for any app requiring
  templating.

* `"org.eclipse.jetty" % "jetty-webapp" % "7.4.5.v20110725" % "jetty"`: This is the
  embedded servlet container used by the web plugin.  Your application should be
  portable to any servlet container supporting at least the 2.5 specification.

* `"javax.servlet" % "servlet-api" % "2.5" % "provided"`: Required for building your
  app.  It is placed in the provided configuration so that it is not bundled with
  your application.  Your servlet container will provide this at deployment time.

You may add any other dependencies you wish in this section.

### Building

The prototype application uses [sbt >= 0.10](http://github.com/harrah/xsbt) for
its build system.


Hello World Application
-----------------------

Scalatra is installed, how about making your first application?

{pygmentize:: scala}
     import org.scalatra._
     import java.net.URL
     import scalate.ScalateSupport

     class MyScalatraFilter extends ScalatraFilter with ScalateSupport {

       get("/") {
         <html>
           <body>
             <h1>Hello, world!</h1>
             Say <a href="hello-scalate">hello to Scalate</a>.
           </body>
         </html>
       }
     }
{pygmentize}
     
Run this application by `$ ./sbt jetty-run` and load
`http://localhost:8080` in your browser.

As you can see, Scalatra doesn't force you to setup much infrastructure: a
request to a URL evaluates some Scala code and returns some text in response.
Whatever the block returns is sent back to the browser.


Real World Applications in Scalatra
----------------------------------

### LinkedIn Signal

Scalatra is used for their backend REST services.

### ChaCha

ChaCha is using it in multiple internal applications.

### The Guardian

Scalatra is used in a number of systems that help power [The Guardian](http:///www.guardian.co.uk/), for instance the [music artist pages](http://www.guardian.co.uk/info/developer-blog/2011/jun/23/internet).

Check out a full list of Scalatra apps [in the wild][in-the-wild].

[in-the-wild]: http://www.scalatra.org/built-with.html

About this book
---------------
This book will assume you have a basic knowledge of the Scala scripting language.

For more information about the Scala language visit the following links:

* [scala-lang](http://www.scala-lang.org/)

Need Help? 
----------

The Scalatra club is small, but super-friendly.  Join us on IRC at
irc.freenode.org in #scalatra if you have any questions.  It's a bit
slow at times, so give us a bit to get back to your questions.
