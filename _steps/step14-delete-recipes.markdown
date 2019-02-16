---
layout: step
number: 14
title: Delete Recipes
permalink: step14/
draft: true
---

```js
function deleteRecipe(id){
  db.get('recipes')
    .remove({id: parseInt(id)})
    .write();
}
```

```js
module.exports = {
  deleteRecipe: deleteRecipe,
  addRecipe: addRecipe,
  getAllRecipes: getAllRecipes,
  getRecipe: getRecipe,
  updateRecipe: updateRecipe
};
```

```js
// GET /admin/recipe/:id/delete
server.get('/admin/recipe/:id/delete', function(request, response){
  deleteRecipe(request.params.id);
  response.redirect('/admin');
});
```

```html
<script>
function deletePrompt(event){
  event.preventDefault();
  if(confirm('Sure you want to delete this recipe?')){
    window.location = event.target.href;
  }
}
</script>
```

```html
<a href="/admin/recipe/<%= recipe.id %>/delete" onClick="deletePrompt(event)">delete</a>
```
