Views
=====

Views can either be constructed within your Scala classes with HTML or
using the ScalateSupport helper.

Inline HTML
-----------

.. code-block:: scala

  def get("/") { contentType="text/html"

    <html>
      <head>Test</head>
      <body>
      Test Body for {uri("/")}
      </body>
    </html>

  }

ScalateSupport
--------------

Including a basic template to be returned to the browser.

Basic usage 
.. code-block:: scala
   
   def get("/") { 
     contentType="text/html"

     layoutTemplate("/WEB-INF/views/index.ssp") 
   }

Choosing a different template 
.. code-block:: scala
   def get("/") {
     contentType="text/html"

     layoutTemplate("/WEB-INF/views/index.ssp",("layout" ->"/WEB-INF/layouts/app.ssp")) 
   }

Passing parameters 
.. code-block:: scala
   
   def get("/") {
     contentType="text/html"

     layoutTemplate("/WEB-INF/views/index.ssp",Map("foo" ->"uno","bar"->"dos")) 
   }

Putting it all together 
.. code-block:: scala
   
   def get("/") {
     contentType="text/html"
   
     layoutTemplate("/WEB-INF/views/index.ssp",("layout" ->"/WEB-INF/layouts/app.ssp"),Map("foo" -> "uno","bar"->"dos")) 
   }

