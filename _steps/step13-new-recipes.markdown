---
layout: step
number: 13
title: Adding a new recipe
permalink: step13/
draft: true
---

`views/partials/admin/recipe_form.ejs`
```html
<div>
Name: <input name="name" type="text" value="<%= recipe.name %>"/>
</div>
<div>
Content: <textarea name="content" rows="20" cols="80"><%= recipe.content %></textarea>
</div>
<input type="submit" value="Save changes" />
<a href="/admin/">Cancel</a>
```

`views/pages/admin/edit.ejs`
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
      <h1>Editing: <%= recipe.name %></h1>
      <form method="post" action="/admin/recipe/<%= recipe.id %>">
        <% include ../../partials/admin/recipe_form %>
      </form>
  </div>
  </body>
</html>

```

`views/admin/new.ejs`
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
      <h1>Adding new recipe</h1>
      <form method="post" action="/admin/recipe/new">
        <% include ../../partials/admin/recipe_form %>
      </form>
  </div>
  </body>
</html>

```



```js
function getNextId(){
  var nextId = db.get('recipes').maxBy(function(recipe){ return recipe.id; });
  return nextId.value().id+1;
}
```

```js
function addRecipe(recipe){
  db.get('recipes')
  .push({ id: getNextId(), name: recipe.name, content: recipe.content })
  .write();
}
```

```js
module.exports = {
  getAllRecipes: getAllRecipes,
  getRecipe: getRecipe,
  updateRecipe: updateRecipe,
  addRecipe: addRecipe
};
```

```js
// POST /admin/recipe/new
// create new
server.post('/admin/recipe/new', function(request, response){
  var recipe = {
    name: request.fields.name,
    content: request.fields.content
  };
  addRecipe(recipe);
  response.redirect('/admin');
});
```
