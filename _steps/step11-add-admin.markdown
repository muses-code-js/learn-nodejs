---
layout: step
number: 11
title: Adding Admin features
permalink: step11/
---

```js
// ADMIN

// GET /admin
server.get('/admin', function(request, response){
  var recipes = recipesDB.getAllRecipes();
  response.render('pages/admin/index', {recipes: recipes});
});
```

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Tiny Cakes! | Admin</title>
      <link rel="stylesheet" href="/tinycakes.css" />
  </head>
  <body>

    <% include ../../partials/header %>

    <div class="container">
      <h1>Recipe Admin</h1>

    <a href="/admin/recipe/new">Add New Recipe</a>
    <ul class="adminlist">
      <% recipes.forEach(function(recipe){ %>
        <li>
        <%= recipe.name %>
        [<a href="/admin/recipe/<%= recipe.id %>/edit">edit</a> |
        <a  href="/admin/recipe/<%= recipe.id %>/delete">delete</a>]</li>
      <% }) %>
    </ul>

  </div>
  </body>
</html>
```
