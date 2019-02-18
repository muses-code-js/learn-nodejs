---
layout: step
number: 6
title: Templates
permalink: step6/
status: draft
---

```
npm install ejs --save
```

```js
server.set('view engine', 'ejs');
```

mkdir views/pages/

copy index.html & about.html to index.ejs about.ejs

```js
// GET /
server.get('/', function(request, response) {
  response.render('pages/index');
});

// GET /about
server.get('/about', function(request, response) {
  response.render('pages/about');
});

```
