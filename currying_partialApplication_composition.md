# Function/code factorization through _Currying_, _Partial application_ or _First-class composition_


## Currying

> In mathematics and computer science, currying is the technique of translating the evaluation of a function that takes multiple arguments into evaluating a sequence of functions, each with a single argument. For example, a function that takes two arguments, one from X and one from Y, and produces outputs in Z, by currying is translated into a function that takes a single argument from X and produces as outputs functions from Y to Z. Currying is related to, but not the same as, partial application. [Wikipedia](https://en.wikipedia.org/wiki/Currying)

```javascript
/**
 * No curry for you
 *
 * @param {number} x
 * @param {number} y
 * @returns {number}
 */
function add(x, y){
    return x + y;
}
add(3, 4); // > 7


/**
 * Add some spicy curry
 *
 * @param {number} x
 * @returns {function}
 */
function add_curry(x) { // :P
  return function(y) {
   return x + y;
  }
}
add_curry(3)(4); // 7

```

## Partial Application

> In computer science, partial application (or partial function application) refers to the process of fixing a number of arguments to a function, producing another function of smaller arity. [Wikipedia](https://en.wikipedia.org/wiki/Partial_application)

Difference from Currying
> One of the significant differences between the two is that a call to a partially applied function returns the result right away, not another function down the currying chain. [Wikipedia](https://en.wikipedia.org/wiki/Currying#Contrast_with_partial_function_application)

```javascript
/**
 * No partial
 *
 * @param {number} x
 * @param {number} y
 * @returns {number}
 */
function add(x, y){
    return x + y;
}
add(3, 4); // > 7

/**
 * Partial
 * @see https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#Partially_applied_functions
 *
 * @param {number} y
 * @returns {number}
 */
var add_partial = add.bind(null, 3);

add_partial(4); // -> 7

```

```javascript

/**
 * Using multilevel binary partially applied functions to retunr an unary
 * (f ∘ g )(v)
 *
 * @param  {...function} args
 * @returns {Function}
 */
function multiLevelPartialRight (...args) {
    let cfn = (v) => v;

    while (args.length) {
        cfn = args.pop().bind(null, cfn)
    }

    return cfn
}

multiLevelPartialRight(
    /* f */(fn, v) => fn(v) + 1,
    /* g */(fn, v) => fn(v) * 2
)(2) // >  (f ∘ g )(v) = 5

```

## First-class composition

> In computer science, function composition is an act or mechanism to **combine** simple functions to build more complicated ones. Like the usual composition of functions in mathematics, **the result of each function is passed as the argument of the next, and the result of the last one is the result of the whole**. [Wikipedia](https://en.wikipedia.org/wiki/Function_composition_(computer_science))

Simple usecase
> If an airplane's elevation at time `t` is given by the function `h(t)`, and the oxygen concentration at elevation `x` is given by the function `c(x)`, then `(c ∘ h)(t)` describes the oxygen concentration around the plane at time `t`. [Wikipedia](https://en.wikipedia.org/wiki/Function_composition#Examples)

A well known usecase is a mapper or transducer
> From a given response data, remove duplicates, normalize numbers, sort by N.... and so on

Notes:

- the flow is right-to-left, calling each function with the output of the latest provided first.
- JavaScript doesn't have a `composition` operator like Haskell
- JavaScript have Lambda expressions!!

```javascript

/**
 * simplest imperative nested solution
 * n(x) = f(g(x))
 *
 * Notation:
 * ( f ∘ g )(x) = f(g(x) // f ∘ g means f after g
 *
 * Alternative postfix notation:
 * (xg)f
 * @see https://en.wikipedia.org/wiki/Reverse_Polish_notation
 *
 * @param {Function} f
 * @param {Function} g
 */
function fog (f, g) { return (x) => f(g(x)) };

fog(
    /* f */  v => v + 1,
    /* g */ v => v * 2
)(2) // >  (f ∘ g )(v) = 5

```

```javascript

/**
 * Iterative solution
 * (f ∘ g )(v)
 *
 * @param  {...function} args
 * @returns {Function}
 */
function composeSequenceRight (...args) {

    return (v) => {
        while (args.length) {
            v = args.pop()(v)
        }
        return v;
    }

}

composeSequenceRight(
    /* f */  v => v + 1,
    /* g */ v => v * 2
)(2) // >  (f ∘ g )(v) = 5

```

```javascript

/**
 * Iterative oneliner functional style solution
 * (f ∘ g )(v)
 * @see https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/ReduceRight
 *
 * @param  {...function} args
 * @returns {Function}
 */
const composeRight = (...args) => (value) => args.reduceRight((acc, fn) => fn(acc), value)

composeRight(
    /* f */  v => v + 1,
    /* g */ v => v * 2
)(2) // >  (f ∘ g )(v) = 5

```

## Interesting readings:

- [Composing Exercises in Programming Style](https://blog.frankel.ch/exercises-programming-style/6/)
- [Partially Applied Functions In JavaScript](https://lostechies.com/derickbailey/2012/07/20/partially-applied-functions-in-javascript/)
- [JavaScript Function Composition, Currying, and Partial Application](https://medium.com/wdstack/javascript-function-composition-currying-and-partial-application-5a04107530ee)
- [Curry or Partial Application?](https://medium.com/javascript-scene/curry-or-partial-application-8150044c78b8)
- [Creating partially applied functions in Javascript](https://codeburst.io/creating-partially-applied-functions-in-javascript-1f623a56d055)
