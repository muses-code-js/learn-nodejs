---
layout: step
number: 12
title: Editing a Recipe
permalink: step12/
draft: true
---

```js
// GET /admin/recipe/:id/edit
server.get('/admin/recipe/:id/edit', function(request, response){
  var recipe = getRecipe(request.params.id);
  response.render('pages/admin/edit', {recipe: recipe})
});
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
        <div>
          Name: <input name="name" type="text" value="<%= recipe.name %>"/>
        </div>
        <div>
        Content: <textarea name="content" rows="20" cols="80"><%= recipe.content %></textarea>
      </div>
      <input type="submit" value="Save changes" />
      <a href="/admin/">Cancel</a>

      </form>
  </div>
  </body>
</html>
```

```js
function updateRecipe(recipe){
  db.get('recipes')
    .find({id: recipe.id})
    .assign({ name: recipe.name, content: recipe.content})
    .write();
}
```

```js
module.exports = {
  getAllRecipes: getAllRecipes,
  getRecipe: getRecipe,
  updateRecipe: updateRecipe
};
```

`npm install formidable --save`

```js
var formidable = require('express-formidable');
server.use(formidable());
```

```js
// POST /admin/recipe/:id
// update existing one
server.post('/admin/recipe/:id', function(request, response){
  updateRecipe({
    id: request.params.id,
    name: request.fields.name,
    content: request.fields.content
  });
  response.redirect('/admin');
});
```
