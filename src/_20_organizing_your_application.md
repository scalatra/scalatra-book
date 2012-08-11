Organizing your application
===========================

The recommended way of structuring a Scalatra project is as follows:

    project

    src
    |_ main
       |_ scala
       |  |   |_Scalatra.scala <= see note below!
       |  |
       |  |_org
       |      |_yourdomain
       |        |
       |        |_projectname
       |          |_ ArticlesServlet.scala
       |          |_ UsersServlet.scala
       |
       |_ webapp
          |
          |_ WEB-INF
             |
             |_ views
             |  |_ default.jade
             |
             |_ layouts
             |  |_ default.jade
             |
             |_ web.xml


### Mounting multiple servlets (or filters)

If you've got more than one servlet or filter in your application, you'll
need to mount them. If you're an old Java hand, you'll be quite comfortable
doing this through the `web.xml` file in traditional servlet style, but
Scalatra 2.1.x also introduces a more dynamic (and less XML-ish) method
of wiring your app together.

You can set your web.xml file up like this:

{pygmentize:: xml}
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
      version="3.0">
    <listener>
      <listener-class>org.scalatra.servlet.ScalatraListener</listener-class>
    </listener>

    <servlet-mapping>
      <servlet-name>default</servlet-name>
      <url-pattern>/img/*</url-pattern>
      <url-pattern>/css/*</url-pattern>
      <url-pattern>/js/*</url-pattern>
      <url-pattern>/assets/*</url-pattern>
    </servlet-mapping>
</web-app>
{pygmentize}

Note that there are no servlet-names, servlet classes, etc. It's all handled
dynamically by the ScalatraListener class.

You can then place a file called Scalatra.scala in your `src/main/scala`
directory. The simplest version of this file might look like:

{pygmentize:: scala}
import org.scalatra.LifeCycle
import javax.servlet.ServletContext
import org.yourdomain.projectname._

class Scalatra extends LifeCycle {

  override def init(context: ServletContext) {
    context mount (new ArticlesServlet, "/articles/*")
    context mount (new UsersServlet, "/users/*")
  }
}
{pygmentize}

This Scalatra class allows you to mount either servlets or filters (or both)
into your application, and define path patterns that they'll respond to.

It's also a good place to put things like database initialization code, which
need to be set up once in your application.


### Static Files

Static files can be served out of the `webapp` folder, which acts as the ROOT
directory. As with any servlet based application, the contents of this directory
are all public, with the exception of files in the WEB-INF directory.

An example structure may help in understanding this.

    src
    |_ main
       |_ scala
       |  |_ Web.scala
       |_ webapp
          |_ WEB-INF
          |  |_ secret.txt
          |  |_ views
          |  |  |_ default.jade
          |  |
          |  |_ layouts
          |  |  |_ default.jade
          |  |
          |  |_ web.xml
          |- stylesheets
          |    |_ default.css
          |- images
               |_ foo.jpg


In this application, the only publicly accessible files will be at
`stylesheets/default.css` and `images/foo.jpg`. Everything else will be
protected by the web application container.


### ScalatraServlet vs. ScalatraFilter

There are two base classes you can inherit from in order to make a
Scalatra application: `ScalatraServlet` and `ScalatraFilter`.

{pygmentize:: scala}
class YourServlet extends ScalatraServlet with ScalateSupport {
  // your class here
}
{pygmentize}

vs.

{pygmentize:: scala}
class YourFilter extends ScalatraFilter with ScalateSupport {
  // your class here
}
{pygmentize}

The main difference is the default behavior when a route is not found.
A `ScalatraFilter` will delegate to the next filter or servlet in the chain (as
configured by web.xml), whereas a `ScalatraServlet` will return a 404
response.

Another difference is that `ScalatraFilter` matches routes relative to
the WAR's context path. `ScalatraServlet` matches routes relative to the
servlet path. This allows you to mount multiple servlets in different namespaces
in the same WAR.

#### Use ScalatraFilter if:

* You are migrating a legacy application inside the same URL space
* You want to serve static content from the WAR rather than a
  dedicated web server

#### Use ScalatraServlet if:

* You want to match routes with a prefix deeper than the context path.
