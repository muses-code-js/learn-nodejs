---
layout: step
number: 13
title: Working with Markdown
permalink: step13/
status: screenshot
---

So we have a problem with the displaying of the content of our recipes.  We are just displaying that content as raw text, not as HTML.  How do we change raw text like this into HTML?  

Well the content that we have is actually written in a special syntax called **Markdown** which is designed to make this easy.

**Markdown** is a very simple markup language designed to be easy to read and easy to convert to HTML.  Markdown is very popular and you will encounter it frequently as a developer.  This whole workshop is actually written in markdown as is a lot of other documentation.

There are also many, many tools and packages for processing Markdown content.  The one we are going to use is called **Marked**.

So first lets install the `marked` package using `npm` just like the previous packages we have installed:

```
npm install marked —save
```

Then import `marked` into `server.js` at the top:

```javascript
var marked = require('marked');
```

`marked` is just a function which returns the HTML version of the text it is passed.  So we’ll update our recipes route so instead of passing the recipe we got from the database to our template, we will make a new recipe object based on that one but with content converted by `marked`.  Then we'll pass that to the template.

Update the `/recipe/:id` route like this:

```javascript
server.get('/recipe/:id', function(request, response) {
  var recipe = getRecipe(request.params.id);
  var newRecipe = {
    id: recipe.id,
    name: recipe.name,
    content: marked(recipe.content)
 };
  response.render('pages/recipe', { recipe: newRecipe });
});
```

Restart our server and lets check what it looks like now:

![Escaped output]({{ '/assets/steps/13/escaped-output.png' | relative_url }}){:title="Escaped output" class="img-responsive imgbox"}


Opps, not quite right yet.  What happened then?

Well the `<%= %>` output tag actually returns what is called **escaped  HTML**.  

**Escaping** characters means converting them to special sequences of characters which will not get interpretted as HTML.  This is very important when you have characters like `<` and `>` in the content you want to output but you don't want the browser to think you are trying to use HTML tags that don't exist.  That can get strange.

But in this case we want the output to be interpreted as HTML because we have already converted it from markdown to HTML and we know it's ok.  

So let's use a different output tag, `<%- %>`, the **unescaped** output tag.

Lets update our `recipe.ejs`  to replace the `<body>` as below so we are displaying `recipe.content` as unescaped HTML:  

```html
  <body>
    <% include ../partials/header %>
    <div class="container">
      <h1><%= recipe.name %></h1>
      <div><%- recipe.content %></div>
  	 </div>
  </body>
```

Refresh the recipe page and it will look much nicer now:

![unescape output]({{ '/assets/steps/13/correct-output.png' | relative_url }}){:title="unescaped output" class="img-responsive imgbox"}
