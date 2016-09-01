# JS Conventions

* [General Syntax](#general-syntax)
* [.editorconfig](#editorconfig)
* [Use Strict](#use-strict)
* [Naming](#naming)
* [Switch Blocks](#switch-blocks)
* [Named Functions](#named-functions)
* [Don't Abuse Bind](#dont-abuse-bind)
* [Comments](#comments)
* [Modules](#modules)
    * [CommonJS Module Syntax](#commonjs-module-syntax)
* [Unit Testing](#unit-testing)
* [Scope in JavaScript](#scope-in-javascript)
    * [The `this` Object](#the-this-object)
    * [Usage of `self`/`that`](#usage-of-self--that)
    * [Understanding `this`](#understanding-this)
* [Linting](#linting)
* [Unused Parameters](#unused-parameters)
* [Curly Braces](#curly-braces)
* [Indentation](#indentation)
* [String Literals](#string-literals)
* [Exceptions](#exceptions)
* [Common Mistakes](#common-mistakes)

## General Syntax

* Use soft-tabs with 4 spaces.
* Put single quotes around strings.
* Always declare variables using `var`.
* Always use semicolons.
* Do not put spaces before commas or semi-colons.
* Put a space after colons and commas.
* Put spaces before and after operators.
* Logical blocks should be separated by a blank line.
* Use same value and type comparisons (===, !==).
* Put opening braces on the same line as the function or block.
* Put a single space after `for`, `while`, `if`, and `switch` keywords.
* Put a single space after the final parenthesis in `for`, `while`, `if`, and `switch` statements.
* Be very careful not to have any dangling commas in array and object initialization.

### Incorrect example

```js
items=["a",'b' ,'c',]

for(var i=0;i<items.length; i++){
  var item = items[i];
  alert(item)
}

function yelp(str)
{
  switch(str) {
  case 'a':
    alert('a');
    break;
  case 'b':
    alert('b');
    break;
  }
}

yelp("c");

if ('a' != 'b')
{
  alert('a');
}
else
{
  alert("b");
}
```

### Correct example

```js
var items = ['a', 'b', 'c'];

for (var i = 0; i < items.length; i++) {
    var item = items[i];
    alert(item);
}

function yelp(str) {
    switch (str) {
        case 'a':
            alert('a');
            break;

        case 'b':
            alert('b');
            break;

        default:
            break;
    }
}

yelp('c');

if ('a' !== 'b') {
    alert('a');
} else {
    alert('b');
}
```

## .editorconfig

To ensure your pull request doesn't contain any unnecessary white-space or formatting changes, __utilize the editor preferences__ provided in the [editor config](.editorconfig) - compatible with common text editors simply by downloading the plugin for your editor at [http://editorconfig.org](http://editorconfig.org).

## Use Strict

Always declare `'use strict';`. [See advantages](http://www.nczonline.net/blog/2012/03/13/its-time-to-start-using-javascript-strict-mode/).  Please note that the **script-level** declaration can cause issues when concatenating strict and non-strict scripts.  Consistent application of `'use strict';` is important to avoid these issues.

### Script-level declaration

```js
'use strict';           // must precede any other statements in the script,
                        // strict-mode applies to the entire script

function example() {
    // code here
}
```

### Function-level declaration

```js
function example() {
    'use strict';       // must precede any other statements in the function body,
                        // strict-mode applies only within this functional closure

    // code here
}
```

## Naming

* Always use `camelCase` for vars, parameters, and functions, never underscores.
* Objects that you instantiate multiple times should have names that start with a capital letter, otherwise the name should start with a lowercase letter.
* Use all uppercase with underscores for spaces for constants. Do not use the `const` keyword.

### Incorrect

```js
function coordinate_pair(x, y, creator) {
    this.x = x;
    this.y = y;
    this.creator = creator;
}

function CoordinateService() {
    const creator = 'coord-service';
    function create_coordinate(X, Y) {
        return new coordinate_pair(X, Y, creator);
    }
    return {
        'Create_coordinate': Create_coordinate
    };
}
```

### Correct

```js
// CoordinatePair will be instantiated multiple times with the `new` keyword.
function CoordinatePair(x, y, creator) {
    this.x = x;
    this.y = y;
    this.creator = creator;
}

function coordinateService() {
    var CREATOR = 'coord-service';
    function createCoordinate(x, y) {
        return new CoordinatePair(x, y, CREATOR);
    }
    return {
        'createCoordinate': createCoordinate
    };
}
```

## Switch Blocks

* Indent within case blocks AND within switch blocks.
* Always have a default case on switch statements.
* Separate case blocks with a blank line.

### Incorrect

```js
function speak(type) {
    switch (type) {
    case 'dog':
        alert('woof');
        break;
    case 'cat':
        alert('meow');
        break;
    case 'developer':
        break;
    }
}
```

### Correct

```js
function speak(type) {
    switch (type) {
        case 'dog':
            alert('woof');
            break;

        case 'cat':
            alert('meow');
            break;

        case 'developer':
            break;

        default:
            alert('hello');
    }
}
```

## Named Functions

Always use named functions in favor of anonymous functions. If an error is thrown, named functions will provide more helpful stack traces.

### Incorrect

```js
var getFullName = function(firstName, lastName) {
    return firstName + ' ' + lastName;
};

somethingThat.takesACallback(function() {
    alert('success!');
});
```

### Correct

```js
function getFullName(firstName, lastName) {
    return firstName + ' ' + lastName;
}

somethingThat.takesACallback(function aCallback() {
    alert('success!');
});
```

## Don't Abuse Bind


When invoking a function in a particular context it is better to use:

```js
function.apply(context)
```
... versus ...

```js
function.bind(context)()
```

... as it is one function invocation versus two. Also, for performance-critical areas, it is
faster to create a new function closing around `this` than to use bind, e.g:

```js
function boundCall() {
    this.doSomething();
}
```

It is generally better to wrap a function that closes over the scope than to use
`bind`. For example:

```js
var self = this;
someRegistrar.register(function registrationCallback() {
    console.log(self);
});
```

## Comments

To call out different sections of a module use

```js
/************************************************************************
 * MINOR SECTION
 ************************************************************************/

/************************************************************************
 *
 * MAJOR SECTION
 *
 ************************************************************************/
```

## Modules

Require module filenames should match the name that would be exported by the module. If you have a module that exports `SomeThing`, the filename should be `SomeThing.js`.

```js
define(function(require) {
    'use strict';

    var SomeThing = require('package/package/SomeThing');
});
```
Example with multiple entries:

```js
define(function(require) {
    'use strict';

    var ClassA = require('path/one/classA');
    var ClassB = require('path/two/ClassB');
});
```

Example without entries:

```js
define(function(/* require */) {
    'use strict';
});
```

Require other JS modules using a relative path and a `.js` file extension.

### Incorrect

```js
// File: components/faux/fauxController.js

// Requiring file: components/faux/fauxChannel.js
var fauxChannel = require('components/faux/fauxChannel');
```

### Correct

```js
// File: components/faux/fauxController.js

// Requiring file: components/faux/fauxChannel.js
var fauxChannel = require('./fauxChannel.js');
```

### CommonJS Module Syntax

```js
function greetingService() {
    'use strict';

    function sayHello() {
        alert('Hello!');
    }

    return {
        'sayHello': sayHello
    };
}
module.exports = greetingService;
```

## Unit Testing

Use nested describe statements.

Example:

```js
define(function(require) {
    'use strict';

    var DocumentGenerator = require('DocumentGenerator');

    describe('DocumentGenerator', function() {
        describe('feature a', function() {
            it('should work', function() {
                expect(true).toBeTruthy();
            }
        }
    }
});
```

## Scope in JavaScript

### The `this` Object

When establishing a scoped variable for an OO call where `this` is the target of
a method invocation, use `self` for example:

```js
var self = this;
```

For other cases use ‘that’

```js
var that = this;
```

### Usage of `self` / `that`

The rule of thumb is that you use self when there is a function inside of a
function. Therefore you would be taking advantage of closure.

### Understanding `this`

The `this` keyword refers to the scope of the current function. As such, it is
dynamically scoped and its value can be different depending on how the function
was invoked. There are two ways the scope (`this`) can change.

1) Implicit scope change

```js
var myObject = new MyClass();

/* In this invocation, `this` inside the body of `myFunction` will refer to the
instance of `MyClass`. */
myObject.myFunction();

/* In this invocation, despite the fact that the same code is running, `this`
will actually refer to the `window` object (assuming we are at the top level of
the program). */
var myFunction = myObject.myFunction;
myFunction();
```

2) Explicit scope change

```js
/* Both `call` and `apply` take a scope parameter which will be assigned to
`this` in the function. A function can also be permanently bound to a scope
(value of `this`) using the `bind` method. */

// `this` will be `window`
myFunction();
myFunction.apply(null, []);
myFunction.call(null);

// `this` will be `someObject`
myFunction.apply(someObject, []);
myFunction.call(someObject);

// `this` will ALWAYS be `someObject` after a `bind`
myFunction = myFunction.bind(someObject);
myFunction();
myFunction.apply(window, []); // still `someObject`
myFunction.call(window); // still `someObject`
myFunction = myFunction.bind(window);
myFunction(); // still `someObject`
```

See http://jsbin.com/kobas/latest/edit?js,console for some additional examples.

## Linting

Several teams have had success with JSHint as a compile time linting tool.

Rules of thumb: there are cases where the general strict ruleset for linting will not be appropriate for a piece of code you’re writing.

Relaxing linting rules should be done with care and sparingly. The relaxation directive should be as specific as possible and be as close to the offending code as possible.

## Unused Parameters

Oftentimes, especially with event handlers, you may have a function callee in
which the parameters go unused.

JSHint will pick up on those as unused variables which it will flag.

You may want to retain the indication that a parameter *could* be passed. In
this case, add the parameters with a jsDoc string, indicating their optional status:

```js
/**
 * @param element {object} A required element object
 * @param [event] {object} An optional event object
 */
function myHandlerDoesNotUseEvent(element) {};
```

It is also reasonable in some cases to use an inline comment to indicate that
a function will be passed a parameter but that it is currently unused:

```js
function myHandlerDoesNotUseEvent(/* element */) {};
```

This allows the general linting improvement to run, and still retain semantic
information about the caller.

## Curly Braces

Opening braces go on the same line as the function or control flow statement.
For example:

```js
if (boolean) {
    // logic
} else {
    // logic
}

function foo(bar) {
    return {
        a: b,
        foo: bar
    };
}
```

## Indentation

Use spaces, not tabs. Four spaces per tab. This looks better in debug
environments such as Chrome.

## String Literals

Single quotes (`'`) should be favored over double quotes (`"`). This makes for
less visual clutter and consistency in this regard makes searching for string
literals in the code easier.

## Exceptions

The following pattern is preferred for thrown exceptions. Promises work better
with actual `Error` objects than with throw strings (a common pattern in JS
code).

```js
throw new Error('something awful happened');
```

## Common Mistakes

### Instance Variables of 'Classes'

Instance variables should be initialized in the constructor function and NOT in
the prototype.
