Getting to know Scalatra
=======================

## It's Witchcraft

You saw in the introduction how to install Scalatra, its dependencies, and
write a small "hello world" application. In this chapter you will get a
whirlwind tour of the framework and familiarize yourself with its features.

## Routing

Scalatra is super flexible when it comes to routing, which is essentially an
HTTP method and a regular expression to match the requested URL. The four basic
HTTP request methods will get you a long ways:

*   GET
*   POST
*   PUT
*   DELETE

Routes are the backbone of your application, they're like a guide-map to how
users will navigate the actions you define for your application.

They also enable to you create [RESTful web services][restful-web-services], in
a very obvious manner. Here's an example of how one such service might look:

{pygmentize:: scala}
get("/dogs") {
  # get a listing of all the dogs
}

get("/dog/:id") {
  # just get one dog, you might find him like this:
  val dog = Dog.find(params("id"))
  # using the params convention, you specified in your route
}

post("/dog") {
  # create a new dog listing
}

put("/dog/:id") {
  # HTTP PUT request method to update an existing dog
}

delete("/dog/:id") {
  # HTTP DELETE request method to remove a dog who's been sold!
}
{pygmentize}

As you can see from this contrived example, Scalatra's routing is very easy 
to get along with. Don't be fooled, though, Scalatra can do some pretty 
amazing things with Routes.

