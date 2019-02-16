---
layout: step
number: 4
title: Static Assets
permalink: step4/
draft: true
references:
  - label: Guide to Express Middlware
    url: https://expressjs.com/en/guide/using-middleware.html
  - label: express.static() docs
    url: https://expressjs.com/en/4x/api.html#express.static 
---

Ok so our server is now running and listening for requests, but we haven't told it what requests to respond to or what to when for each.

For that we have two different approaches:

1. Creating routes
2. Setting static assets

We will be dealing with routes next, so lets do static assets first.

## What are Static Assets

Static assets are those files in your app which do not change, hence static.  In a simple website with no dynamic server-side behaviour all the files are static assets.  When a user requests a URL the server just responds by getting that file and sending it to the user.  When you have server-side behaviour that means that some URLs don't map directly to a specific file, and the server will be assembling a new page matching that specific request.  However you still usually have some files that just get served up as is, like CSS files  and images for example.  These files are our static assets.

To use static assets in Express all we have to do is put our static assets in a  folder and tell Express to use it for static assets.  So let's do that with our TinyCakes website from the learn-HTML workshop.

## Setup our files

If you haven't done the learn-HTML workshop or if you don't have the finished project anymore you can download [tiny-cakes-site.zip](tiny-cakes-site.zip).

1. Create a folder inside of our learn-nodejs folder called `public`.  
2. Copy the your HTML, CSS, and image files from your TinyCakes site into `public`.  If you are using the downloaded ZIP, then extract those files from the ZIP file and copy them into `public`.

You should have something that looks like this:

[INSERT FILEMANAGER SCREENSHOT]

## Tell Express about static assets

Now open up your `server.js` file and add the following line right before the line with `server.listen` :

```javascript
var staticAssets = express.static('public');
server.use(staticAssets);
```

So now your `server.js` file should look like:

```javascript
var express = require('express');
var server = express();

var staticAssets = express.static('public');
server.use(staticAssets);

server.listen(3000, function () {
  console.log('Server has started listening on port 3000.');
});
```

Make sure you save those changes.

## Restart your server 

Changes that your make to your server code only take effect when the server starts up.  This means that any time you make changes, you will have to stop your server and restart it to see the changes.

Head back to your terminal now and stop your server by pressing the `control` and `c` keys (AKA `ctrl-c`).  In the terminal `ctrl-c` tells the currently running program to stop right away.  You should see the terminal respond by displaying a prompt for you to type commands in again.

[INSERT SCREENSHOT OF TERMINAL CTRL-C]

Start your server running again by typing node server.js into your terminal and pressing enter just like before.

## Check that it worked

Once you have your server running open a browser window and go to <http://localhost:3000/index.html>

You should see the homepage of the TinyCakes site:

[INSERT SCREENSHOT]

You can navigate to other pages and everything should work.  Your server is now delivering up all those pages as is.  What is happening is that for every request that your app receives Express first checks if a file exists in `public` that has the same name and path, and if it does it responds by sending that file. 

## So how did that work?

Pretty cool right.  So how did that happen in our code?

The first line used the `static()` function of Express to create what we call a **middleware** object.  Middleware is how we add features to Express. Every time your server receives a request it will run it through each middleware we have given it before it does anything else.  Middleware can do anything.  We will be using middleware several times in this workshop.

So here we created a new middleware for handling static assets that are in the folder `public`. 

```javascript
var staticAssets = express.static('public');
```

The next line tells express to use that middleware that we just created.  That's what the function `use()` does, gives our server more middleware to use.  

```javascript
server.use(staticAssets);
```

What we will be doing over the next few steps few steps is taking the individual pages from public and making them dynamic.  When we are finished the only files in public will be images and CSS.
