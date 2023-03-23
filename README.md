# Express.js Cheat Sheet
Express.js Cheat Sheet with the most needed stuff..


# Status Codes
https://developer.mozilla.org/de/docs/Web/HTTP/Status
```javascript
res.status(400).json({ error: 'message' })
res.status(200).send("SUCCESS")
```

## Simulate Status Codes
- https://httpstat.us/200































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Hello World
```javascript
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
})

// you can use aswell await
// await app.listen(port)
```































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Error Handling

<br><br>

## Handle Async
```javascript
/* ---- Method #1 - Pass the error with next() ---- */
app.get('/route', async(req, res, next) => {
    try {
        const result = await request('http://example.com');
        res.end(result);
    } catch(err) {
        next(err);
    }
});




/* ---- Method #2 - Custom handler ---- */
const asyncHandler = fn => (req, res, next) => {
    return Promise
        .resolve(fn(req, res, next))
        .catch(next);
};

app.use(asyncHandler(async(req, res, next) => {
    await authenticate(req);
    next();
}));

app.get('/async', asyncHandler(async(req, res) => {
    const result = await request('http://example.com');
    res.end(result);
}));

// Any rejection will go to the error handler








/* ---- Method #3 - Plugin ---- */
// https://github.com/davidbanham/express-async-errors/

const express = require('express');
require('express-async-errors');
const User = require('./models/user');
const app = express();

app.get('/users', async (req, res) => {
  const users = await User.findAll();
  res.send(users);
});
```































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# File Structur
- https://www.geeksforgeeks.org/why-express-app-and-server-files-kept-separately/





































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Stop
```javascript
var server = app.listen(3000);
await server.close();
```




























<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Website

<br><br>

## Homepage:
```javascript
app.get('/', function (req, res) {
  res.send('Hello World!')
});
```

<br><br>

## Load html file
```javascript
// method #1
// viewed at http://localhost:8080
app.get('/', function(req, res) {
    res.render(__dirname + '/website/index.html');
});

// method #2
app.get('/', function(req, res) {
    res.sendFile(__dirname + '/website/index.html');
});

```

<br><br>

## Load full project on homepage
```javascript
// method #1
app.use(express.static(__dirname + '/website'));

// method #2 go folder back
const path = require('path')
const folder = path.join(__dirname, '..', 'resources')
app.use(express.static(folder))
```




























































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Middleware
```javascript
const express = require('express')
const app = express()
const port = 30558

// Middleware #1
app.use((req, res, next)=>{
    // do something..
    next()
})

// Middleware #2
app.use((req, res, next)=>{
    // do something..
    next()
})

app.listen(port, () => {
    console.log(`Example app listening at http://localhost:${port}`)
})
```





































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



# Change view engine (ejs, jade, ..)
```javascript
// global
app.set('view engine', 'ejs');
```
<br><br>


# Parse application/json
```javascript
const bodyParser = require('body-parser');
app.use(bodyParser.json());
```

<br><br>

# Verfiy Content Type
```javascript
app.use('/api/', (req, res, next) => {
    if (!req.is('application/json')) {
        // Send error here
        res.send(400);
    } else {
        // Do logic here
    }
});
```

<br><br>

# Get Header details
```javascript
// get specific header
req.headers['authorization'];

// get all header details
console.log( JSON.stringify(req.headers, null, 4) ):
```

<br><br>

# Response Timeout (Socket hang up error)
- By default, normal HTTP requests to Node.js/Express/Sails.js apps time out after 2 minutes (120000 milliseconds) if a response is not sent.
- In some cases like as example on Google Cloud you get Socket hang up errors when you use port 80 for your express server. Make sure to use a different port like as example 1337 (https://www.youtube.com/watch?v=JmjqPpQdtW8)
```javascript
//method #1 (recommended)
app.post('/square', function(req, res){  req.setTimeout(400000);  });

// method #2 (seems not work to prevent socket hang up error)
var timeout = require('connect-timeout');
app.post('/square', apiLimiter, timeout('20s'), function(req, res){  });
```

































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# express.Router (https://expressjs.com/de/guide/routing.html)
- Use the express.Router class to create modular, mountable route handlers. A Router instance is a complete middleware and routing system; for this reason, it is often referred to as a “mini-app”.

<br><br>

The following example creates a router as a module, loads a middleware function in it, defines some routes, and mounts the router module on a path in the main app.

<br><br>

Create a router file named birds.js in the app directory, with the following content:
```javascript
var express = require('express')
var router = express.Router()

// middleware that is specific to this router
router.use(function timeLog (req, res, next) {
  console.log('Time: ', Date.now())
  next()
})

// define the home page route
router.get('/', function (req, res) {
  res.send('Birds home page')
})

// define the about route
router.get('/about', function (req, res) {
  res.send('About birds')
})

module.exports = router
```




<br><br>

## Chain callback
```javascript
// routes.js
var express = require('express')
var router = express.Router()

const {createImage, saveImage} = require('./services.js')

router.post('/about', createImage, saveImage)




// services.js
const createImage = async () => {
 // ..
 req.transferData = 123
 next()
}

