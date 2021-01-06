## Features

 Build the app image 
 >docker build -f Dockerfile-MeanApp.txt -t mean-app .

  Start the mongodb app
 >docker run --name mong-server -d mongo

 Start the node app
  docker run --link mong-server:db_1 -p 80:3000 -d mean-app 
------------------------------------
  App parts ad dependencies
+ Angular/Node/Express/MongoDB.
+    
+ Passport - login, logout, signup.
+ CRUD operations with Express JS and MongoDB.
+ No Jade Templating, instead implemented html partilals via Angular template.
+ Bower configuration to handle JS browser dependencies.
+ RequireJS.
+ Unit test base configuration with Jasmine and Karma.
+ Twitter Bootstrap
+ JQuery 

 
## Directory Layout
    
    server.js             --> app config
    package.json          --> for npm
    nginx.conf            --> nginx reverse proxy configuration
    config/               --> contains mongoDB and passport configuration
        database.js
        passport.js
    models/               --> contains mongoDB simple user Schema
        users.js
        person.js
        thing.js
        models.js
    public/               --> all of the files to be used in on the client side
      css/                --> css files
        app.css           --> default stylesheet
      js/                 --> javascript files
        app.js            --> declare top-level app module
        controllers.js    --> application controllers
        directives.js     --> custom angular directives
        filters.js        --> custom angular filters
        services.js       --> custom angular services
        bower_components/ --> angular and 3rd party JavaScript libraries
          angular/
          angular-local-storage/
          angular-route/
          bootstrap/
          cryptojslib/
          jquery/
          noty/
    app/
      api.js              --> api definitions
      routes.js           --> route for serving HTML pages, JSON and partials
    views/
      index.html          --> main page for app
      partials/           --> angular view partials (partial jade templates)
        header.html
        nav.html
        register.html
        login.html
      auth/
        home.html
        person.html
        thing.html

