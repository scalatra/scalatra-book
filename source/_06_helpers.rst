Helpers
=======

Scalate
-------

`Scalate <http://scalate.fusesource.org/>`_ is a template engine for
generating text and markup. It is the default rendering engine included
as a helper trait.

Once you have the ``ScalateSupport`` trait applied you can call
``templateEngine.layout('index.page')`` within your action handlers.

.. code-block:: scala
   
   class MyScalatraFilter extends ScalatraFilter with ScalateSupport { 
     notFound { // If no route matches, then try to render a Scaml template 
     val templateBase = requestPath match { 
       case s if s.endsWith("/") => s + "index" 
       case s => s 
     } 
     val templatePath = "/WEB-INF/scalate/templates/" + templateBase + ".scaml"

     servletContext.getResource(templatePath) match { 
       case url: URL => { 
         contentType = "text/html" 
         templateEngine.layout(templatePath) case _ => filterChain.doFilter(request, response) 
       } 
     } 
   }

Scalate error page
~~~~~~~~~~~~~~~~~~

Mixing in ScalateSupport enables the Scalate error page for any uncaught
exceptions. This page renders the template source with the error
highlighted. To disable this behavior, override
``isScalateErrorPageEnabled``:

.. code-block:: scala
   
   override def isScalatePageEnabled = false


Scentry + Authentication
------------------------

Scentry is a user submitted authentication scheme. Combined
``ScentrySupport`` and ``BasicAuthSupport`` traits allow you to quickly
tie a User class to the session and Basic Authentication methods.

There is a new authentication middleware in the auth directory, to be
documented soon. See an example at `usage
example <http://gist.github.com/660701>`_. Another
`example <https://gist.github.com/732347>`_ for basic authentication can
be found

To use it from an SBT project, add the following to your project:

.. code-block:: scala
   
   val auth = "org.scalatra" %% "scalatra-auth" % scalatraVersion

User Password
~~~~~~~~~~~~~

The User Password is the most common form/ajax/mixed strategy used in
standard application development.

TODO: Add example

Remember Me
~~~~~~~~~~~

Remember Me is a common strategy that can be applied to your
application. It allows your users to return to your site already logged
into their account by validating a secure token.

TODO: Add example

Basic Authentication
~~~~~~~~~~~~~~~~~~~~

Basic Authentication is the simplest authentication method. Due to the
constraints of Basic Authentication it is recommended that people
utilize the User Password strategy if they need to have better control
over user interaction.

TODO: Add example

Flash Map
---------

Flash support is included within Scalatra by default. Flash entries are
not normally available within the current request scope. The exception
is adding new flash entries into ``flash.now``.

TODO: Add better supporting examples

.. code-block:: scala
   
   flash += ("error" -> "An error occurred") 
   flash.now += ("info" -> "redirect to see the error")

File Upload
-----------

Scalatra provides optional support for file uploads with Apache Commons
`FileUpload <http://commons.apache.org/fileupload/>`_.

1. Depend on scalatra-fileupload.jar. In your SBT build:

.. code-block:: scala
   
   val scalatraFileUpload = "org.scalatra" %% "scalatra-fileupload" % scalatraVersion

2. Extend your application with ``FileUploadSupport``:

.. code-block:: scala
   
   import org.scalatra.ScalatraServlet 
   import org.scalatra.fileupload.FileUploadSupport

   class MyApp extends ScalatraServlet with FileUploadSupport { 
     // ... 
   }


3. Be sure that your form is of type ``multipart/form-data``:

.. code-block:: scala
   
   get("/") {
   
     <form method="post" enctype="multipart/form-data"></form>

   }

4. Your files are available through the ``fileParams`` or
   ``fileMultiParams`` maps:

.. code-block:: scala
   
   post("/") { processFile(fileParams("file")) }


Anti-XML integration
--------------------

Scalatra provides optional `Anti-XML <http://anti-xml.org/>`_
integration:

1. Depend on scalatra-anti-xml.jar. In your SBT build:

.. code-block:: scala
   
   val scalatraAntiXml = "org.scalatra" %% "scalatra-anti-xml" % scalatraVersion

2. Extend your application with ``AntiXmlSupport``

.. code-block:: scala
   
   import org.scalatra.ScalatraServlet 
   import org.scalatra.antixml.AntiXmlSupport 
   import com.codecommit.antixml._

   class MyApp extends ScalatraServlet with AntiXmlSupport { 
     // ... 
   }


3. Actions results of type ``com.codecommit.antixml.Elem`` will be
   serialized to the response body, and a content type of ``text/html``
   will be inferred if none is set.

.. code-block:: scala
   
   get("/") { XML.fromString("") }

URL Support and Reverse Routes
------------------------------

UrlSupport provides two instances that provide you with relative url's.
UrlSupport.url will return a string that can be used in your output or a
redirect statement.

1. Page relative url 
.. code-block:: scala
   
   get("/"){ 
     // This will redirect to http:///page-relative 
     redirect(url("page-relative")) 
   }
   

2. Context relative url 
.. code-block:: scala
   get("/"){ 
     // This will redirect to http:////context-relative
     redirect(url("/context-relative")) 
   } 

3. Mapped params 
.. code-block:: scala
   get("/") { // This will redirect to http:////en-to-es?one=uno&two=dos 
     redirect( url("/en-to-es",
     Map("one" -> "uno", "two" -> "dos")) ) 
   }

TODO: add reverse routing

WebSocket and Comet support through Socket.IO
---------------------------------------------

**WebSocket support is Deprecated**

Scalatra provides optional support for websockets and comet through
`socket.io <http://socket.io>`_. We depend on `the socketio-java
project <http://code.google.com/p/socketio-java>`_ to provide this
support.

1) Depend on the scalatra-socketio.jar. In your SBT build: 

.. code-block:: scala
   
   val scalatraSocketIO = "org.scalatra" %% "scalatra-socketio" % scalatraVersion

2) SocketIO mimics a socket connection so it's easiest if you just
   create a socketio servlet at /socket.io/\*

.. code-block:: scala
   
   import org.scalatra.ScalatraServlet 
   import org.scalatra.socketio.SocketIOSupport

   class MySocketIOServlet extends ScalatraServlet with SocketIOSupport {
     // ... 
   } 

3) Setup the callbacks

.. code-block:: scala
   
   socketio { 
     socket => socket.onConnect { connection => // Do stuff on connection }
     socket.onMessage { (connection, frameType, message) => 
       // Receive a message 
       // use ``connection.send("string")`` to send a message 
       // use ``connection.broadcast("to send")`` to send a message to all connected clients except the current one 
       // use ``connection.disconnect`` to disconnect the client. 
     }
     socket.onDisconnect { (connection, reason, message) => // Do stuff on disconnection } 
   }

4) Add the necessary entries to web.xml

.. code-block:: scala
   
   SocketIOServlet com.example.SocketIOServlet
   flashPolicyServerHost localhost flashPolicyServerPort 843
   flashPolicyDomain localhost flashPolicyPorts 8080


When you want to use websockets with jetty the sbt build tool gets in
the way and that makes it look like the websocket stuff isn't working.
If you deploy the war to a jetty distribution everything should work as
expected.
