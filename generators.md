
# Generators

## Documentation

- https://www.ecma-international.org/ecma-262/6.0/#sec-generatorfunction
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator/next
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#iterator
- https://davidwalsh.name/es6-generators 
- https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20%26%20beyond/ch3.md#generators
- http://2ality.com/2015/03/es6-generators.html
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield
- http://www.ecma-international.org/ecma-262/6.0/#sec-14.4.14
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of
- https://www.ecma-international.org/ecma-262/6.0/#sec-for-in-and-for-of-statements

## Examples

### Simple generator example
```javascript

var msj = ['hola', '  éste ', ' es', 'mi ', 'mensaje', 1];

/**
 * Iteration and processing using
 * a for loop and a functional approach
 */
for (let text of msj.map(val => val.toString().trim())) {
    console.log(text);
}

/**
 * Iteration and processing using
 * a generator and a for...of loop
 * @see https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of
 */
function* gen (input) {
    for (let val of input) {
        yield val.toString().trim();
    }
}
/**
 * e.g.
 */
for (let text of gen(msj)) {
    console.log(text);
}
```

### Execution order example

```javascript
function* bottlesOfBeer (initialValue) {
    let bob = initialValue;
    let lastMessage = `No more bottles of beer on the wall, no more bottles of beer. 
Go to the store and buy some more, ${bob} bottles of beer on the wall.`;

    while (true) {
        console.log(`${bob} bottles of beer on the wall, ${bob} bottles of beer.`);

        yield bob--;

        console.log(`Take one down and pass it around, ${bob} bottles of beer on the wall.`);

        if (bob < 1) {
            bob = initialValue;
            console.log(lastMessage);
        }
    }
}
```

### Passing a value to the `next` method
```javascript

/**
 * @see https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator/next
 */
function* passingValToNext () {
    let val = 10;

    while (true) {
        console.log(`UP val=${val}`);

        val = yield val + 10; // what happens if no val has been passed to `next()`?

        console.log(`DOWN val=${val}`);
    }
}
```

### Example with initial value and passing value to `next`
```javascript

/**
 * 
 * @param {Number} expectedTotal
 * @returns {Object} Iterator
 */
function* calculateDownloadProgress (expectedTotal) {
    let totalDownloaded = 0;
    let newItems = 0;

    while (true) {
        totalDownloaded += newItems || 0; // lazy verification for the value passed by `next`
        
        let percent = ((totalDownloaded / expectedTotal) * 100).toFixed(2);

        newItems = yield `${percent}%`;
    }
}

/**
 * e.g.
 */
let a = calculateDownloadProgress(100);
console.log(a.next());
console.log(a.next(12));
console.log(a.next(12.3));
console.log(a.next());
console.log(a.next(1));
```

### Example of  `yield* syntax` for adding a custom iterator to an object
```javascript
/**
 * @see https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield
 * @see http://www.ecma-international.org/ecma-262/6.0/#sec-14.4.14
 */
let bla = {
    [Symbol.iterator]: function* () {
        yield* 'Each letter will be yield separately';
    }
}

/**
 * e.g.
 */
console.log([...bla]);
```