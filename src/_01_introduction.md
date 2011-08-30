Introduction
=============

What is Scalatra?
----------------

Scalatra is a tiny, [Sinatra](http://www.sinatrarb.com/)-like web framework for [Scala](http://www.scala-lang.org/).

Installation
------------
The simplest way to install Scalatra is through g8:

     scalatra/scalatra-sbt

### Dependencies

Scalatra 

Scalatra supports many different template engines (it uses the Scalate library
internally to support practically every template engine)


### Scalatra+Giter8 Living on the Edge

The 2.0 version of Scalatra lives in its Git repository, available at 
**<http://github.com/scalatra/scalatra/tree/master>**.

You can use the 2.0 version to try new functionality or to contribute to the
framework. You need to have [Git version control
software](http://www.git-scm.com) and [giter8](https://github.com/n8han/giter8#readme).

{pygmentize::}
     $ g8 scalatra/scalatra-sbt
     > organization [com.example]: 
     > name ["scalatra-sbt-prototype"]: scalatra-example
     > servlet_name [MyScalatraFilter]:
     > scala_version [2.9.0-1]:
     > version [1.0]:
{pygmentize}

To use Scalatra 2.0 with SBT, you'll have to modify your Project file to include.

{pygmentize:: scala}
val scalatraVersion = "2.0.0-SNAPSHOT"
val scalatra = "org.scalatra" %% "scalatra" % scalatraVersion
{pygmentize}

### Scalatra Git Clone

The 2.0 project template can also be cloned into a new project using the commands listed below.

Clone this repository:

{pygmentize::}
          $ git clone git://github.com/scalatra/scalatra-sbt-prototype.git my-app
          $ cd my-app
          $ ./sbt
          > update
          > jetty-run
          > ~prepare-webapp
{pygmentize}

   Go to http://localhost:8080/.

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

Check out a full list of Scalatra apps [in the wild][in-the-wild].

[in-the-wild]: http://www.scalatra.org/wild

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
