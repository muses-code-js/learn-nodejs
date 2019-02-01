---
layout: step
number: 9
title: Adding a Database
permalink: step9/
---

```js
var low = require('lowdb');
var FileSync = require('lowdb/adapters/FileSync');

var adapter = new FileSync('db.json');
var db = low(adapter);

db.defaults({
  recipes: [
    {
      id: 1,
      name: 'Traditional Cupcakes',
      content: 'content goes here'
    }
  ]
}).write();

```

```js
function getAllRecipes(){
  var recipes = db.get('recipes').value();
  return recipes;
}

module.exports = {
  getAllRecipes: getAllRecipes
}
```

```js
var recipesDB = require('./recipesDB.js');

// GET /
server.get('/', function(request, response) {
  var recipes = recipesDB.getAllRecipes();
  response.render('pages/index', { recipes: recipes });
});
```

```js
<ul>
  <% recipes.forEach(function(recipe){ %>
    <li><a href="/recipe/<%= recipe.id %>"><%= recipe.name %></a></li>
  <% }) %>
</ul>
```
