Below you will find Workiva's code standards for developing flexible, durable, and sustainable HTML / Template markup.

+ __[Golden rule](#golden-rule)__
+ __[Pragmatism over semantics](#pragmatism-over-semantics)__
+ __[Formatting](#formatting)__
+ __[HTML5 doctype](#html5-doctype)__
+ __[Language attribute](#language-attribute)__
+ __[IE compatibility mode](#ie-compatibility-mode)__
+ __[Character encoding](#character-encoding)__
+ __[CSS and JS includes](#css-and-javascript-includes)__
+ __[Attribute order](#attribute-order)__
+ __[Boolean attributes](#boolean-attributes)__
+ __[Attribute indentation](#attribute-indentation)__
+ __[JavaScript generated markup](#javascript-generated markup)__

---


## Golden rule

> All code in any code base should look like a single person typed it, no matter how many people contributed.

This means strictly enforcing these agreed upon guidelines at all times. For additions or contributions, please [submit an issue][new-issue].



## Pragmatism over semantics

Strive to maintain HTML standards and semantics, but don't sacrifice pragmatism. Use the least amount of markup with the fewest intricacies whenever possible.



## Formatting

* Use soft-tabs with four spaces
* Nested elements should be indented once, except for core `<body>` and `<head>` elems
* Always use double quotes, never single quotes for element attributes
* Don't include a trailing slash in self-closing elements, as the [HTML5 Spec](http://dev.w3.org/html5/spec-author-view/syntax.html#syntax-start-tag) says they're optional
* Don't omit optional closing tags (e.g. `</li>` or `</body>`)

__Incorrect example:__

```html
<!DOCTYPE html>
<html>
<head>
<title>Page title</title>
<link rel='stylesheet' href='base.css' />
<script type="text/javascript" language="javascript" src="..."></script>
<link rel="stylesheet" href="style.css" />
</head>
<body>
<img src='images/company-logo.png' alt='Company' />
<h1 class='hello-world'>Hello, world!</h1>
</body>
</html>
```

__Correct example:__

```html
<!DOCTYPE html>
<html>
<head>
    <title>Page title</title>
    <link rel="stylesheet" href="base.css">
    <link rel="stylesheet" href="style.css">
    <script src="..."></script>
</head>
<body>
    <img src="images/company-logo.png" alt="Company">
    <h1 class="hello-world">Hello, world!</h1>
</body>
</html>
```


## HTML5 doctype

Enforce standards mode in every browser with the HTML5 doctype at the beginning of every HTML page / template.

```html
<!DOCTYPE html>
<html>
    <head>
    </head>
</html>
```


## Language attribute

From the HTML5 spec:

> Authors are encouraged to specify a lang attribute on the root html element, giving the document's language. This aids speech synthesis tools to determine what pronunciations to use, translation tools to determine what rules to use, and so forth.

Read more about the `lang` attribute [in the spec](http://www.w3.org/html/wg/drafts/html/master/semantics.html#the-html-element).

Head to Sitepoint for a [list of language codes](http://reference.sitepoint.com/html/lang-codes).

```html
<html lang="en">
    <!-- ... -->
</html>
```


## IE compatibility mode

Internet Explorer supports the use of a document compatibility `<meta>` tag to specify what version of IE the page should be rendered as. Unless circumstances require otherwise, it's most useful to instruct IE to use the latest supported mode with __edge mode__.

For more information, [read this awesome Stack Overflow article](http://stackoverflow.com/questions/6771258/whats-the-difference-if-meta-http-equiv-x-ua-compatible-content-ie-edge-e).

```html
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
```


## Character encoding

Quickly and easily ensure proper rendering of your content by declaring an explicit character encoding. When doing so, you may avoid using character entities in your HTML, provided their encoding matches that of the document (generally `UTF-8`).

```html
<head>
    <meta charset="UTF-8">
</head>
```


## CSS and JavaScript includes

Per HTML5 spec, typically there is no need to specify a type when including CSS and JavaScript files as `text/css` and `text/javascript` are their respective defaults.

```html
<!-- External CSS -->
<link rel="stylesheet" href="wdesk.css">

<!-- In-document CSS -->
<style>
  /* ... */
</style>

<!-- JavaScript -->
<script src="wdesk.js"></script>
```


## Attribute order

HTML attributes should come in this particular order for easier reading of code.

* `class`
* `id`, `name`
* `data-*`
* `src`, `for`, `type`, `href`, `tabindex`
* `title`, `alt`
* `role`, `aria-*`

Classes are how Workiva's reusable components are styled, so they come first. Ids are more specific and should be used sparingly (e.g., for in-page bookmarks), so they come second.


## Boolean attributes

A boolean attribute is one that needs no declared value. XHTML required you to declare a value, but __HTML5 has no such requirement__.

For further reading, consult the [WhatWG section on boolean attributes](http://www.whatwg.org/specs/web-apps/current-work/multipage/common-microsyntaxes.html#boolean-attributes):

> The presence of a boolean attribute on an element represents the true value, and the absence of the attribute represents the false value.

If you must include the attribute's value, and __you don't need to__, follow this WhatWG guideline:

> If the attribute is present, its value must either be the empty string or [...] the attribute's canonical name, with no leading or trailing whitespace.

__In short, don't add a value.__

```html
<input type="text" disabled>

<input type="checkbox" value="1" checked>

<select>
    <option value="1" selected>1</option>
</select>
```


## Attribute indentation

Sometimes, HTML attributes can become incredibly long and unreadable _(especially when making components comply with our [Accessibility Requirements][html-accessibility], or using an HTML data-api for a JS plugins)_.

Remember - this code is meant to be __read by humans__. Indent the attributes _(using spaces)_ so that they line up with the first attribute which should be on the same line as the tag opening.  Yes - it takes up more lines, but our production templates should be getting minified anyway - so __make the markup readable__.

__Incorrect example:__

```html
<button type="button" class="btn dropdown-toggle" title="Detailed desc of what clicking this dropdown menu will reveal" aria-haspopup="true" aria-expanded="true" data-toggle="dropdown">Dropdown Button <i class="caret" aria-hidden="true"></i></button>
```

__Correct example:__

```html
<button class="btn dropdown-toggle"
        data-toggle="dropdown"
        type="button"
        title="Detailed desc of what clicking this dropdown menu will reveal"
        aria-haspopup="true"
        aria-expanded="true">
    Dropdown Button
    <i class="caret" aria-hidden="true"></i>
</button>
```


## JavaScript generated markup

__Writing HTML markup in a `.js` file makes it harder to find, harder to edit, and less performant. Avoid it whenever possible.__

> The obvious __exception is `.jsx` files used by ReactJS__, which exist for the explicit purpose of writing dynamic HTML markup.


---


The Workiva HTML code conventions are heavily inspired by [Mark Otto's Code Guide](http://mdo.github.io/code-guide/) and the [Github Styleguide](http://github.com/styleguide).



[new-issue]: /WebFilings/w-coding-conventions/issues/new
[html-accessibility]: http://bit.ly/webskin-a11y
