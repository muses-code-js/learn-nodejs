---
layout: step
number: 10
title: Adding the Recipe Page
permalink: step10/
---

```js
function getRecipe(id){
  var recipe = db.get('recipes').find({id: parseInt(id) }).value();
  return recipe;
}
```

```js
module.exports = {
  getAllRecipes: getAllRecipes,
  getRecipe: getRecipe,
};
```

`npm install marked --save`

```js
var marked = require('marked');

// GET /recipe/:id
server.get('/recipe/:id', function(request, response) {
  var recipe = getRecipe(request.params.id);
  response.render('pages/recipe', { recipe: recipe, marked: marked  });
});
```

`views/pages/recipes.ejs`

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
      <h1><%= recipe.name %></h1>
      <div><%- marked(recipe.content) %></div>

  </div>

  </body>

</html>
```