Take a more indepth look at [Scalatra's routes][routes], and see for yourself. 

[routes]: http://www.scalatra.org/stable/book/#Routes
[restful-web-services]: http://en.wikipedia.org/wiki/Representational_State_Transfer#RESTful_web_services

## Filters

Scalatra offers a way for you too hook into the request chain of your
application via [Filters][filters].

Filters define two methods available, `before` and `after` which both accept a
block to yield corresponding the request and optionally take a URL pattern to
match to the request.

### before

The `before` method will let you pass a block to be evaluated **before** _each_
and _every_ route gets processed.

{pygmentize:: scala}
before() {
  MyDb.connect
}

get("/") {
  val list = MyDb.findAll()
  templateEngine.layout("index.ssp", list)
}
{pygmentize}

In this example, we've set up a `before` filter to connect using a contrived
`MyDb` module.

### after

The `after` method lets you pass a block to be evaluated **after** _each_ and
_every_ route gets processed.

{pygmentize:: scala}
after() {
  MyDb.disconnect
}
{pygmentize}

As you can see from this example, we're asking the `MyDB` module to
disconnect after the request has been processed.

### Pattern Matching

Filters optionally take a pattern to be matched against the requested URI
during processing. Here's a quick example you could use to run a contrived
`authenticate!` method before accessing any "admin" type requests.

{pygmentize:: scala}
before("/admin/*") {
  basicAuth
}

after("/admin/*") {
  user.logout
}
{pygmentize}

[filters]: http://www.scalatra.org/stable/book#Filters

## Handlers

Handlers are top-level methods available in Scalatra to take care of common HTTP
routines. For instance there are handlers for [halting][halting] and
[passing][passing].

There are also handlers for redirection:

{pygmentize:: scala}
get("/"){
  redirect("/someplace/else")
}
{pygmentize}

This will return a 302 HTTP Response to `/someplace/else`.

_Caution:_ `redirect` is implemented as a HaltException.  You probably don't
want to catch it in an action.

Handlers can be extremely useful when used properly, probably the most common
use is the `params` convention, which gives you access to any parameters passed
in via the request object, or generated in your route pattern.

[halting]: http://www.scalatra.org/stable/book/#Halting
[passing]: http://www.scalatra.org/stable/book/#Passing

## Halting

To immediately stop a request within a filter or route:

{pygmentize:: scala}
halt()
{pygmentize}

You can also specify the status:

{pygmentize:: scala}
halt(403)
{pygmentize}

Or the status and the body:

{pygmentize:: scala}
halt(403, <h1>Go away!</h1>)
{pygmentize}

Or even the HTTP status reason and headers.  For more complex invocations, it 
is recommended to use named arguments:

{pygmentize:: scala}
halt(status = 403,
     reason = "Forbidden",
     headers = Map("X-Your-Mother-Was-A" -> "hamster",
                   "X-And-Your-Father-Smelt-Of" -> "Elderberries"),
     body = <h1>Go away or I shall taunt you a second time!</h1>)
{pygmentize}

The `reason` argument is ignored unless `status` is not null.  The default 
arguments leave that part of the request unchanged.

_Caution:_ `halt` is implemented as a HaltException.  You probably don't want
to catch it in an action.

## Passing

A route can punt processing to the next matching route using `pass()`.  
Remember, unlike Sinatra, routes are matched from the bottom up.

{pygmentize:: scala}
get("/guess/*") {
  "You missed!"
}

get("/guess/:who") {
  params("who") match {
    case "Frank" => "You got me!"
    case _ => pass()
  }
}
{pygmentize}

The route block is immediately exited and control continues with the next 
matching route.  If no matching route is found, a 404 is returned.

_Caution:_ `halt` is implemented as a HaltException.  You probably don't want
to catch it in an action.

## Templates

Scalatra has very powerful templating support. You can either inline your views:

{pygmentize:: scala}
def get("/") {
  contentType="text/html"

  <html>
  <head><title>Test</title></head>
  <body>Test Body for {uri("/")}</body>
  </html>
}
{pygmentize}

Or you can use the [Scalate][scalate] templating engine:

{pygmentize:: scala}
def get("/") {
  contentType="text/html"

  templateEngine.layout("index.ssp")
}
{pygmentize}

For more info, see the [Views][views] section.

## Helpers

Helpers exist as traits in Scalatra that can applied to your base class. 
Please see [Helpers][helpers] for more details.

[scalate]: http://scalate.fusesource.org
[views]: http://www.scalatra.org/stable/book/#Views
[helpers]: http://www.scalatra.org/stable/book/#Helpers%20in%20Scalatra

## Accessing the Servlet API

### HttpServletRequest

The request is available through the `request` variable.  The request is 
implicitly extended with the following methods:

1. `body`: to get the request body as a string
2. `isAjax`: to detect AJAX requests
3. `cookies` and `multiCookies`: a Map view of the request's cookies
4. Implements `scala.collection.mutable.Map` backed by request attributes

### HttpServletResponse

The response is available through the `response` variable. If you override 
the Scalatra handling and write directly to the response object 
(Ex: response.getOutputStream), then your action should return Unit() to 
prevent a conflict with multiple writes.

### HttpSession

Scalatra has session handling built into the framework by default. There are 
no modules or traits that you need to include. 

However, sessions are *off* until the `session` method is called. 
Once `session` is called, a cookie-based session will be created. 

Then you will be able to use the default cookie based session handler in your
application:

{pygmentize:: scala}
get("/") {
  if(session.contains("counter")) session("counter") = 0
  session("counter") = session("counter").toInt + 1
  "You've hit this page %s times!" format session("counter").toInt
}
{pygmentize}

The `session` implicitly implements `scala.collection.mutable.Map` backed by 
`session` attributes.  

There's also a `sessionOption` method, which returns a `session` if one already 
exists, and `None` if it doesn't. This is a good way to avoid creating a 
`session`.

If you don't need a `session` in your application, e.g. if you're building out
a stateless RESTFul API, never call the `session` method, and don't mix in 
`FlashMapSupport` (which also creates a session). 

The default session in Scalatra is cookie-based, but the cookie is used only 
as a session identifier (session data is stored server-side). If you are 
building out a shared-nothing architecture, this is something to be aware of.

### ServletContext

The servlet context is available through the `servletContext` variable.  The 
servlet context implicitly implements `scala.collection.mutable.Map` backed 
by servlet context attributes.

## Configuration

The environment is defined by:

1. The `org.scalatra.environment` system property.
2. The `org.scalatra.environment` init property.
3. A default of `development`.

If the environment starts with "dev", then `isDevelopmentMode` returns true.  
This flag may be used by other modules, for example, to enable the Scalate 
console.

## Error handling

Error handlers run within the same context as routes and before filters.

### Not Found

Whenever no route matches, the `notFound` handler is invoked.  The default 
behavior is:

{pygmentize:: scala}
notFound {
  <h1>Not found. Bummer.</h1>
}
{pygmentize}

* _ScalatraServlet_: send a 404 response
* _ScalatraFilter_: pass the request to the servlet filter chain

