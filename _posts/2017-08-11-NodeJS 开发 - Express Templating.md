---
layout: post
title: NodeJS 开发 - Express Templating
date: 2017-08-11
tag: NodeJS, express, web, pug, jade, html
comments: true
---

## PUG
Pug (previously jade) is a templating engine for Express. Templating engines are used to remove the cluttering of our server code with HTML, concatenating strings wildly to existing HTML templates.

To use:
```sh
$ npm install --save pug
```

## PUG and HTML
There are many tools converting HTML and PUG. The following example shows their consistency:
```pug
doctype html
html
   head
      title = "Hello Pug"
   body
      p.greetings#people Hello World!
```
can be converted to 
```html
<!DOCTYPE html>
<html>
   <head>
      <title>Hello Pug</title>
   </head>
   
   <body>
      <p class = "greetings" id = "people">Hello World!</p>
   </body>
</html>
```

## Passing Values to Templates
The advantage of using pug templates instead of static html is the ability to passing values and render the page view on server side.

This feature enables the pages content to be dynamic and can be easily linked with database.

### Example
For a new route handling:
```js
var express = require('express');
var app = express();

app.get('/dynamic_view', function(req, res){
   res.render('dynamic', {
      name: "CHAIN SOON EXAMPLE", 
      url:"http://chensunmac.github.io"
   });
});

app.listen(3000);
```

We can create a new view file in views directory, called *dynamic.pug*, with the following code
```pug
html
   head
      title=name
   body
      h1=name
      a(href = url) URL
```

We can also use these passed variables within text. To insert passed variables in between text of a tag, we use #{variableName} syntax. 
For example, we can edit the **h1** line to 
```pug
h1 Greetings from #{name}
```
This is called interpolation (内插).

#### Conditional and Loop in Template
We can use conditional statements and looping constructs as well. 

Consider the following −

If a User is logged in, the page should display "Hi, User" and if not, then the "Login/Sign Up" link. To achieve this, we can define a simple template like −

```pug
html
   head
      title Simple template
   body
      if(user)
         h1 Hi, #{user.name}
      else
         a(href = "/sign_up") Sign Up
```

When we render this using our routes, we can pass an object as in the following program
```js
res.render('/dynamic',{
   user: {name: "younger chain", age: "18"}
});
```
The user content can eventually extract from json files.

#### Include and Components

It is always helpful that we can include components in the template because it avoids of replicate efforts. For example, in a news website, the header with logo and categories is always fixed. Instead of copying that to every view we create, we can use the include feature.

- **Header.pug**
```pug
div.header.
   I'm the header for this website.
```
- **footer.pug**
```pug
div.footer.
   I'm the footer for this website.
```
- **content.pug**
```pug
html
   head
      title Simple template
   body
      include ./header.pug
      h3 I'm the main content
      include ./footer.pug
```
Then by just doing ***res.render('content');***, we can have the view of all the files.


## The static part
Node.JS performs way better than orginal HTML framework in the scenario with lot of dynamic fields. However, static files exist and they are essential elements of web application, for example, images and other resources. These resources are usually put in **public** directory.

Express, by default does not allow you to serve static files. You need to enable it using the following built-in middleware.

```js
app.use(express.static('public'));
```
We can also set multiple static assets directories using the following program 
```js
var express = require('express');
var app = express();

app.use(express.static('public'));
app.use(express.static('images'));

app.listen(3000);
```


