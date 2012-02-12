Testing
=======

Scalatra includes a test framework for writing the unit tests for your
Scalatra application. The framework lets you send requests to your app
and examine the response. All HTTP verbs are supported, as well as
request parameters and session tracking.

Integrations
------------

scalatra-test can be used with the test framework of your choosing. A
basic example from each supported framework is shown below. You may mix
and match if transitioning from one framework to another; sbt will find
and run them all by default.

`ScalaTest <http://scalatest.org/>`_
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Dependency
^^^^^^^^^^

::

    "org.scalatra" %% "scalatra-scalatest" % scalatraVersion % "test"

Example
^^^^^^^

Extend ScalatraSuite with your preferred Suite implementation. You get
ShouldMatchers and MustMatchers for free.

.. code-block:: scala

   class MyScalatraServletTests extends ScalatraSuite with FunSuite { 
     // ``MyScalatraServlet`` is your app which extends ScalatraServlet 
     addServlet(classOf[MyScalatraServlet], "*")

     test("simple get") { 
       get("/path/to/something") { 
         status should equal (200) body should include ("hi!") 
       } 
     } 
   } 

Convenience traits are provided for many Suite implementations:

-  ScalatraSpec
-  ScalatraFlatSpec
-  ScalatraFreeSpec
-  ScalatraWordSpec
-  ScalatraFunSuite
-  ScalatraFeatureSpec
-  ScalatraJUnit3Suite
-  ScalatraJUnitSuite (JUnit 4)
-  ScalatraTestNGSuite

`Specs2 <http://etorreborre.github.com/specs2/>`_
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Dependency
^^^^^^^^^^

::

    "org.scalatra" %% "scalatra-specs2" % scalatraVersion % "test"

Example
^^^^^^^

Specs2 supports two basic styles: Unit and Acceptance. Both are
supported by scalatra-test.

Unit
''''

From the `Specs2
QuickStart <http://etorreborre.github.com/specs2/guide/org.specs2.guide.QuickStart.html>`_:

    unit specifications where the specification text is interleaved with
    the specification code. It is generally used to specify a single
    class.

.. code-block:: scala

   import org.scalatra.test.specs2
   
   class HelloWorldMutableServletSpec extends MutableScalatraSpec {
     addServlet(classOf[HelloWorldServlet], "/\*")

     "GET / on HelloWorldServlet" should { "return status 200" in { get("/") { status must\_== 200 } } } 
   }

Acceptance
''''''''''

From the `Specs2
QuickStart <http://etorreborre.github.com/specs2/guide/org.specs2.guide.QuickStart.html>`_:

    acceptance specifications where all the specification text stands as
    one and the implementation code is elsewhere. It is generally used
    for acceptance or integration scenarios

.. code-block:: scala 

   class HelloWorldServletSpec extends ScalatraSpec {
     def is = "GET / on HelloWorldServlet" ^ "return status 200" ! getRoot200 end
   
     addServlet(classOf[HelloWorldServlet], "*")
   
     def getRoot200 = get("/") { status must_== 200 } 
   }

`Specs <http://code.google.com/p/specs/>`_
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Specs is now in maintenance mode. The author recommends that new
projects begin with Specs2.

Dependency
^^^^^^^^^^

::

    "org.scalatra" %% "scalatra-specs" % scalatraVersion % "test"

Example
^^^^^^^

.. code-block:: scala 

   object MyScalatraServletTests extends ScalatraSpecification { 
     addServlet(classOf[MyScalatraServlet], "/\*")

     "MyScalatraServlet when using GET" should { "/path/to/something should
   return 'hi!'" in { get("/") { status mustEqual(200) body
   mustEqual("hi!") } } } 
   }

Other test frameworks
~~~~~~~~~~~~~~~~~~~~~

Dependency
^^^^^^^^^^

::

    "org.scalatra" %% "scalatra-test" % scalatraVersion % "test"

Usage guide
^^^^^^^^^^^

Create an instance of ``org.scalatra.test.ScalatraTests``. Be sure to
call ``start()`` and ``stop()`` before and after your test suite.

FAQ
---

How do I set a servlet init parameter?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

scalatra-test is built on Jetty''s
`ServletTester <http://download.eclipse.org/jetty/stable-7/apidocs/org/eclipse/jetty/testing/ServletTester.html>`_,
so it does not read your web.xml. Most things you can do in a web.xml
can be done from the context on the tester object. In this case, call
this in the constructor of your servlet:

::

    tester.getContext.setInitParameter("db.username", "ross")

Maven Repository
----------------

To make usage of Scalatra as a dependency convenient, Maven hosting is
now available courtesy of
`Sonatype <https://docs.sonatype.com/display/NX/OSS+Repository+Hosting>`_.

-  `Releases <https://oss.sonatype.org/content/repositories/releases>`_
-  `Snapshots <https://oss.sonatype.org/content/repositories/snapshots>`_

