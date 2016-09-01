Below you will find Workiva's code standards for developing flexible, durable, and sustainable Sass / CSS.

Before continuing, you should have a general understanding of CSS [terminology][css-terms], [cascading & specificity][css-specificity] and [the basics of Sassy CSS][sass-basics].


+ __[Golden rule](#golden-rule)__
+ __[CSS is code](#css-is-code)__
+ __[Coding style](#coding-style)__
  + [Spacing](#spacing)
    + [Examples](#spacing-examples)
  + [Formatting](#formatting)
    + [Entity casing / separators](#entity-casing-and-separators)
    + [Examples](#formatting-examples)
    + [Selector Nesting](#sass-nesting)
  + [Declaration order](#declaration-order)
+ __[Naming conventions](#naming-conventions)__
  + [Class naming conventions](#class-naming-conventions)
  + [Sass naming conventions](#sass-naming-conventions)
+ __[Specificity](#specificity)__
+ __[Prefixed CSS3 properties](#prefixed-css3-properties)__
+ __[Comments](#comments)__


---




## Golden rule

> All code in any code base should look like a single person typed it, no matter how many people contributed.

This means strictly enforcing these agreed upon guidelines at all times. For additions or contributions, please [submit an issue][new-issue].



## CSS _is_ code

__CSS isn't just visual design.__ Don't throw out programming best practices just because you're writing CSS. Concepts like OOP, DRY, the open/closed principle, separation of concerns, etc. _still apply_ to CSS.



## Coding style

### Spacing

+ Use soft-tabs with a four space indent.
+ Put spaces after `:` in property declarations.
+ Put spaces before `{` in rule declarations.
+ Put line breaks between rulesets.
+ Put line breaks before nested selectors if parent selector has a declaration.
+ When grouping selectors, keep individual selectors to a single line.
+ Place closing braces of declaration blocks on a new line.
+ Each declaration should appear on its own line for more accurate error reporting.


#### Sass spacing

Much the same way JavaScript is used to dynamically alter/create elements on a page, Sass `@mixin`s, `@function`s and `$variable`s are used to dynamically generate static css stylesheets, so the [same rules apply here as they do with Javascript][js-conventions].

+ Put spaces before and after operators.
+ Put a space after `:` and `,`.
+ Put a space before all opening `{`
+ Do not put spaces before `,`, `:` or `;`.
+ Do not put spaces before mixin / function opening `(`
+ Logical blocks should be separated by a blank line.


#### Spacing examples

Here are some good examples that apply the [spacing guidelines](#spacing):

```scss
// Bad examples:
.not-so-good{
  color:#000;
  background-color: #fff;
  border:1px solid #0f0;}
.another-bad-one
{
  color :#000;
  .bad-child-elem {

    .bad-grandchild-elem {
      color: #0f0;
    }
  }
}

@mixin input-error ($tint:#ff0000,$input-classes:input ,input-large){
    background-color: $tint;
    opacity: 1;
    @each $class in $input-classes{
        .#{$class} {
          width: 100%;
        }
    }}


// Good examples:
.good {
    color: #000;
    background-color: #fff;
    border: 1px solid #0f0;
}

.another-good-one {
    color: #000;

    .good-child-elem {
        .good-grandchild-elem {
            color: #0f0;
        }
    }
}

@mixin input-error($tint: #ff0000, $input-classes: input, input-large) {
    background-color: $tint;
    opacity: 1;

    @each $class in $input-classes {
        .#{$class} {
          width: 100%;
        }
    }
}
```



### Formatting

+ Lowercase all hex values: `#fff` instead of `#FFF`.
+ Use shorthand hex values where available: `#fff` instead of `#ffffff`.
+ Use `//` for comments instead of `/* */`.
+ End _all_ declarations with a `;`.
+ Avoid specifying units for zero values: `margin: 0;` instead of `margin: 0px;`.
+ Don't preface decimal values with a leading zero.
+ Quote attribute values in selectors: `input[type="text"]` instead of `input[type=text]`.
+ Strive to limit use of shorthand declarations to instances where you must explicitly set all the available values.


#### Entity casing and separators

In order to make your stylesheets easy to read, your CSS class names, Sass variables, mixins and functions should all follow the same casing and separation conventions that are built-in to the CSS language:

+ CSS class names should always be lowercase, separating words with dashes.
+ Sass variable, mixin and function names should also be lowercase, separating words with dashes.

__Hard to read:__

```scss
// Using casing / separators that differ from the built-in
// CSS properties/values makes code harder to read.
.btnLink {
    @include btnBase;
    @include link_Base;

    font-family: $fontFamilyBase;
    font-weight: normal;
    color: $linkColor;
    text-shadow: $linkText_Shadow;

    &.isActive {
        @include btnLinkActive;
    }
}
```

__Easy to read:__

```scss
.btn-link {
    @include btn-base;
    @include link-base;

    font-family: $font-family-base;
    font-weight: normal;
    color: $link-color;
    text-shadow: $link-text-shadow;

    &.is-active {
        @include btn-link-active;
    }
}
```


#### Sass formatting

+ Sass mixins with no arguments should be included without parentheses: `@include mixin;` instead of `@include mixin()`.
+ Put opening braces on the same line as the `@mixin`, `@function`, `@if`, `@else`, `@for`, `@each` control blocks.


#### Formatting examples

Here are some good examples that apply the [formatting guidelines](#formatting):

```scss
// Example of good basic formatting practices
@mixin styleguide-block {
    background-color: rgba(0,0,0,.5);
    border: 1px solid #0f0;
}

.styleguide-format {
    @include styleguide-block;

    color: #fff;
}

// Example of individual selectors getting their own lines (for error reporting)
.multiple,
.classes,
.get-new-lines {
    display: block;
}

// Avoid specifying units for zero values
.units-on-zero-values-is-bad {
    margin-top: 0px;
}

.unitless-zero-values-are-good {
    margin-top: 0;
}

// Avoid unnecessary shorthand declarations
.not-so-good {
    margin: 0 0 20px;
}

.good {
    margin-bottom: 20px;
}
```


#### Sass nesting

+ As a rule of thumb, avoid unnecessary nesting in SCSS. At most, aim for three levels _(not including pseudo-classes like hover/focus/etc)_.
  + Using meaningful, [namespaced CSS classes](#class-naming-conventions) can help keep your selectors specific without requiring unnecessary nesting.

```scss
// Example of irresponsible usage of the Sass nesting feature
.panel {
    width: 100%;

    .body {
        position: relative;

        .callout {
            display: block;

            .column {
                float: left;
                width: 20%;
            }
        }
    }
}

// Example of responsible usage of the Sass nesting feature
// using class names based on SMACSS to make single
// selectors specific enough on their own
.panel {
    width: 100%;

    &.panel-success {
        .panel-body {
            color: $green;
        }
    }
}

.panel-body {
    position: relative;
}

.panel-body-callout {
    display: block;
}

.panel-body-callout-column {
    float: left;
    width: 20%;
}
```


### Declaration order

Workiva recommends adhering to a variation of the [Recess CSS property order][csscombconfig].

As shown in the example below, related Sass directives / property declarations should be grouped together following the order:

1. Sass directives
2. Positioning
3. Box model
4. Typographic
5. Visual
6. Misc

Sass `$variable`, `@include` and `@extend` directives come before any other declarations for two reasons. First, it keeps your rules clean and easy to read by logically grouping Sass directives together before any additional declarations. Second, placing any `@include` directive(s) at the top of the rule ensures that the declarations below it will be more specific via the CSS cascade.

After Sass directives, positioning comes first because _it can remove an element from the normal flow of the document_ and override box model related styles. The box model comes next as it dictates a component's dimensions and placement.

Everything else takes place inside the component or without impacting the previous two sections, and thus they come last.

```scss
.declaration-order {
    // ----- Sass directives first ----- //
    $local-var: value; // Sass variable (locally scoped)
    @include clearfix; // Sass mixin include
    @extend %foobar;   // Sass placeholder extension

    // ----- Positioning ----- //
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 100;

    // ----- Box-model ----- //
    display: block;
    float: right;
    clear: right;
    width: 100px;
    height: 100px;
    padding: 12px 10px;
    margin: 4px;
    overflow: hidden;

    // ----- Typography ----- //
    font: normal 13px "Helvetica Neue", sans-serif;
    line-height: 1.5
    text-align: center;
    text-indent: 2px;
    letter-spacing: 1px;
    color: #333;

    // ----- Visual / Decorative ----- //
    background-color: #f5f5f5;
    border: 1px solid #e5e5e5;
    border-radius: 3px;
    box-shadow: 0 1px 0 #000;
    outline: none;

    // ----- Misc ----- //
    cursor: pointer;
    opacity: 1;
}
```

> __Complete list of declarations__ can be found [here][csscombconfig]


#### Sort declarations automatically

If you are smart enough to use a superior IDE like Sublime Text, install the "CSScomb" package, and set the Workiva CSScomb config file as your config by copying [the Workiva CSScomb config][csscombconfig] to your `$HOME` directory (`/Users/yourusername/`).




## Naming conventions



### Class naming conventions

Workiva's core CSS platform - [Web Skin][web-skin] - is architected using __[MVCSS][mvcss]__ and strives to follow the __[BEM][bem]__ methodology for naming CSS classes whenever possible. It is recommended that you strive to write CSS classes for your Wdesk app using similar conventions.

> __NOTE:__ We will use the terms _block_, _component_, and _module_ interchangibly throughout this Wiki when referring to the _things_ we build using the Web Skin framework.


#### BEM methodology

__BEM__ stands for _Block_-_Element_-_Modifier_ and is designed to help modularize front-end development by breaking everything into blocks containing elements, then using modifiers to tweak them.

To apply the BEM methodology to CSS, we give each _element_ within a _block_ a class. Consider the Web Skin [list group component][ws-list-group]:

```html
<ul class="list-group">
    <li class="list-group-item">...</li>
    <li class="list-group-item">...</li>
    <li class="list-group-item">...</li>
</ul>
<ul class="list-group list-group-bordered">
    <li class="list-group-item">...</li>
    <li class="list-group-item">...</li>
    <li class="list-group-item">...</li>
</ul>
```

The unordered list has the _block_ class `list-group` while each list item has the _block_-_element_ class `list-group-item`. The bordered variation of our list group component is specified by using the _modifier_ class `list-group-bordered`.


#### Namespace CSS classes

Using [nested selectors within their parent(s)](#sass-nesting) is not a 100% effective at encapsulation and preventing style cross-contamination.

A much better approach is applying namespaces to the classes themselves. If an element is a member of a visual component, every one of its sub-element classes should use the component’s base class name as a namespace.

```scss
// High risk of style cross-contamination
.widget { }
.widget .title { }

// Low risk of style cross-contamination
.widget { }
.widget-title { }
```

Namespacing your classes keeps your components self-contained and modular. It minimizes the likelihood that an existing class will conflict, and it lowers the specificity required to style child elements.


#### Differentiating state from modifier

One area where pure BEM methodology falls short is differentiating _state_ change from a _modifier_. We believe these are two different things.

+ A _state_ class is something that should mutate as a result of user input, device connectivity, etc - and is almost always mutated as a result of JavaScript.
+ A _modifier_ class is used to render a mutation / variation of the standard component or element - and is almost never mutated after the page/view is loaded.

The [SMACSS State Rules][smacss-state] guideline is how we supplement BEM for our needs.


#### State class name prefixes

State classes should always be prefixed with a verb like `is-`, `has-` or `not-`. Consider the Web Skin [list group component][ws-list-group] once more:

```
<ul class="list-group">
    <li class="list-group-item is-active">...</li>
    <li class="list-group-item">...</li>
    <li class="list-group-item">...</li>
</ul>
```

An example where the `has-` prefix is more appropriate is in [form validation][ws-form-validation]:

```
<form>
    <div class="form-group has-error">
        <input type="text" name="input-name" class="form-control" required>
    </div>
</form>

```

> __NOTE:__ Currently, not all Web Skin component states use the aforementioned recommended prefixes. Starting with the `0.5.0` release, all _state_ classes in Web Skin will follow the new `.is-{state}`, `.has-{state}`, `.not-{state}` conventions.



### Sass naming conventions

+ Use meaningful names for everything; use structural or purposeful names over presentational.
+ Provide sensible defaults for parameters when possible

__Bad example:__

```scss
@mixin red-background($tint, $border) {
    background-color: $tint;
    border-color: $border;
}

.form {
  .has-error {
    @include red-background(#ff0000, red);
  }
}
```

__Good example:__

```scss
@mixin input-state-error($bg-color: $state-bg-error, $bd-color: $state-bd-color-error) {
    background-color: $bg-color;
    border-color: $bd-color;
}

.form {
    .has-error {
        @include input-state-error;
    }
}
```



## Specificity

The one and only time it is _ever_ appropriate to use an ID for a CSS selector is if an element occurs __exactly once__ inside a page. __When in doubt, use a class name__.

+ __Good__ candidates for IDs: header, footer, modal dialogs.
+ __Bad__ canddiates for IDs: navigation, reusable components.

When styling a component, remember to following our [class naming conventions](#class-naming-conventions) as much as possible.


### Specificity guidelines

+ Use classes over generic element tag for optimum rendering performance.
+ Do not over-qualify class name selectors with an element type:
  + `.class-name` instead of `div.class-name`.
  + The _rare_ exception is a case where you need to specify exceptions to the default styling of a particular class.
+ If you _must_ use an ID selector (`#selector`) make sure that you have no more than _one_ in your rule declaration.
  + A rule like `#header .search #quicksearch { ... }` is considered harmful.
+ When modifying an existing element for a specific use, always [namespace your classes](#namespace-css-classes).
  + `.listings-layout.listings-bigger` instead of `.listings-layout.bigger`.
  + Think about `ack/grep`ing your code in the future!



## Prefixed CSS3 properties

Since Wdesk apps should be using [Sass][sass] and [Compass][compass], along with the Web Skin Sass API, we have __no need to ever manually write prefixed CSS3 properties.__ Use Web Skin's mixins provided by the Sass API to keep your code <acronym title="Dont Repeat Yourself">DRY</acronym>, and to ensure that we only generate the prefixed properties our apps require based on browser support levels we declare.

> __NOTE:__ Many commonly used CSS3 properties have pre-defined mixins (like `border-radius` and `box-shadow`).  For less commonly used properties, use the `experimental` mixin.

```scss
// Yuck.
.never-do-this {
    -webkit-border-radius: 3px;
       -moz-border-radius: 3px;
         -o-border-radius: 3px;
            border-radius: 3px;
       -webkit-box-shadow: inset 0 1px 0 rgba(0,0,0,.1);
          -moz-box-shadow: inset 0 1px 0 rgba(0,0,0,.1);
            -o-box-shadow: inset 0 1px 0 rgba(0,0,0,.1);
               box-shadow: inset 0 1px 0 rgba(0,0,0,.1);
  -webkit-background-clip: padding-box;
     -moz-background-clip: padding-box;
       -o-background-clip: padding-box;
          background-clip: padding-box;
}

// Nice and DRY.
.do-this-instead {
    @include border-radius(3px);
    @include box-shadow(inset 0 1px 0 rgba(0,0,0,.1));
    @include experimental(background-clip, padding-box);
}
```



## Comments

Code is written and maintained by _people_. Comments help to ensure your code is descriptive and approachable by others.

Great code comments convey context or purpose and should not just reiterate a component or class name.

__Bad example:__

```scss
// Modal header
.modal-header {
    ...
}
```

__Good example:__

```scss
// Wrapping element for .modal-title and .modal-close
.modal-header {
    ...
}
```


### Nesting Comments

Use four levels of comments to demonstrate hierarchy when necessary

__Example:__

```css
// ********************************************************
//
//   First Level
//
// ********************************************************


// ----------------------------------------------
//   Second Level
// ----------------------------------------------


// ----- Third Level ----- //


// Fourth Level
```

> If using a superior IDE like __[SublimeText 3](http://www.sublimetext.com/)__, we highly recommend saving these comment formats as snippets - and assigning them various shortcut keys so its easy to comment your code as you go along.
  + [First Level Comment Snippet][st3-comment-1]
  + [Second Level Comment Snippet][st3-comment-2]
  + [Third Level Comment Snippet][st3-comment-3]
  + [Comment Snippet Key Bindings][st3-comment-bindings]


---


The Workiva CSS code conventions are heavily inspired by [Mark Otto's Code Guide](http://mdo.github.io/code-guide/) and the [Github Styleguide](http://github.com/styleguide).





[web-skin]: /WebFilings/web-skin
[csscombconfig]: /WebFilings/w-coding-conventions/blob/master/css/.csscomb.json
[css-specificity]: /WebFilings/web-skin/wiki/CSS-Specificity
[st3-comment-1]: /WebFilings/web-skin/blob/master/ide-resources/sublime-text/snippets/sass-comment-1.sublime-snippet
[st3-comment-2]: /WebFilings/web-skin/blob/master/ide-resources/sublime-text/snippets/sass-comment-2.sublime-snippet
[st3-comment-3]: /WebFilings/web-skin/blob/master/ide-resources/sublime-text/snippets/sass-comment-3.sublime-snippet
[st3-comment-bindings]: /WebFilings/web-skin/blob/master/ide-resources/sublime-text/key-bindings/sass-comment-shortcuts.sublime-keymap

[ws-list-group]: https://api.webfilings.com/WebSkin/docs/build/html/components/#list-group
[ws-panel]: https://api.webfilings.com/WebSkin/docs/build/html/components/#panels
[ws-form-validation]: https://api.webfilings.com/WebSkin/docs/build/html/css/#forms-validation

[js-conventions]: /w-coding-conventions/blob/master/js/conventions.md
[new-issue]: /WebFilings/w-coding-conventions/issues/new

[atomic-design]: http://bradfrostweb.com/blog/post/atomic-web-design/
[mvcss]: http://mvcss.github.io/
[bem]: http://bem.info/method/
[smacss]: https://smacss.com/
[smacss-state]: https://smacss.com/book/type-state
[css-terms]: http://en.wikipedia.org/wiki/Cascading_Style_Sheets#Syntax
[sass]: http://sass-lang.com
[sass-basics]: http://sass-lang.com/guide
[compass]: http://compass-style.org
