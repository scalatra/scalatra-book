
Testing
=======

Scalatra includes a test framework for writing the unit tests for your Scalatra application.  The framework lets you send requests to your app and examine the response.  It can be mixed into the test framework of your choosing; integration with [ScalaTest](http://www.scalatest.org/) and [Specs](http://code.google.com/p/specs/) is already provided.  ScalatraTests supports HTTP GET/POST tests with or without request parameters and sessions.  For more examples, please refer to core/src/test/scala.

ScalaTest
---------

### Dependencies

- scalatra-scalatest

### Code

Extend ScalatraSuite with your preferred Suite implementation.  You get ShouldMatchers and MustMatchers for free.

{pygmentize:: scala}
class MyScalatraServletTests extends ScalatraSuite with FunSuite {
  // `MyScalatraServlet` is your app which extends ScalatraServlet
  addServlet(classOf[MyScalatraServlet], "/*")

  test("simple get") {
    get("/path/to/something") {
      status should equal (200)
      body should include ("hi!")
    }
  }
}
{pygmentize}

Convenience traits are provided for many Suite implementations:

* ScalatraSpec
* ScalatraFlatSpec
* ScalatraFreeSpec
* ScalatraWordSpec
* ScalatraFunSuite
* ScalatraFeatureSpec
* ScalatraJUnit3Suite
* ScalatraJUnitSuite (JUnit 4)
* ScalatraTestNGSuite

Specs
-----

### Dependencies

- scalatra-specs

### Example

{pygmentize:: scala}
object MyScalatraServletTests extends ScalatraSpecification {
  addServlet(classOf[MyScalatraServlet], "/*")

  "MyScalatraServlet when using GET" should {
    "/path/to/something should return 'hi!'" in {
      get("/") {
        status mustEqual(200)
        body mustEqual("hi!")
      }
    }
  }
}
{pygmentize}


Specs2
------

### Example 
{pygmentize:: scala }
import org.scalatra.test.specs2._

class HelloWorldServletSpec extends ScalatraSpec { def is =
  "GET / on HelloWorldServlet"                     ^
    "return status 200"                            ! getRoot200^
                                                   end

  addServlet(classOf[HelloWorldServlet], "/*")

  def getRoot200 = get("/") { 
    status must_== 200 
  }
}

class HelloWorldMutableServletSpec extends MutableScalatraSpec {
  addServlet(classOf[HelloWorldServlet], "/*") 

  "GET / on HelloWorldServlet" should {
    "return status 200" in {
      get("/") { 
        status must_== 200
      }
    }
  }
}
{pygmentize}

Other test frameworks
---------------------

### Dependencies
- scalatra-test

### Usage guide
Create an instance of org.scalatra.test.ScalatraTests.  Be sure to call `start()` and `stop()` before and after your test suite.

## Maven Repository

To make usage of Scalatra as a dependency convenient, Maven hosting is now available courtesy of [Sonatype](https://docs.sonatype.com/display/NX/OSS+Repository+Hosting).

* [Releases](https://oss.sonatype.org/content/repositories/releases)
* [Snapshots](https://oss.sonatype.org/content/repositories/snapshots)
