---
layout: default
title: Scalatra | Your First Scalatra App
---

Hello World Application
=======================

Scalatra is installed, how about making your first application?  Source files
go into `src/main/scala/com/example/app` (substitute your package for
`com/example/app`).  Open
`src/main/scala/com/example/app/MyScalatraServlet.scala`, or whatever you named
your servlet when you generated your project with g8:

{pygmentize:: scala}
package com.example.app

import org.scalatra._
import scalate.ScalateSupport

class MyScalatraServlet extends ScalatraServlet with ScalateSupport {

  get("/") {
    <html>
      <body>
        <h1>Hello, world!</h1>
        Say <a href="hello-scalate">hello to Scalate</a>.
      </body>
    </html>
  }
}
{pygmentize}

If you haven't already done so, from your project root, you can run the project
with:

{pygmentize::}
$ sbt
> container:start
{pygmentize}

The application starts on [http://localhost:8080](http://localhost:8080).  As
you can see, Scalatra doesn't force you to setup much infrastructure: a
request to a URL evaluates some Scala code and returns some text in response.
Whatever the block returns is sent back to the browser.
