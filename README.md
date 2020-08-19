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


## parse application/json
```javascript
const bodyParser = require('body-parser');
app.use(bodyParser.json());
```


## recieve JSON
```javascript
// Make sure to set the content type header at your POST request ( headers: {"Content-Type": "application/json"} )
app.post('/square', function(req, res){
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



# Postman




## How to send JSON data to an API endpoint
- https://www.youtube.com/watch?v=Jq5XIEsaVyE
