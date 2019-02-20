---
layout: step
number: 9
title: Adding a Database
permalink: step9/
status: draft in progress
---

Most web applications store their content in a database of some kind. When you post an update on Facebook or Twitter, or add a review on Amazon, it goes into a database.  Then when someone views those pages, the web application is retrieving that content and rendering it in a template.

There are many different databases you could use, like MySQL, MariaDB, PostgreSQL, Microsoft SQL Server, Oracle and MongoDB to name just a few.  However installing, configuring and running anyone of those databases is almost a workshop in itself.

We aren’t going to delve into one of those.  We are going to use a package which lets us store data in a file locally kind of like a database.  And we are going to do it in a way that will make it easy to switch out databases at a later stage.

## Create our database using lowdb

`npm install lowdb —save`


Now lets create a new file, `recipeDB.js`.  This is where we are going to put all the code which uses `lowdb`.

First up lets require the bits from the lowdb package that we need:

var low = require('lowdb');
var FileSync = require('lowdb/adapters/FileSync');

Now we tell it to use the file recipeDB.json to store our data:

var adapter = new FileSync('recipeDB.json');
var db = low(adapter);

And now we create some default data for our new database:

db.defaults({
  recipes: [
    {
      id: 1,
      name: 'Traditional Cupcakes',
      content: 'content goes here'
    }
  ]
}).write();

That’s everything we need to setup out lowdb database, but we’re going to add a little extra.

We could use this `db` object directly in our routes and it would work fine but we would be getting our database and routes code all tangled together.  It’s better if you can keep those things separated.  To keep them separated we’ll create some functions to interact with the database and those those from the routes instead.

The first function we’ll create is one to get all the recipes that we have in the database:

function getAllRecipes(){
  var recipes = db.get('recipes').value();
  return recipes;
}

And then we export this function in an object so we can use it in `server.js`:

module.exports = {
  getAllRecipes: getAllRecipes
}

Ok so make sure you save your changes to `recipesDB.js` and lets go back to `server.js`.




