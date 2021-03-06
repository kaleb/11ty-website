---
eleventyNavigation:
  parent: Layouts
  key: Layout Chaining
  excerpt: Wrap layouts in other layouts.
---
# Layout Chaining

Your layouts can also use a layout! Add the same `layout` front matter data to your layout template file and it’ll chain. You do not have to use the same template engine across layouts and content! You can mix and match.

To chain a layout, let’s look at an example:

{% codetitle "layout-chain-example.md" %}

```markdown
---
layout: mainlayout.njk
title: My Rad Blog
---
# My Rad Markdown Blog Post
```

We want to add a main element around our post’s content because we like accessibility.

<seven-minute-tabs>
  <div role="tablist" aria-label="Template Language Chooser">
    Language:
    <a href="#mainlayout-njk" id="mainlayout-njk-btn" role="tab" aria-controls="mainlayout-njk" aria-selected="true">Nunjucks</a>
    <a href="#mainlayout-11tyjs" id="mainlayout-11tyjs-btn" role="tab" aria-controls="mainlayout-11tyjs" aria-selected="false">11ty.js</a>
  </div>
  <div id="mainlayout-njk" role="tabpanel" aria-labelledby="mainlayout-njk-btn">
    <p>Here’s what <code>mainlayout.njk</code> would look like:</p>
    {%- codetitle "_includes/mainlayout.njk" %}
    {%- highlight "html" %}
    {%- include "examples/layout-chaining/mainlayout.njk" %}
    {%- endhighlight %}
  </div>
  <div id="mainlayout-11tyjs" role="tabpanel" aria-labelledby="mainlayout-11tyjs-btn">
    <p>Here’s what <code>mainlayout.11ty.js</code> would look like:</p>
    {%- codetitle "_includes/mainlayout.11ty.js" %}
    {%- highlight "js" %}
    {%- include "examples/layout-chaining/mainlayout.11ty.js" %}
    {%- endhighlight %}
  </div>
</seven-minute-tabs>

This layout would then be itself wrapped in the same `mylayout.njk` we used in our previous example:

{% codetitle "_includes/mylayout.njk" %}

{% raw %}
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{title}}</title>
  </head>
  <body>
    {{ content | safe }}
  </body>
</html>
```
{% endraw %}

Used together, this would output:

{% codetitle "_site/layout-chain-example/index.html" %}

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Rad Blog</title>
  </head>
  <body>
    <main>
      <h1>My Rad Markdown Blog Post<h1>
    </main>
  </body>
</html>
```

## Addendum about existing Templating features

It is worth noting that existing template reuse mechanisms built into different templating languages are still available to you. For instance, Nunjucks calls it [Template Inheritance](https://mozilla.github.io/nunjucks/templating.html#template-inheritance) and exposes with `{% raw %}{% extends %}{% endraw %}`. Eleventy’s layout system exists a layer above this and exposes a nice multi-template-language mechanism to configure layouts in your content’s front matter and share data between them.
