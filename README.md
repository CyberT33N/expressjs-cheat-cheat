# Express.js Cheat Sheet
Express.js Cheat Sheet with the most needed stuff..





<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# Routes

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

# POST


## Parse application/json
```javascript
const bodyParser = require('body-parser');
app.use(bodyParser.json());
```


## Recieve JSON
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



<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# Rate Limit

## Third party modules
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



# Postman




## How to send JSON data to an API endpoint
- https://www.youtube.com/watch?v=Jq5XIEsaVyE