const saveImage = async () => {
 // ..
 const useData = req.transferData
}

module.exports = {
    createImage,
    saveImage
}
```








































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# app


<br><br>

## .all()
- This method is like the standard app.METHOD() methods, except it matches all HTTP verbs.
- The following callback is executed for requests to /secret whether using GET, POST, PUT, DELETE, or any other HTTP request method:
```javascript
app.all('/secret', function (req, res, next) {
  console.log('Accessing the secret section ...')
  next() // pass control to the next handler
})
```

<br><br>

## .delete()
- Routes HTTP DELETE requests to the specified path with the specified callback functions. For more information, see the routing guide.
- The following callback is executed for requests to /secret whether using GET, POST, PUT, DELETE, or any other HTTP request method:
```javascript
app.delete('/', function (req, res) {
  res.send('DELETE request to homepage')
})
```

<br><br>

## .disable()
- Sets the Boolean setting name to false, where name is one of the properties from the app settings table. Calling app.set('foo', false) for a Boolean property is the same as calling app.disable('foo').
```javascript
app.disable('trust proxy')
app.get('trust proxy')
// => false
```

<br><br>

## .disabled()
- Returns true if the Boolean setting name is disabled (false), where name is one of the properties from the app settings table.
```javascript
app.disabled('trust proxy')
// => true

app.enable('trust proxy')
app.disabled('trust proxy')
// => false
```



<br><br>

## .enable()
- Returns true if the Boolean setting name is disabled (false), where name is one of the properties from the app settings table.
```javascript
app.enable('trust proxy')
app.get('trust proxy')
// => true
```


<br><br>

## .enabled()
- Returns true if the Boolean setting name is disabled (false), where name is one of the properties from the app settings table.
```javascript
app.enabled('trust proxy')
// => false

app.enable('trust proxy')
app.enabled('trust proxy')
// => true
```


<br><br>

## .engine()
- Registers the given template engine callback as ext. By default, Express will require() the engine based on the file extension. For example, if you try to render a “foo.pug” file, Express invokes the following internally, and caches the require() on subsequent calls to increase performance.
```javascript
app.engine('html', require('ejs').renderFile)
```



<br><br>

## .get(name)
- Returns the value of name app setting, where name is one of the strings in the app settings table. For example:
```javascript
app.get('title')
// => undefined

app.set('title', 'My Site')
app.get('title')
// => "My Site"
```



<br><br>

## .get(path, callback [, callback ...])
- Routes HTTP GET requests to the specified path with the specified callback functions.
```javascript
app.get('/', function (req, res) {
  res.send('GET request to homepage')
})
```



<br><br>

## .listen(path, [callback])
- Starts a UNIX socket and listens for connections on the given path. This method is identical to Node’s http.Server.listen().
```javascript
var express = require('express')
var app = express()
app.listen('/tmp/sock')
```




<br><br>

## .listen([port[, host[, backlog]]][, callback])
- Binds and listens for connections on the specified host and port. This method is identical to Node’s http.Server.listen(). If port is omitted or is 0, the operating system will assign an arbitrary unused port, which is useful for cases like automated tasks (tests, etc.).
```javascript
var express = require('express')
var app = express()
app.listen(3000)



// example #2
var express = require('express')
var https = require('https')
var http = require('http')
var app = express()

http.createServer(app).listen(80)
https.createServer(options, app).listen(443)
```


<br><br>

## .METHOD(path, callback [, callback ...])
- Routes an HTTP request, where METHOD is the HTTP method of the request, such as GET, PUT, POST, and so on, in lowercase. Thus, the actual methods are app.get(), app.post(), app.put(), and so on. See Routing methods below for the complete list.
- Express supports the following routing methods corresponding to the HTTP methods of the same names:
<br> checkout
<br> copy
<br> delete
<br> get
<br> head
<br> lock
<br> merge
<br> mkactivity
<br> mkcol
<br> move
<br> m-search
<br> notify
<br> options
<br> patch
<br> post
<br> purge
<br> put
<br> report
<br> search
<br> subscribe
<br> trace
<br> unlock
<br> unsubscribe








<br><br>

## .param(path, [callback])
- Add callback triggers to route parameters, where name is the name of the parameter or an array of them, and callback is the callback function. The parameters of the callback function are the request object, the response object, the next middleware, the value of the parameter and the name of the parameter, in that order.
```javascript
app.param('user', function (req, res, next, id) {
  // try to get the user details from the User model and attach it to the request object
  User.find(id, function (err, user) {
    if (err) {
      next(err)
    } else if (user) {
      req.user = user
      next()
    } else {
      next(new Error('failed to load user'))
    }
  })
})
```





<br><br>

## Access every request
```javascript
app.use(function (req, res, next) {

    if( path.extname(path.basename(req.url)) ) log("The file " + path.basename(req?.url) + " was requested.");
    else log("The endpoint " + path.basename(req?.url) + " was requested.");

    next();

});
```










































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# POST

## Get params from POST request
```javascript
app.post('/pizza', apiLimiter, function(req, res){(async () => {
log( 'PIZZA - POST REQUEST INCOMING.. Query: ' + JSON.stringify(req?.query, null, 4) );

    if ( !req?.query?.id || !req?.query?.title ) {
        const e = 'As it seems the POST request doesnt contain a valid ID & Title.. We cancel the request now..';
        log(e);
        res.send(e);
        return;
     }

})().catch((e) => {  log('ASYNC - POST - Error at pizza Route.. Error: ' + e)  })});
```

## Recieve JSON from POST request
```javascript
// Make sure to set the content type header at your POST request ( headers: {"Content-Type": "application/json"} )
app.post('/square', function(req, res){
  log( 'request.body: '  + req.body );      // your JSON
  res.send(req.body);    // echo the result back
});

