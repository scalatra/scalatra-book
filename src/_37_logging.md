
Logging
=======

You can easily add logging facilities to your project, if you're using the
default logging dependency in your `build.sbt` file:

      "ch.qos.logback" % "logback-classic" % "1.0.0" % "runtime"

To add a logger to your project, put a companion object next to your servlet:

{pygmentize:: scala}
object YourServlet {
  val logger = LoggerFactory getLogger(UploadServlet getClass)
}
{pygmentize}

Then in your servlet or filter class, you can do something like this:

{pygmentize:: scala}
class YourServlet extends ScalatraServlet {
  val logger = YourServlet.logger

  def get("/") {
    logger.info("foo")
    // whatever else you want to put in the body of the action
  }
}
{pygmentize}
