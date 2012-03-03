Installation
============

### Installation using `giter8`

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

### Installation using `git clone`

For those who prefer not to install giter8, an equivalent prototype project is
maintained on GitHub:

{pygmentize::}
$ git clone http://github.com/scalatra/scalatra-sbt-prototype.git
{pygmentize}

You will need to manually update organization, name, and version in `build.sbt`.

### Dependencies

Your `build.sbt` file defines the libraries which your application will depend on.
It declares a few dependencies:

* `"org.scalatra" %% "scalatra" % "2.1.0-SNAPSHOT"`: This is the core Scalatra 
  module, and is required to run the framework.

* `"org.scalatra" %% "scalatra-scalate" % "2.1.0-SNAPSHOT"`: This integrates with 
  [Scalate](http://scalate.fusesource.org), a template engine supporting multiple
  template formats.  This is optional, but highly recommended for any app requiring
  templating.

* `"org.eclipse.jetty" % "jetty-webapp" % "8.0.4.v20111024" % "test;container"`: 
  This is the embedded servlet container used by the web plugin.  Your 
  application should be portable to any servlet container supporting at least
  the 2.5 specification.

* `"javax.servlet" % "servlet-api" % "3.0.1" % "provided"`: Required for 
  building your app.  It is placed in the provided configuration so that it 
  is not bundled with your application.  Your servlet container will provide 
  this at deployment time.

If your project depends on any additional libraries, you may add any other 
dependencies you wish in this section.

### Building

The prototype application uses [sbt >= 0.10](http://github.com/harrah/xsbt) 
for its build system. 

Enter your application's top-level directory and type `sbt`, and the site will
build itself.

For info installing and running sbt on your system, see [the sbt website][sbt-site].

[sbt-site]: http://www.scala-sbt.org/

Running in Development
======================

### Automatic Code Reloading

Restarting an application manually after every code change is both slow and
painful. It can easily be avoided by using a tool for automatic code reloading.

SBT will allow you to [signal a restart of the application when it detects
code changes](https://github.com/harrah/xsbt/wiki/Triggered-Execution). The
syntax for restarting includes adding `~` in front of the command you want to
re-execute.  To recompile and reload your application automatically with
xsbt-web-plugin 0.2.10, run the following:

{pygmentize::}
$ sbt
> container:start
> ~ ;copy-resources;aux-compile
{pygmentize}
