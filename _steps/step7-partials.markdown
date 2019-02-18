---
layout: step
number: 7
title: Partials
permalink: step7/
status: write
---

mkdir views/partials

create views/partials/header.ejs

```html
<header>
  <div class="container">
    <img src="/tinycakes.png" title="Tiny Cakes!" alt="The Tiny Cakes logo, a stylized cartoon cupcake." height="48px" width="48px" />
    <h2>Tiny Cakes!</h2>
    <nav>
      <a href="/">Home</a>
      <a href="/about">About</a>

    </nav>
</div>
</header>
```

update index & about

```js
<% include ../partials/header %>
```
