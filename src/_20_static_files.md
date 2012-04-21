Static Files
============

Static files can be served out of the `webapp` folder, which acts as the ROOT
directory. As with any servlet based application, the contents of this directory 
are all public, with the exception of files in the WEB-INF directory. 

An example structure helps.

project

    src
    |_ main
       |_ scala
       |  |_ Web.scala
       |_ webapp
          |_ WEB-INF
          |  |_ secret.txt
          |  |_ views
          |  |  |_ default.jade
          |  |
          |  |_ layouts
          |  |  |_ default.jade
          |  |
          |  |_ web.xml
          |- stylesheets
          |    |_ default.css
          |- images
               |_ foo.jpg


In this application, the only publicly accessible files will be at
`stylesheets/default.css` and `images/foo.jpg`. Everything else will be 
protected by the web application container. 