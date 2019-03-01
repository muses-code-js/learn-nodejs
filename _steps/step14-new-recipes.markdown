---
layout: step
number: 14
title: Adding a new recipe
permalink: step14/
---

So we can edit recipes now.  That's cool, but what about the page for adding new ones?

Well the steps for this is almost exactly the same as for the edit page.  And we get to reuse the form partial from before so it's even easier.

So our steps are:

1. Create and export the database function to create a recipe
2. Create a route & template that will display a form for us to specify the values for the new recipe
3. Create a route for the form to use to save the new recipe

## Create addRecipe database function

As before when we want to do a new thing with the database we should create a new function for that and export it.  Our new database function is going to be passed the `name` and `content` for a new recipe to save in the database but we are going to be lacking one piece, the `id`.  

The recipe's `id` has to be unique number in the database.  So what we will do is create a function that will tell us what the next `id` to use is.  Then we will use that in `addRecipe`.

So let's create our `getNextId` function in `recipesDB.js`:

```javascript
function getNextId(){
  var recipeWithBiggestId = db.get('recipes').maxBy(function(recipe){ 
		return recipe.id; 
	});
  return recipeWithBiggestId.value().id+1;
}
```
What this function does is ask the database what the recipe is with the largest `id` value, and returns that number plus `1`.

Now that we have that, let's create our function to add new recipes to `recipesDB.js`:

```javascript
function addRecipe(name, content){
	var newRecipe = { 
	  id: getNextId(), 
	  name: name, 
	  content: content 
	};
  db.get('recipes')
  .push(newRecipe)
  .write();
}
```

Then we export `addRecipe`.

```javascript
module.exports = {
  getAllRecipes: getAllRecipes,
  getRecipe: getRecipe,
  updateRecipe: updateRecipe,
  addRecipe: addRecipe
};
```

Note that we don't export `getNextId`?  That's because we are only going to use it within the same file.  It won't get used anywhere else.

## Add route and template for new recipe form

Now lets add the route and template for the page which will have the form for creating as new recipe.  This is almost exactly the same as editing a recipe, except our URL will be `/admin/recipe/new` with no URL parametes and we won't have a recipe to start with so we'll create a blank one to use.

Add the route to `server.js`:

```javascript
server.get('/admin/recipe/new', function(request, response){
  var newRecipe = {
	name: '',
	content: ''
	};
  response.render('pages/admin/new', {recipe: newRecipe})
});
```

Now lets create our template at `views/pages/admin/new.ejs`:

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

This is almost exactly the same as our `edit.ejs` except our `action` value for the form is different.

## Add route to save new recipe

Now we just need to add the route which this form will submit to.

First import our `addRecipe` database function at the top of `server.js`:

```javascript
var addRecipe = require('./recipesDB.js').addRecipe;
```

Then add the following new route in `server.js` before the route for `/admin/recipe/:id`:

```javascript
server.post('/admin/recipe/new', function(request, response){
  var recipe = {
    name: request.fields.name,
    content: request.fields.content
  };
  addRecipe(recipe);
  response.redirect('/admin');
});
```

It is very important to add this new route before the one for `/admin/recipe/:id`.

The order that routes are added to our server determines the order that they are checked in.  If you had these in the wrong order, the route for updating a recipe would be matched first and it would try (and fail) to update a recipe with the id of `'new'`.

Ok.  Save those changes and restart your server.

## Testing adding a new recipe

Let's add a muffin recipe.

Go to <http://localhost:3000/admin> and click `Add new Recipe`.

You'll see the new recipe page like this:

![New recipe page]({{ '/assets/steps/14/add-new.png' | relative_url }}){:title="New recipe page" class="img-responsive imgbox"}


Set the name to `Muffins`, and add the content from below:

```
This recipe makes 12 plain muffins.  

Refer to directions at the bottom for how to modify the recipe for different types of muffins.

## Ingredients:

 * 2 cups white flour
 * 1 tablespoon baking powder
 * 1/2 teaspoon salt
 * 2 tablespoons sugar
 * 1 egg, slightly beaten
 * 1 cup milk
 * 1/4 cup melted butter

## Method:

1. Preheat the oven to 375°F.
2. Butter muffin pans.
3. Mix the flour, baking powder, salt, and sugar in a large bowl.
4. Add the egg, milk, and butter, stirring only enough to dampen the flour; the batter should not be smooth.
5. Spoon into the muffin pans, filling each cup about two-thirds full.
6. Bake for about 20 to 25 minutes each.

## Variations:

### Blueberry Muffins:
 * Use 1/2 cup sugar.
 * Reserve 1/4 cup of the flour, sprinkle it over 1 cup blueberries, and stir them into the batter last.

### Pecan Muffins:
 * Use 1/4 cup sugar.
 * Add 1/2 cup chopped pecans to the batter.
 * After filling the cups, sprinkle with sugar, cinnamon, and more chopped nuts.

### Whole-Wheat Muffins:
 * Use 3/4 cup whole-wheat flour and 1 cup white flour.

### Date or Raisin Muffins:
 * Add 1/2 cup chopped pitted dates or 1/3 cup raisins to the batter.

### Bacon Muffins:
 * Add 3 strips bacon, fried crisp and crumbled, to the batter.
```

Click save.  Now go to the homepage and you will see our new Muffin recipe.  Click it and you will be taken to the recipe page for Muffins.

![New recipe]({{ '/assets/steps/14/new-recipe.png' | relative_url }}){:title="New recipe" class="img-responsive imgbox"}
