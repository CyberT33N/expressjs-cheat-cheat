# Express.js Cheat Sheet
Express.js Cheat Sheet with the most needed stuff..





<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# Website

## Homepage:
```javascript
app.get('/', function (req, res) {
  res.send('Hello World!')
});
```

## Load html file
```javascript
res.render('./website/index.html');
```

## Load full project on homepage
```javascript
app.use(express.static(__dirname + '/website'));
```




<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />


## Parse application/json
```javascript
const bodyParser = require('body-parser');
app.use(bodyParser.json());
```



## Verfiy Content Type
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


## Get specific Header
```javascript
// you can create any header you want and then get the data from it. Very usefully for tokens as example or other data
req.headers['authorization']
```


## Response Timeout (Socket hang up error)
- By default, normal HTTP requests to Node.js/Express/Sails.js apps time out after 2 minutes (120000 milliseconds) if a response is not sent.
- In some cases like as example on Google Cloud you get Socket hang up errors when you use port 80 for your express server. Make sure to use a different port like as example 1337 (https://www.youtube.com/watch?v=JmjqPpQdtW8)
```javascript
//method #1 (recommended)
app.post('/square', function(req, res){  req.setTimeout(400000);  });

// method #2 (seems not work to prevent socket hang up error)
var timeout = require('connect-timeout');
app.post('/square', apiLimiter, timeout('20s'), function(req, res){  });
```

<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

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


<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

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





<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

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



<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# OAuth
https://auth0.com/blog/node-js-and-express-tutorial-building-and-securing-restful-apis/

<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# Simulate all kind of requests

## Postman
- https://www.youtube.com/watch?v=Jq5XIEsaVyE
