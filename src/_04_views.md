
Views
=====

Views can either be constructed within your Scala classes with HTML or using the ScalateSupport helper.

Inline HTML
-----------

{pygmentize:: scala}
  def get("/") {
    contentType="text/html"

    <html>
    <head><title>Test</title></head>
    <body>Test Body for {uri("/")}</body>
    </html>
  }
{pygmentize}

ScalateSupport
--------------

Including a basic template to be returned to the browser.
{pygmentize:: scala}
  def get("/") {
    contentType="text/html"

    templateEngine.layout("/WEB-INF/views/index.ssp")
  }
{pygmentize}