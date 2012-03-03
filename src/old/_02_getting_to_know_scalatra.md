The following text (indented) is I think fairly well covered in the "routes" section, 
and doesn't need to be repeated:

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


"Templates" are already covered in the "views" section:

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

