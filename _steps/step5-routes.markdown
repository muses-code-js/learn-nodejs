---
layout: step
number: 5
title: Creating Routes
permalink: step5/
status: draft
---

Ok so our server is running and listening to requests but it doesn't know what to do when it receives one yet.

We use **routes** to tell our server what requests to respond to and what do for each.

## Adding a route

To add a route you call the function for the request method, using the URL and your handler function as parameters.

Let's add a route for the request `/hello` that will respond by sending the text `hello there`.


At the following code to `server.js` before line that starts with `server.listen`:

```js
server.get('/hello', function(request, response) {
  response.send('hello there');
});
```

So now your whole `server.js` file should look like: 

```javascript
var express = require('express');
var server = express();

server.get('/hello', function(request, response) {
  response.send('hello there');
});

server.listen(3000, function () {
  console.log('Server has started listening on port 3000.');
});
```

Changes that your make to your server code only take effect when the server starts up.  This means that any time you make changes, you will have to stop your server and restart it to see the changes.

Head back to your terminal now and stop your server by pressing the `control` and `c` keys (AKA `ctrl-c`).  In the terminal `ctrl-c` tells the currently running program to stop right away.

Start your server running again by typing `node server.js` into your terminal and pressing enter just like before.

Now open a web browser and go to <http://localhost:3000/hello>

You should see the text displayed: `hello there`

[INSERT SCREENSHOT]



===

A **route** is made up of three things:

1. A URL path
2. A HTTP request method
3. A handler function

When the server receives a request, it looks at the request’s URL path and request method. If it has a route that matches both, then it responds to the request by running the handler function associated with the matching route.

<!-- 
We add routes to our server to tell it which requests to respond to, and what to do for the response in each case. -->


