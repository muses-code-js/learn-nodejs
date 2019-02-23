---
layout: step
number: 3
title: Create Server
permalink: step3/
status: rewrite
---

```js
var express = require('express');
var server = express();

server.listen(3000, function() {
  console.log('server has started running');
});
```

***

Since we are going to be building a server, we should probably talk about what a server *is*.

A **server** is a computer program that provides some kind of functionality to other programs (called **clients**). The client and server programs are often (but not always) running on different computers.  We often refer to the computer that the server software is running on as a server, too.

![Server flow](../assets/step3-a.png){:class="img-responsive imgbox" title="Server flow"}

A server "listens" for requests sent to it from clients.  Depending on the request, the server may perform some kind of work and then send a response back to the client.  For example, a client might send a request for information that the server looks up in a database and then sends back as the response.  If the server cannot fulfill the request for some reason then the response could be an error message like the famous *'Error 404: File not found.'*  

This idea of having server programs that respond to requests from server programs is called a **client/server architecture**.  It works because both the clients and the servers use the same format for the requests and responses.  This agreed upon format is called the **protocol**.

Client/server architecture is used when you have a resource that you need multiple users to access at the same time. Instead of sending the resource to all of the users (e.g. on a CD-ROM), you can keep the resource in one place and use a server to allow clients to access it. There are many benefits to doing this, such as how easy it is to update the shared resource.

The application we are creating is a **web server**.  The client is your web browser.  They use the `http` protocol to communicate.  The browser sends requests to the server using URLs to indicate what it wants from the server.

But enough talk.  Let's code!

## Creating server.js

All of the code that we are going to write in this tutorial will go in one file,
`server.js`.  

We will now create that file and write the code to start the server running.

### Create a server.js file

Create a new file called `server.js`. You can do this using the `File` menu in your text editor. Once created you will see it listed in the file view. Double-click on it to open. It should be completely empty.

### Import the Express module

We already installed Express in Step 2, but we need a way to refer to it in our code so we can use it.  We do that using the function `require`.  

To import Express, write the following at the top of `server.js`:

```javascript
var express = require('express');
```

Now we have a variable `express` that points to whatever the Express package  exports.  We could have called this variable anything we wanted but it's common practice to use the same name as the package.

### Create the server object

The Express module exports a single function.  When we **invoke** that function it returns an **Express application** object.  That object provides the all the behaviour that we use to build our server.

Add the second line of code to your `server.js` file:

```javascript
var express = require('express');
var server = express();
```

This new line invokes `express()` and stores the result in the variable `app`.

### Start our server "listening"

Now we have our server object (`app`), we can use its `listen()` function to start listening for requests.

The `listen()` function takes two arguments: a **port** number and a **callback function**.

The **port number** can be thought of as a door number or apartment number.  Requests for this server will be sent "to that port". As every apartment on the same street has a different door number, every server running on the same computer must be listening on a different port. We will use port `3000`.

The **callback function** is a function for the server to run immediately after it starts listening.  We are going to use it to display a message to let us know that the server has started listening.

Add the last three lines to `server.js`:

```javascript
var express = require('express');
var server = express();

server.listen(3000, function () {
  console.log('Server has started listening on port 3000.');
});
```

## Start it up

You've built your server, but it isn't running yet.

To run a Node.js program we use the `node` program.

Type the following command in your terminal (from the same directory as `server.js`) and press ENTER:

```
node server.js
```

<!-- If you are using cloud9, you can also select the file `server.js` in the workspace folder tree and click the `Run` button on the top menu. -->

In the terminal you will see it display the "Server has started listening" message.

It will look a little like this:

![Running node server.js in the terminal](../assets/step3-b.png){:title="Running node server.js in the terminal" class="img-responsive imgbox"}

If you see this, congratulations! :clap: :clap: You have built yourself a server and started it running!

## About console.log()

Right now, your server is doing one thing: using the `console.log()` function to print the message `Server has started listening on port 3000.`

The `console.log()` function displays messages in the terminal, also known as the console. We call these 'console' or 'log' messages, and they only appear in the terminal where your application is running - users of your app don't see them.

When an app is complete, we generally use log messages to display status information and error messages from our program. While we're still building the app, though, we can also use `console.log()` to look into what the program is doing while it runs. For example, we can use `console.log()` to check the value of a variable before and after an operation to make sure that our program is doing what we expect it to do.

This workshop uses `console.log()` a lot to make sure you can see what is happening in each part of the code. You can remove them if you like - just remember that `console.log()` is useful when you're trying to debug a problem.
