---
layout: step
number: 7
title: Partials
permalink: step7/
---

One of the drawbacks to HTML that you might have noticed is that there is no easy method to share content between pages.  In particular having common content like navigation means having to repeat that content in every page.  Which means when that content changes, you have to update it in every page.  Which is annoying, time consuming and easy to mess up.

However with templates you have a neat feature called partials.

Partials are tiny little templates that are only part of a page.  Once you create a partial, you can tell any template to use that partial and it function as though it is part of that template. 

Putting common content like navigation, site header and site footer in partials makes your site much easier to update.

Lets update our site navigation to use partials and fix the links we broke in the previous step.

## Create our partial

Inside of `views` create the folder `partials`.  This is where we will put our partials.

Now in partials, create the file `header.ejs`

Now open up `views/pages/index.ejs` and find our `<header>` tag.  Select the entire thing, copy and paste it into `header.ejs`.

So now `views/partials/header.ejs` should look like:

```html
<header>
  <div class="container">
    <img src="/tinycakes.png" title="Tiny Cakes!" alt="The Tiny Cakes logo, a stylized cartoon cupcake." height="48px" width="48px" />
    <h2>Tiny Cakes!</h2>
    <nav>
      <a href="index.html">Home</a>
      <a href="about.html">About</a>
    </nav>
</div>
</header>
```

## Include our partial

Now go to `views/pages/index.ejs` and `views/pages/about.ejs` and where the `<header>` is now, replace it with the following:

```html
<% include ../partials/header %>
```
If you refresh the pages it should seem exactly the same.

But here’s the thing.  Our links are still broken right?  Open up `header.ejs` and update the links as below:

```html
<nav>
  <a href="/">Home</a>
  <a href="/about">About</a>
</nav>
```

Now check both the home & about pages in your browser.  The links in the header should be fixed in both pages.  So much more convenient that having to make those changes in both files.  Now imagine if you had dozens or hundreds of pages.  So much easier this way than managing each of them by hand.

[INSERT fixing logo path]