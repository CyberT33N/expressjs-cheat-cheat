# expressjs-cheat-cheat
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


# Postman




## How to send JSON data to an API endpoint
- https://www.youtube.com/watch?v=Jq5XIEsaVyE
