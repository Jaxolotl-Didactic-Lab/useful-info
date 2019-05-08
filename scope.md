# Scope
> **REMINDER**  
> DO NOT CONFUSE "scope" with "context"

---------------------------

## DEFINE SCOPE
## Scope is the **"part of the program"** where a [Name Binding](https://en.wikipedia.org/wiki/Name_binding) is valid. 

---------------------------

## "Part of a program" meaning


### WHEN defined at **RUN-TIME** ( Dynamic Scope )
> "Dynamic scoping does not care how the code is written, but instead how it executes. Each time a new function is executed, a new scope is pushed onto the stack. This scope is typically stored with the function’s call stack. When a variable is referenced in the function, the scope in each call stack is checked to see if it provides the value." [Lua](https://leafo.net/guides/dynamic-scoping-in-lua.html)

Late binding, non-optimizable at compile time

### WHEN defined at **LEX-TIME** ( Lexical Scope )
> "The scope of a quantity is the set of statements and expressions in which the declaration of the identifier associated with that quantity is valid." [Algol60](https://www.masswerk.at/algol60/report.htm#2_7)

Early binding, optimizable at compile time  
JavaScript use this scope paradigm

### ECMAScript definitions
- [Lexical Environment](http://www.ecma-international.org/ecma-262/6.0/#sec-lexical-environments)

### General definitions:
- https://developer.mozilla.org/en-US/docs/Glossary/Scope
- https://en.wikipedia.org/wiki/Scope_(computer_science)

### Relative concepts
- [Name Binding](https://en.wikipedia.org/wiki/Name_binding)
- [Identifier](https://en.wikipedia.org/wiki/Identifier)
- [Local Scoped Variable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
- [Global Scoped Variable](https://developer.mozilla.org/en-US/docs/Glossary/Global_object)
- [Block Scoped Variable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)



# Closure

> "A closure is the combination of a function and the lexical environment within which that function was declared." [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

> "In programming languages, a closure, also lexical closure or function closure, is a technique for implementing lexically scoped name binding in a language with first-class functions. Operationally, a closure is a record storing a function together with an environment.The environment is a mapping associating each free variable of the function (variables that are used locally, but defined in an enclosing scope) with the value or reference to which the name was bound when the closure was created. Unlike a plain function, a closure allows the function to access those captured variables through the closure's copies of their values or references, even when the function is invoked outside their scope." [Wikipedia](https://en.wikipedia.org/wiki/Closure_(computer_programming))

> "In computer science, a closure (also lexical closure, function closure, function value or functional value) is a function together with a referencing environment for the non-local variables of that function. A closure allows a function to access variables outside its typical scope. Such a function is said to be "closed over" its free variables. The referencing environment binds the nonlocal names to the corresponding variables in scope at the time the closure is created, additionally extending their lifetime to at least as long as the lifetime of the closure itself. When the closure is entered at a later time, possibly from a different scope, the function is executed with its non-local variables referring to the ones captured by the closure." [Enacademic](https://enacademic.com/dic.nsf/enwiki/39434)


### General Definition
- https://en.wikipedia.org/wiki/Closure_(computer_programming)

### Relative Concepts
- [First Class Citizen](https://en.wikipedia.org/wiki/First-class_citizen)
- [Higher order function](https://en.wikipedia.org/wiki/Higher-order_function)
- [Garbage collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science))
- [Free variables and bound variables](https://en.wikipedia.org/wiki/Free_variables_and_bound_variables)
- [Domain of discourse](https://en.wikipedia.org/wiki/Domain_of_discourse)
- [Referential transparency](https://en.wikipedia.org/wiki/Referential_transparency)
- [Side effect](https://en.wikipedia.org/wiki/Side_effect_(computer_science))
- [Function pointer](https://en.wikipedia.org/wiki/Function_pointer)



# Strict Mode
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode/Transitioning_to_strict_mode
- https://humanwhocodes.com/blog/2016/10/the-ecmascript-2016-change-you-probably-dont-know/

## What happens on strict mode?

### TL;DR
1. Eliminates some JavaScript `silent errors` by changing them `to throw errors`.
2. Fixes mistakes that make it difficult for JavaScript engines to perform optimizations: strict mode code can sometimes be made to run faster than identical code that's not strict mode.
3. Prohibits some syntax likely to be defined in future versions of ECMAScript.

### More details:

Semantic differences
- this resolution won't propagate to the global scope, thus for a strict mode function, the specified this is not boxed into an object, and if unspecified, this will be `undefined`
- arguments doesn't alias named function arguments
- `eval` doesn't create a new variable in the scope from which it was called, `eval` of strict mode code does not introduce new variables into the surrounding scope.

When adding `'use strict';` the following cases will throw an Error:
- SyntaxError
    - Octal syntax `var n = 023;`
    - `with` statement
    - Using delete on a variable name `delete myVariable`;
    - Using `eval` or `arguments` as variable or function argument name
    - Using one of the newly reserved keywords (in prevision for ECMAScript 2015): 
        - `implements`,
        - `interface`,
        - `let`,
        - `package`,
        - `private`,
        - `protected`,
        - `public`,
        - `static`,
        - and `yield`
    - Escape characters are not allowed `var y = \010;`
    - Declaring function in blocks `if (a < b) { function f() {} }`
    - Obvious errors
        - Declaring twice the same name for a property name in an object literal `{a: 1, b: 3, a: 7}` This is no longer the case in ECMAScript 2015 (bug 1041128).
        - Declaring two function parameters with the same name function `f(a, b, b) {}`
- TypeError
    - Writing to a get-only property is not allowed
    - Writing to a read-only property is not allowed
    - Deleting an undeletable property is not allowed `delete Object.prototype`
    - Setting properties on primitive values `false.true = '';` , `(14).sailing = 'home';` , `'with'.you = 'far away';` 
- Runtime errors
    - Setting a value to an undeclared variable
    - Trying to delete a non-configurable property
    - Poisoned arguments and function properties, e.g. accessing `arguments.callee`, `arguments.caller`, `anyFunction.caller`, or `anyFunction.arguments`
- ReferenceError
    - Using a variable, without declaring it



# Examples

```javascript
/**
 * REMEMBER
 * In JavaScript a variable can't have a reference to another variable!!!! IT'S ALL ABOUT VALUES!!!!
 * 
 * A value can be assigned to a variable in two ways
 *      BY VALUE -> for primitives -> the data is duplicated and then the variable points to that new duplicated data
 *          var a = 2;
 *          var b = a; -> a new numeric data is created and b will point tho this new data
 *                        there's no relationship between the 2 of a and the 2 of b
 * 
 *      BY REFERENCE -> for compound (objects) -> the variable points the the data
 *          var a = {x: 3};
 *          var b = a; -> there's no reference from b to a,
 *                        the references if from that both a and b point to the same object in memory { x:3 }
 * 
 *                        It means if we modify the actual object { x:3 } by adding or removing information, all identifiers will
 *                          be still linked to the same data which has changed. Instead of we reassign the identifier data or redefine the identifier, 
 *                          the data will be accessible through any remining identifier pointing to it otherwise it'll be garbage collected
 *
 * Below a simple verification
 */

// Synchronous modification 

var a = { x:3 };
var b = a;
a = { n:8 };

console.log(a); // -> {n: 8}
console.log(b); // -> {x: 3}

// Asynchronous modification 
var c = { x:3 };
var d = c;
setTimeout(() => c = { x:4 }, 1000);
setTimeout(() => console.log(c), 2000); // -> {x: 4}
setTimeout(() => console.log(d), 3000); // -> {x: 3}

```

```javascript

/**
 * How many LHS look-us can you see?
 * How many RHS look-us can you see?
 */

function speakToMe(input) {
    var helperVal = input;
    return input + helperVal;

}

var result = speakToMe( 10 );

```


```javascript
/**
 * explain scope of:
 *  a
 *  b
 *  c
 *  d
 * 
 *  what is TDZ in ES6?
 */
function n ( z ) {
    a = 3; 
    var b = 4;

    {
        var c = 5;
        let d = 7;
    }
}

```


```javascript
/**
 * 
 */
(function(){
  var a = b = 3;
})();

console.log(a); // what will it print?
console.log(b); // and this?

```

```javascript
/**
 * CLOSURE example from wikipedia
 * @see https://en.wikipedia.org/wiki/Closure_(computer_programming)#Lexical_environment
 * 
 * in ECMAScript
 */

var f, g;

function foo() {
  var x;
  f = function() { return ++x; };
  g = function() { return --x; };
  x = 1;
  alert('inside foo, call to f(): ' + f());
}

foo();  // 2
alert('call to g(): ' + g());  // 1 (--x)
alert('call to g(): ' + g());  // 0 (--x)
alert('call to f(): ' + f());  // 1 (++x)
alert('call to f(): ' + f());  // 2 (++x)

/**
 * Function foo and the closures referred to by variables f and g all use the same relative memory location signified by local variable x.
 * 
 * In some instances the above behaviour may be undesirable, and it is necessary to bind a different lexical closure. 
 * Again in ECMAScript, this would be done using the Function.bind().
 */

```

## Abandon all hope who enter here

### Ev[a|i]l
Modifying the lex-time defined scope dynamically 

```javascript

/**
 * @see https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval
 * 
 */

function cheat(a, b, c) {
    // 'use strict'; // <- what if we use strict mode?
    eval(a);
    console.log(b + c);
}

cheat('var c = 7', 10, 5); // <- what if we omit the var statement?
```

```javascript

function cheatAgain(a, b) {
    // 'use strict'; // <- what if we use strict mode?
    eval(a);
    console.log(b + c);
}

var c = 5;

cheatAgain('var c = 7', 10); // <- what if we omit the var statement?

```

### with 
Creating a lexical scope at run-time

```javascript

/**
 * 
 * @see https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/with
 * 
 */

var a = {
    b: 2
};

var a1 = {
    c: 3
}

with(a){
   b = 3;
}

with(a1){
   b = 4;
}

console.log(a);
console.log(a1); // <- oops
console.log(b); // <- what?!!!


```
```javascript
'use strict';
with (Math){x = cos(2)};
```

# Other
- A great analysis of a memory leak example related to inacurate use of closures [See here](http://point.davidglasser.net/2013/06/27/surprising-javascript-memory-leak.html)