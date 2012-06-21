Introduction
============

Scalatra is one of a new breed of simple, accessible, scalable web frameworks. It combines the power of the JVM with the beauty and brevity of [Scala](http://scala-lang.org), allowing you to build high-performance web APIs which may be asynchronous or realtime. 

Getting started is easy!

You need to install a few things before you can get started, but before we get to that, here's a very simple Scalatra app:

{pygmentize:: scala}
package com.example.app
import org.scalatra._

class MyScalatraFilter extends ScalatraFilter {
  get("/") { 
    Hello world! 
  }
}
{pygmentize}

Notice a few things about it:

* it's small
* it uses a [Sinatra](http://sinatrarb.com/)-style DSL
* it defines a single method, an HTTP GET to the path "/"
* it responds with some text

This is the essence of Scalatra - small, easy to understand systems which embrace HTTP's stateless nature. You can build anything you want with Scalatra, but one of the things it's been most successful at is the construction of RESTful APIs. 

Why would you want to use Scalatra?

* it's a perfect server-side counterpart to in-browser client development frameworks such as [backbone.js](http://backbonejs.org/) or [angular.js](http://angularjs.org)
* it helps you to quickly build high-performance, scalable HTTP APIs
* it's been proven in production - LinkedIn, the Guardian newspaper, games website IGN, and the UK government all rely on it
* it will happily use all available cores of that new 16-core server with no effort on your part
* it's a simple, fun, and practical way to learn Scala