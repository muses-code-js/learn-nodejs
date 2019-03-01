---
layout: step
number: 15
title: Delete Recipes
permalink: step15/
status: screenshot
---

The final step.  Deleting recipes.

This is actually a little simpler in terms of routes and templates because we don't need a special delete page.  We can just link straight to a route that does the delete and redirects back the the admin page.  

So our steps will be:

1. Create and export the database function to delete a recipe
2. Create the route that will delete the recipe


## Add our `deleteRecipe` database function

Of course we need to add a function to `recipesDB.js` to delete a recipe:

```javascript
function deleteRecipe(id){
  db.get('recipes')
    .remove({id: parseInt(id)})
    .write();
}
```

And update the export:

```javascript
module.exports = {
  deleteRecipe: deleteRecipe,
  addRecipe: addRecipe,
  getAllRecipes: getAllRecipes,
  getRecipe: getRecipe,
  updateRecipe: updateRecipe
};
```

## Add route for deleting a recipe

Import our `deleteRecipe` function into `server.js`:

```javascript
var deleteRecipe = require('./recipesDB.js').deleteRecipe;
```

Then add this route to `server.js` for deleting a recipe:

```javascript
server.get('/admin/recipe/:id/delete', function(request, response){
  deleteRecipe(request.params.id);
  response.redirect('/admin');
});
```

Make sure you save the changes, and restart your server.

To test it out just create a new recipe with anything in it, and then click the delete link for that recipe on the admin page.  It will get deleted and you'll be back at the admin page again.

But that delete was maybe a little too easy to do wasn't it?  Like it would be very easy to delete a recipe by accident.  And once the recipe is gone from the database, it is just gone.  There is no way to undelete that.  So lets put a simple safe guard in place.

## Add a confirmation prompt for deleting

What we are going to do is put a little bit of Javascript in the page itself so we will get a confirmation prompt when we click the delete button.  If we click `Ok` then a little bit of javascript code runs to make the browser go to the URL.  If we click cancel then it doesn't.  

Add this code inside of the head tag of `views/pages/admin/index.ejs`:

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
Now we have to tell the delete link to use this.  Find the `<a>` tag which is the delete link in this template and replace it with the following:

```html
<a href="/admin/recipe/<%= recipe.id %>/delete" onClick="deletePrompt(event)">delete</a>
```

FYI this part isn't actually NodeJS code, this Javascript runs inside of the page in the browser, not on the server.  It just makes the experience a little nicer for the user.

Anyway, after saving those changes add another recipe and try to delete it.  Now a prompt will pop up when you click the button and the recipe will only get deleted if you click `OK`.
