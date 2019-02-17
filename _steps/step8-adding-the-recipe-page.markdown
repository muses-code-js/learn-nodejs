---
layout: step
number: 8
title: Adding the Recipe Page
permalink: step8/
status: write
---


mkdir views/pages/recipe.ejs

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

```js
server.get('/recipe/:id', function(request, response) {
  response.render('pages/recipe', { recipeId: request.params.id });
});
```
