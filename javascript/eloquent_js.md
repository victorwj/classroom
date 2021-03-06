# JavaScript

JavaScript is an indispensable language for web programming. 
Learned for research work at [Princeton CITP](https://citp.princeton.edu/).

## Sources
- [Eloquent JavaScript 3rd Edition](https://eloquentjavascript.net/)
- [JavaScript: The Good Parts](https://github.com/NorthPaulo/research/blob/master/Frontend-books%26research/JavaScript%20-%20The%20Good%20Parts%20-%20Douglas%20Crockford%20-%20May%202008.pdf)
- Reviewed: _The Good Parts_ for a quick language overview, then _Eloquent JavaScript_ Ch.12+ for more on the browser and DOM environment

## Notes
- JavaScript is loosely typed; it never casts
- "What matters about an object is what it can do, not what it is descended from"
- JavaScript is a _prototypal_ language; objects inherit directly from other objects (not classes)
- "The pseudoclassical form [of JavaScript] can provide comfort to programmers who are unfamiliar with JavaScript, but it also hides the true nature of the language. Much of the complexity of class hierarchies is motivated by the constraints of static type checking. JavaScript is completely free of those constraints. In classical languages, class inheritance is the only form of code reuse. JavaScript has more and better options."
- "JavaScript's loose typing and excessive error tolerance provide little compile-time assurance of our program's quality, so to compensate, we should code with strict discipline."


### Objects
- Numbers (everything in JS is float64), strings, booleans, null, undefined are simple types
- Everything else are _objects_
- Objects can simply be made using {}, pretty much JSON
- Objects can be accessed using [] or .
- Objects are always passed by reference
- All objects are linked to a _prototype object_ which it can inherit properties from
- Objects produced using object literals are linked to `Object.prototype`
- Function objects are linked to `Function.prototype` which is linked to `Object.prototype`
- Javascript _does not_ have block scope, unlike C-style languages
- It only has function scope
- Best style is therefore to declare variables at the top, as opposed to right-before-usage, to avoid unintentional variable hoisting

### Functions
- Crockford: "The best thing about JavaScript is its implementation of functions"
- Functions in JS are objects
- Creating functions:

``` javascript
var add = function (a, b) {
    return a + b;
};

```
- A function literal can appear anywhere an expression can appear; so it can be inside other functions (and has access to the outer params/vars), called _closure_
- Every function receives `this` and `arguments` as additional parameters
- `this` is determined by the invocation pattern: method, function, constructor, or apply invocations

### Invocations
- Invocation is a pair of parentheses that follow any expression that produces a function value
    - There is no type checking to the given arguments
    - Too many arguments will be ignored, too few will result in `undefined`
- Method invocation:
    - Function stored as a property of an object
    - When invoked, the function is bound to that object
    - Methods can use `this` to access/modify the object
- Function invocation: when the function is not the property of an object
    - `this` is bound to the global object
    - Error in language design; it should be bound to the outer function `this`
    - Can be worked around, by convention a variable called `that` set to `this`
- Constructor invocation:
    - JS is a _prototypal_ inheritance language; objects can inherit properties directly from other objects, and the language is class-free
    - Invoke with a `new` prefix; but this is not recommended

``` javascript
var Quo = function (string) { // Capitalize first letter for constructors
    this.status = string; // "this" is special for constructors
}

Quo.prototype.get_status = function () { return this.status; };
var myQuo = new Quo("hello")
document.writeln(myQuo.get_status()); // "hello"
```
- Apply invocation:
    - JS is a _functional_ object-oriented language; functions can have methods
    - `apply` is a method to call a function with a given `this` value 
    - First argument of `apply` is the `this`, the second argument is an array of arguments to call the function with
- `arguments` is a bonus parameter available to functions, and it is an array that contains _all_ arguments including those in excess
    - `arguments` is array-like but does not contain any array methods
- The `exception` object contains a `name` and a `message`; other properties can be added

### Prototypes
- Start by making a useful object; then make many more objects that are like that one 

``` javascript
var myMammal = { ... };
var myCat = Object.create(myMammal);
// Customize new instance
myCat.saying = "meow";
```
- _Differential inheritance_: customizing a new object is specifying the differences from the object on which it is based
- One weakness is that we get no privacy; all properties of an object are visible
- Functional pattern:
    - Create a new object
    - Optionally defines private variables and methods; they are just ordinary vars of the function
    - Augment new object with methods; these methods will have privileged access to vars in step 2
    - Return the new object

``` javascript
var constructor = function (spec, my) {
    var that, other private variables;
    my = my || {}
    // Add shared variables and functions to my
    that = a new object;
    // Add privileged methods to that
    return that;
};
```

- `spec` contains all information to make an instance, `my` is a container shared by constructors in the inheritance chain (optional)

### Arrays
- Arrays in JavaScript aren't like arrays in other languages; they are objects with array-like properties
- `arr[1]` is converted to `arr["1"]`, like the way to access an object; significantly slower than real arrays
- Create an array using `var arr = [];`
- `arr.length` returns length, but it is not an upper bound; there is no out-of-index, and it doesn't allocate more space for the array
- `delete` can be used to remove elements from the array, leaving an undefined hole; don't use it
- Object vs. array rule: if property names are sequential integers, then use an array



### The Good Parts
- JavaScript makes it easy to define global variables, but globals increase the chance of bad interactions and reduces readability
- To reduce globals, use global abatement:

``` javascript
// Limit global footprint to a single name
var MYAPP = {};
MYAPP.flight = {
    airline: "Oceanic",
    number: 815,
};
```

- Closures are also a good way to reduce globals:

``` javascript
// Invoke function, store result in myObject
var myObject = (function() {
    var value = 0;
    // Return an object, working with a value that is scoped off from others
    return {
        increment: function (inc) {
            value += typeof inc === "number" ? inc : 1;
        },
        getValue: function () {
            return value;
        }
    };
}());
```
- Remember that closures have read/write access to actual variables, no copies
- When a function has a lot of arguments, use object specifiers:

``` javascript
// Don't do this
var myObject = myFunction(f, l, m, c, s);
// Instead do this
var myObject = myFunction({
    first: f,
    last: l,
    middle: m,
    state: s,
    city: c
});
// Arguments can be in different order, more clear, less errors
```

### Beautiful Features

### The Awful Parts
- Be careful of implied global declarations: `foo = value` is a global variable
- No block scope; declare all your variables at the top of the function
- Semicolon insertion; JavaScript tries to correct programs unreliably
- `NaN` results from converting bad string to a number; `NaN === NaN` is false

### The Bad Parts
- Don't use `==` or `!=`; Always use `===` or `!==`
- Don't use `with`
- Don't use bit operators (unless you have to); they're very slow
- Don't use `eval()`; it's slow and unclear
- Don't use `continue`; bad coding practice
- Don't use `new`
- Don't use `void`; it's confusingly an operator that takes and operand and returns undefined
- Don't create functions in a loop
