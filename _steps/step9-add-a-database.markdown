---
layout: step
number: 9
title: Adding a Database
permalink: step9/
status: screenshot
---

Most web applications store their content in a database of some kind. When you post an update on Facebook or Twitter, or add a review on Amazon, it goes into a database.  Then when someone views those pages, the web application is retrieving that content and rendering it in a template.

There are many different databases you could use, like MySQL, MariaDB, PostgreSQL, Microsoft SQL Server, Oracle and MongoDB to name just a few.  However installing, configuring and running anyone of those databases is a whole workshop in itself.

So we aren’t going to delve into one of those.  What we are going to do is use a package which lets us store data in a file locally kind of like a database.  And we are going to do it in a way that will make it easy to switch out databases at a later stage.

## Create our database using lowdb

First we need to install the database package that we are going to use.  Go to the terminal again, stop the server and install the package `lowdb` using `npm` like this:

```
npm install lowdb —save
```

Once we have that installed, lets create a new file, `recipesDB.js`.  This is where we are going to put all the code which uses `lowdb`.

First up lets require the bits from the lowdb package that we need:

```javascript
var low = require('lowdb');
var FileSync = require('lowdb/adapters/FileSync');
```

Now we tell it to use the file `recipesDB.json` to store our data:

```javascript
var adapter = new FileSync('recipesDB.json');
var db = low(adapter);
```

And now we create some default data for our new database:

```javascript
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

That’s everything we need to setup out lowdb database, but we’re going to add a little extra.

We could use this `db` object directly in our routes and it would work fine but we would be getting our database and routes code all tangled together.  It’s better if you can keep those things separated.  To keep them separated we’ll create some functions to interact with the database and those those from the routes instead.

The first function we’ll create is one to get all the recipes that we have in the database:

```javascript
function getAllRecipes(){
  var recipes = db.get('recipes').value();
  return recipes;
}
```

And then we export this function in an object so we can use it in `server.js`:

```javascript
module.exports = {
  getAllRecipes: getAllRecipes
}
```

Ok so make sure you save your changes to `recipesDB.js` and lets go back to `server.js`.

## Using our database

Ok so now we have setup our database in `recipesDB.js`, we need to be able to access it in `server.js`.  To do that we use `require()` in a very similar way to what we did when importing Express and EJS.

Add the following line to `server.js` just after the lines with `require` that you already have:

```javascript
var getAllRecipes = require('./recipesDB.js').getAllRecipes;
```

You can use `require()` on a file as well as a package.  What can be accessed when you require a file is defined by the `module.export` statement in that file.  Note also you have to specify the path to the file, which is why we have the `./recipesDB.js` instead of just `recipesDB.js`.  The `.` at the start of a path means "the folder that I am currently in" (which isn't just a Javascript thing BTW).

So now we have the function `getAllRecipes` available in `server.js`, so lets update our route for the home page to use it:

```javascript
server.get('/', function(request, response) {
  var recipes = getAllRecipes();
  response.render('pages/index', { recipes: recipes });
});
```

So now the route for `/` calls the function `getAllRecipes()` and stores the result in the variable `recipe`.  Then it renders the template and passes the `recipes` variable to it with the same name. 

So now lets update the template to make use of that data.

Open up `views/pages/index.ejs` and find the `<ul>` which displays the list of all our recipes and links to their pages.  It looks like this:

```html
<ul>
  <li><a href="traditional-cupcakes.html">Traditional Cupcakes</a></li>
  <li><a href="muffins.html">Muffins</a></li>
</ul>
```

Lets replace it with the following:

```html
<ul>
  <% recipes.forEach(function(recipe){ %>
    <li><a href="/recipe/<%= recipe.id %>"><%= recipe.name %></a></li>
  <% }) %>
</ul>
```

There's another EJS tag, `<% %>`, the **scriptlet** tag.  Anything within these is will be run as Javascript.  We use it here for a `forEach` loop which will output a `<li>` for every recipe we have.  But notice how we have the first line of the `forEach` statement in the scriptlet tag, then some HTML with output tags, and then we have another scriptlet tag with the end of the loop?  You can just embed javascript like this with scriptlet tags, but any time you want that embedded Javascript to return HTML to become part of the template you can just close the scriptlet, put the HTML, and then open a new scriptlet tag and continue your Javascript.  It's pretty wild.  

Anyway... make sure you have saved all the changes to `recipesDB.js`, `server.js` and `views/pages/index.ejs` and restart our server and check the homepage.

It should look like this:

[INSERT SCREENSHOT]

Now instead of links to specific files, the homepage has links to the URLs that match all the recipe ids that we have in our database.  Right now we only have one.

Ok so that was a lot so lets quickly review all the steps we did:

1. Created a new file `recipesDB.js` to hold all the code for setting up and interacting with the database which will hold our recipes.
2. We added an `module.exports` statement to `recipesDB.js` so that the specific functions that we want to access can be imported into `server.js` using `require`  
3. We used `require` to import `recipesDB.js` into `server.js`
4. We updated our route for `/` to use `getAllRecipes()` to get a list of all the recipes and pass it to our template for the homepage.
5. We updated the template for our homepage remove the old list of recipe links and instead create a list of all the recipes passed to it by from the route handler.

So now when we add new recipes to our database in a few steps they will automatically appear on the homepage.