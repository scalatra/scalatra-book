Understanding Scalatra's SBT Dependencies
=========================================

Scalatra uses Scala's [Simple Build Tool][sbt-site], or `sbt`, as a build system.

[sbt-site]: http://www.scala-sbt.org/

The `build.sbt` file defines the libraries which your application will depend on,
so that `sbt` can download them for you and build your Scalatra project. 

Here's an example Scalatra sbt file:

{pygmentize:: scala}
organization := "org.example"

name := "yourapp"

version := "0.1.0-SNAPSHOT"

scalaVersion := "2.9.1"

seq(webSettings :_*)

libraryDependencies ++= Seq(
  "org.scalatra" % "scalatra" % "2.1.0-RC1",
  "org.scalatra" % "scalatra-scalate" % "2.1.0-RC1",
  "org.scalatra" % "scalatra-specs2" % "2.1.0-RC1" % "test",
  "ch.qos.logback" % "logback-classic" % "1.0.6" % "runtime",
  "org.eclipse.jetty" % "jetty-webapp" % "8.1.3.v20120416" % "container",
  "javax.servlet" % "javax.servlet-api" % "3.0.1" % "provided"
)

ivyXML :=
  <dependencies>
    <exclude org="org.eclipse.jetty.orbit" />
  </dependencies>


resolvers += "Sonatype OSS Snapshots" at "http://oss.sonatype.org/content/repositories/snapshots/"
{pygmentize}

If your project depends on any additional libraries, you may add any other
dependencies you wish into the `libraryDependencies` section.

The default dependencies are:

`scalatra`: This is the core Scalatra module, and is required to run the framework.

`scalatra-scalate`: This integrates with [Scalate](http://scalate.fusesource.org),
a template engine supporting multiple template formats.  This is optional, but
highly recommended for any app requiring templating.

`scalatra-specs2`: This integrates the [Specs2][specs2] testing libraries.

`logback-classic`: Basic logging functionality, courtesy of [Logback][qos-ch].

`jetty-webapp`: This is the embedded servlet container used by the web plugin.
Your application should be portable to any servlet container supporting at least
the 2.5 specification.

`servlet-api`: Required for building your app.  It is placed in the `provided`
configuration so that it is not bundled with your application.  Your servlet
container will provide this at deployment time.

[specs2]: https://github.com/etorreborre/specs2
[qos-ch]: http://logback.qos.ch/

