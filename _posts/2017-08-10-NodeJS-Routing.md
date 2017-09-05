---
layout: post
title: NodeJS 开发 - Routing
date: 2017-08-10
tag: NodeJS, express, web, route
comments: true
---


Web frameworks provide resources such as HTML pages, scripts, images, etc. at different routes.


## Simple Routing

The function template ***app.method(path, handler)*** can be applied to any one of the HTTP verbs – get, set, put, delete.

#### Example
```js
var express = require('express');
var app = express();

app.get('/hello', function(req, res){
   res.send("Hello World!");
});

app.listen(3000);
```
If we run our application and go to *localhost:3000/hello*, the server receives a get request at route **"/hello"**, our Express app executes the callback function and sends the "Hello World!" as response.

## Advanced Routing
Defining routes like above is very tedious to maintain. We want to separate the routes from our main **index.js** file, we will use **Express.Router**. 

For example, if we want to organize a section of html pages into a group under *index/sub_index*, we can then create a file called **sub_index.js**:
```js
var express = require('express');
var router = express.Router();

router.get('/', function(req, res){
   res.send('GET route on sub_index.');
});
router.post('/', function(req, res){
   res.send('POST route on sub_index.');
});

//export this router to use in our index.js
module.exports = router;
```

Now to use this router in our index.js, type in the following before the app.listen function call.
```js
var express = require('Express');
var app = express();

var things = require('./sub_index.js');

//both index.js and things.js should be in same directory
app.use('/sub_index', sub_index);

app.listen(3000);
```
The **app.use()** function call on route '/things' attaches the things router with this route. Now whatever requests our app gets at the '/sub_index', will be handled by our sub_index.js router. The '/' route in sub_index.js is actually a subroute of '/sub_index'. Visit **localhost:3000/sub_index/** and you will see the following output.


## Dynamic Routes & URL Building



