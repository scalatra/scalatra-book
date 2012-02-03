Introduction
============

What is Scalatra?
-----------------

Scalatra is a tiny, [Sinatra](http://www.sinatrarb.com/)-like web framework for [Scala](http://www.scala-lang.org/).

Introduction
------------

The simplest way to get started with Scalatra is to generate a project with
[giter8](http://github.com/n8han/giter8).  

{pygmentize::}
$ g8 scalatra/scalatra-sbt
> organization [com.example]: 
> package [com.example.app]: 
> name [scalatra-sbt-prototype]: 
> servlet_name [MyScalatraServlet]: 
> scala_version [2.9.1]: 
> version [0.1.0-SNAPSHOT]: 
{pygmentize}

* `organization`: Used for publishing.  Should be the reverse of a domain 
name you control.  If you don''t own a domain, `com.github.username` is a
popular choice.  

* `package`: All Scala code belongs in a package.  The [Scala Style
Guide](http://docs.scala-lang.org/style/naming-conventions.html#packages)
recommends that your packages start with your organization.  This convention is
used across multiple JVM languages and gives your project a globally unique
namespace.

* `name`: The name of your project.  g8 will generate a project into a
folder of this name, and the artifacts you publish will be based on this name.

* `servlet_name`: the name of your servlet.

* `scala_version`: The version of Scala your project is built with.  When in
doubt, use the default.

* `version`: The version number of your project.  This is entirely up to you,
but we like [Semantic Versioning](http://semver.org/).

### Git clone alternative

For those who prefer not to install giter8, an equivalent prototype project is
maintained on GitHub:

{pygmentize::}
$ git clone http://github.com/scalatra/scalatra-sbt-prototype.git
{pygmentize}

You will need to manually update organization, name, and version in `build.sbt`.

### Dependencies

Your `build.sbt` file declares a few dependencies:

* `"org.scalatra" %% "scalatra" % "2.1.0-SNAPSHOT"`: This is the core Scalatra module,
  and is required to run the framework.

* `"org.scalatra" %% "scalatra-scalate" % "2.1.0-SNAPSHOT"`: This integrates with 
  [Scalate](http://scalate.fusesource.org), a template engine supporting multiple
  template formats.  This is optional, but highly recommended for any app requiring
  templating.

* `"org.eclipse.jetty" % "jetty-webapp" % "8.0.4.v20111024" % "test;container"`: This is the
  embedded servlet container used by the web plugin.  Your application should be
  portable to any servlet container supporting at least the 2.5 specification.

* `"javax.servlet" % "servlet-api" % "3.0.1" % "provided"`: Required for building your
  app.  It is placed in the provided configuration so that it is not bundled with
  your application.  Your servlet container will provide this at deployment time.

You may add any other dependencies you wish in this section.

### Building

The prototype application uses [sbt >= 0.10](http://github.com/harrah/xsbt) for
its build system.

Hello World Application
-----------------------

Scalatra is installed, how about making your first application?  Source files
go into `src/main/scala/com/example/app` (substitute your package for
`com/example/app`).  Open
`src/main/scala/com/example/app/MyScalatraFilter.scala`:

{pygmentize:: scala}
package com.example.app

import org.scalatra._
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
     
Back at the shell prompt, from your project root, you can run the project
with:

{pygmentize::}
$ sbt
> container:start
{pygmentize}

The application starts on [http://localhost:8080](http://localhost:8080).  As
you can see, Scalatra doesn't force you to setup much infrastructure: a
request to a URL evaluates some Scala code and returns some text in response.
Whatever the block returns is sent back to the browser.

Real World Applications in Scalatra
-----------------------------------

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
