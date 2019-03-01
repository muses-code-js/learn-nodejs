---
layout: step
number: 1
title: Project setup
permalink: step1/
---

The first thing we do when starting a new Node.js project is create a `package.json` file.

`package.json` is the configuration file used by the program `npm`, the **Node Package Manager**.  It is used to configure lots of things in your project including package dependencies, automating tasks, and how to publish it as a package.  

However the only thing we are going to use it for in this workshop is saving our project dependencies.

## Creating a new package.json file

First create a folder for our project and call it whatever you want, I called mine `learn-nodejs-workshop` so that's what I will use in the examples.

To create your new `package.json` file:

1. Open your terminal
2. Make sure you are in your new project folder.  Use the `pwd` command to check which folder you are in, and the `cd` command to change folders.
3. Run the command `npm init -y`.

This command creates a new `package.json` file with default values and displays it for you.  

Your file will look something like:

```json
{
  "name": "learn-nodejs-workshop",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

The syntax in this file is called **JSON** (pronounced like 'Jason') which is short for **Javascript Object Notation**. It's a way of representing Javascript data that is easy to read and write to and we use it a lot for saving and sending information.  We talk more about JSON in later steps.

Now we have a `package.json` file, let's add a dependency.