// The example above will work for the route square. If you want to recieve data in general without route you can use
app.use(function (req, res, next) {
  log( 'request.body: '  + req.body );      // your JSON
  res.send(req.body);    // echo the result back
});
```








































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# DELETE

## Get params from DELETE request
```javascript
app.delete('/pizza', apiLimiter, function(req, res){(async () => {
log( 'PIZZA - DELETE REQUEST INCOMING.. Query: ' + JSON.stringify(req?.query, null, 4) );

      if ( !req?.query?.id ) {
            const e = 'As it seems the DELETE request doesnt contain a valid ID in the url.. We cancel the request now..';
            log(e);
            res.send(e);
            return;
       }

})().catch((e) => {  log('ASYNC - DELETE - Error at pizza Route.. Error: ' + e)  })});
```


































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# PUT

## Get params from PUT request
```javascript
app.put('/pizza', apiLimiter, function(req, res){(async () => {
log( 'PIZZA - PUT REQUEST INCOMING.. Query: ' + JSON.stringify(req?.query, null, 4) );

      if ( !req?.query?.id || !req?.query?.title ) {
            const e = 'As it seems the PUT request doesnt contain a valid ID & Title in the url.. We cancel the request now..';
            log(e);
            res.send(e);
            return;
       }

})().catch((e) => {  log('ASYNC - PUT - Error at pizza Route.. Error: ' + e)  })});
```












































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Rate Limit

## Middleware
https://www.npmjs.com/package/express-rate-limit


```javascript
// Enable if you're behind a reverse proxy (Heroku, Bluemix, AWS ELB, Nginx, etc)
// see https://expressjs.com/en/guide/behind-proxies.html
// app.set('trust proxy', 1);

const rateLimit = require('express-rate-limit');
const limit = 60000;

const apiLimiter = rateLimit({
       windowMs: limit, // How long the ip will be blocked until it´s avaible again
       message: "Too many POST requests created from this IP, please try again in " + limit + "ms",
       max: 1 // how many requests are allowed until ip get blocked. When the limit timer is over it will start again.
});


app.post('/square', apiLimiter, function(req, res){  });
```

































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# Route

## Paramaters (http://expressjs.com/en/guide/routing.html#route-parameters)
- Route parameters are named URL segments that are used to capture the values specified at their position in the URL. The captured values are populated in the req.params object, with the name of the route parameter specified in the path as their respective keys.
```javascript
/*
Route path: /users/:userId/books/:bookId
Request URL: http://localhost:3000/users/34/books/8989
req.params: { "userId": "34", "bookId": "8989" }
*/

app.get('/users/:userId/books/:bookId', function (req, res) {
  res.send(req.params)
})
```

































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



# Redirect

```javascript
// Methode #1
const proxy = require('express-http-proxy')

const host = `${process.env.HOST}:${process.env.PORT0}`
const options = {
    proxyReqPathResolver: function (req) {
      const redirectUrl = req.originalUrl.replace('/cs/apple', '/cs/banana')
      return redirectUrl
    }
}
app.use('/v1/cs/apple', proxy(host, options))




// Method #2 - Only works for GET
masterRouter.use('/v1/cs/apple', (req, res, next) => {
    const redirectUrl = req.originalUrl.replace('/cs/apple', '/cs/banana')
    res.redirect(302, redirectUrl)
})
```
























































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



# OAuth

## Third Party
https://auth0.com/blog/node-js-and-express-tutorial-building-and-securing-restful-apis/

## Internal
http://thecodebarbarian.com/oauth-with-node-js-and-express.html











































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# Testing

<br><br>

## Supertest (https://www.npmjs.com/package/supertest)
```javascript
'use strict'

const axios = require('axios')
const http = require('http')
const express = require('express')
const router = express.Router()
const app = express()
const port = 6969

app.post('/test', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, async () => {
  console.log(`Example app listening at http://localhost:${port}`)
  test()
})

const test = async () => {
    const server = http.createServer(app)
    const requestServer = require('supertest')(server)

    const url = '/test'
    const res = await requestServer.post(url)
    console.log(res)
}
```

































































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Make Requests faster


## compression
```
const compression = require('compression')
app.use(compression({ level: 9 }))
```


















































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Third Party Software

## Postman
- Simulate all kind of requests
- https://www.youtube.com/watch?v=Jq5XIEsaVyE

<br><br>

## Hookbin (https://hookbin.com/)
- Check details about your requests

