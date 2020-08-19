# expressjs-cheat-cheat
Express.js Cheat Sheet with the most needed stuff..




<br />
<br />


 _____________________________________________________
 _____________________________________________________


<br />
<br />

# Routes

## Respond with Hello World! on the homepage:
```javascript
app.get('/', function (req, res) {
  res.send('Hello World!')
});
```

## Load html file
```javascript
res.render('./website/index.html');
```

## Load full project
```javascript
app.use(express.static(__dirname + '/public'));
```
