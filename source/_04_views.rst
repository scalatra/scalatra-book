Views
=====

Views can either be constructed within your Scala classes with HTML or
using the ScalateSupport helper.

Inline HTML
-----------

{pygmentize:: scala} def get("/") { contentType="text/html"

.. raw:: html

   <html>
     <head>

Test

.. raw:: html

   </head>
     <body>

Test Body for {uri("/")}

.. raw:: html

   </body>
     </html>

} {pygmentize}

ScalateSupport
--------------

Including a basic template to be returned to the browser.

Basic usage {pygmentize:: scala} def get("/") { contentType="text/html"

layoutTemplate("/WEB-INF/views/index.ssp") } {pygmentize}

Choosing a different template {pygmentize:: scala} def get("/") {
contentType="text/html"

layoutTemplate("/WEB-INF/views/index.ssp",("layout" ->
"/WEB-INF/layouts/app.ssp")) } {pygmentize}

Passing parameters {pygmentize:: scala} def get("/") {
contentType="text/html"

layoutTemplate("/WEB-INF/views/index.ssp",Map("foo" ->
"uno","bar"->"dos")) } {pygmentize}

Putting it all together {pygmentize:: scala} def get("/") {
contentType="text/html"

layoutTemplate("/WEB-INF/views/index.ssp",("layout" ->
"/WEB-INF/layouts/app.ssp"),Map("foo" -> "uno","bar"->"dos")) }
{pygmentize}
