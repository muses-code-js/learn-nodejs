---
layout: step
number: 8
title: Adding the Recipe Page
permalink: step8/
status: screenshot
---

Ok so we have seen how templates and partials can save us heaps of time and make maintenance easier.   The other thing that you can do is creating a template that gets used for multiple pages. This means that for all of our recipes we will only have a single template.  This will save us so much time in the long run.

We do this using two features:

1. URL parameters
2. Passing a data object to `render()` 

## Creating Routes with URL parameters

A URL parameter is a both a way of saying that part of a route’s URL can have any value and a way of extracting that value to use.  This is how we can have a single route that matches multiple similar URLs where the part of the URL that differs lets us know something about what item is supposed to be being displayed.  

First up lets create a new route:

```javascript
server.get('/recipe/:id', function(request, response) {
	response.send('recipe id: '+ request.params.id); 
});
```

Obviously this route doesn’t do much yet, but let's save and restart our server,
and open up <http://localhost:3000/recipe/123>

You should see the text `recipe id: 123`

![URL parameter sent as response]({{ '/assets/steps/8/params-send.png' | relative_url }}){:title="URL parameter sent as response" class="img-responsive imgbox"}


Now try some different values, like <http://localhost:3000/recipe/99834> or 
<http://localhost:3000/recipe/cheesecake>

The `:id` in the URL is our URL parameter.  Express will match any request URL here that that has anything in that spot and copy that value to `request.params` as a property with the same name.  

So `:id` gets copied to `request.params.id`.

Then we just used `response.send` to send a simple message with `request.params.id` in it.

Using this we can extract information from the URL to use in our route to do whatever we need to.

What do you think the value of `request.params.id` would be if you visited <http://localhost:3000/recipe/> ?  

Try it.  What happens?

In order for that route to match there has to be something in the URL where the parameter is, otherwise it won’t match.

(FYI You can make routes that have URLs with optional parameters, but it gets complicated and we won’t be doing that today)

So we can extract this `id` value from the URL.  So what?  What doesn't that let us do?  Well in a few steps time we are going to be creating our recipes and storing them in a database.  Each recipe is going to have it’s own unique `id` value.  The `id` in the URL is how we will know which recipe to get from the database to display in the template.  

But that is a couple of steps ahead yet, let’s first look at how we pass data into our templates from our route handler.

## Passing data to templates

So our route URL for a recipe contains our recipe `id` URL parameter.  We don’t have our recipe database setup yet, so lets just display the `id` in our template.

So lets start creating our recipes template.

Create the file `views/pages/recipe.ejs` with the following code:

```html
<!DOCTYPE html>
<html>

  <head>
    <title>Tiny Cakes!</title>
      <link rel="stylesheet" href="/tinycakes.css" />
  </head>

  <body>
    <% include ../partials/header %>
    <div class="container">
      <h1>Showing recipe: <%= recipeId %></h1>
  </div>

  </body>

</html>
```

This page doesn’t do much, but notice the `<%= recipeId %>` ?  

What that does is display the current value of the variable `recipeId`.  Where does that variable come from you might ask, and the answer is that we pass it to the template when we call `render()`.

Let's update our route from above so it looks like the following:

```javascript
server.get('/recipe/:id', function(request, response) {
  response.render('pages/recipe', { recipeId: request.params.id });
});
```

Save this, restart your server and go to <http://localhost:3000/recipe/123> again.

It should look like this:

![URL parameter sent to template and rendered]({{ '/assets/steps/8/params-render.png' | relative_url }}){:title="URL parameter sent to template and rendered" class="img-responsive imgbox"}


Now instead of just instructing Express to render the template, we are also giving it data for the template to display.

The second parameter being passed to render here is an object.  The properties of that object are automatically made available as variables to the template being rendered.  So the value of `request.params.id` is made available to the template as the variable `recipeId`.

Ok now that we now how to use URL parameters and pass data to templates, lets get our database going.
