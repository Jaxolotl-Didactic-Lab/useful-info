# Execution Context

> See [Execution Context](http://www.ecma-international.org/ecma-262/6.0/#sec-execution-contexts) documentation

> 👎 Execution context != context (aka `this`)  
> 👍 Execution context ~ scope

## Types of Execution Context _(executable code)_

> See [Types of Source Code](http://www.ecma-international.org/ecma-262/6.0/#sec-types-of-source-code)

- [Global code](http://www.ecma-international.org/ecma-262/6.0/#sec-global-environment-records)
- [Function code](http://www.ecma-international.org/ecma-262/6.0/#sec-function-environment-records)
- [eval code](http://www.ecma-international.org/ecma-262/6.0/#sec-eval-x)

## Execution Stack

- Starts with global
- Usually LIFO but not necessarily
    > Evaluation of code by the running execution context may be suspended at various points defined within this specification. Once the running execution context has been suspended a different execution context may become the running execution context and commence evaluating its code. At some later time a suspended execution context may again become the running execution context and continue evaluating its code at the point where it had previously been suspended. Transition of the running execution context status among execution contexts usually occurs in stack-like last-in/first-out manner. However, some ECMAScript features (ES6) require **non-LIFO transitions of the running execution context**.

    Anyone in the audience wants to bring an example of non-LIFO activation record transition feature?

## How Execution Context is defined?

### Creation Phase

- [LexicalEnvironment](http://www.ecma-international.org/ecma-262/6.0/#sec-lexical-environments) component creation.  
  > Identifies the Lexical Environment used to resolve identifier references made by code within this execution context.

  - [Environment Record](http://www.ecma-international.org/ecma-262/6.0/#sec-environment-records)
    - [Declarative Environment Records](http://www.ecma-international.org/ecma-262/6.0/#sec-declarative-environment-records)  

        > Each declarative Environment Record is associated with an ECMAScript program scope containing variable, constant, let, class, module, import, and/or function declarations. A declarative Environment Record binds the set of identifiers defined by the declarations contained within its scope.

      - `Variable` declaration
        - `let`, `const` [let and const declaration](http://www.ecma-international.org/ecma-262/6.0/#sec-let-and-const-declarations)
      - `Function` declaration
        - `arguments`
    - [Object Environment Records](http://www.ecma-international.org/ecma-262/6.0/#sec-object-environment-records)  

        > Each object Environment Record is associated with an object called its binding object. An object Environment Record binds the set of string identifier names that directly correspond to the property names of its binding object. Property keys that are not strings in the form of an IdentifierName are not included in the set of bound identifiers. Both own and inherited properties are included in the set regardless of the setting of their `[[Enumerable]]` attribute. Because properties can be dynamically added and deleted from objects, the set of identifiers bound by an object Environment Record may potentially change as a side-effect of any operation that adds or deletes properties. Any bindings that are created as a result of such a side-effect are considered to be a mutable binding even if the Writable attribute of the corresponding property has the value false. Immutable bindings do not exist for object Environment Records.  

  - Reference to the outer environment,
  - `this` binding.  

    > The value associated with the `this` keyword within ECMAScript code associated with `this` execution context.

- `VariableEnvironment` component creation.  

    > Identifies the Lexical Environment whose environment record holds bindings created by VariableStatements and FunctionDeclarations within this execution context.  
    > `var`  [var declaration](http://www.ecma-international.org/ecma-262/6.0/#sec-variable-statement)

### Execution Phase

> example taken from:
> [Understanding Execution Context and Execution Stack in Javascript. (by Sukhjinder Arora)](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0)

### 0 - The code

```javascript
let a = 20;

const b = 30;

var c;

function multiply(e, f) {
 var g = 20;
 return e * f * g;
}

c = multiply(20, 30);

```

### 1 - Global context creation 

```javascript
// (PSEUDO-CODE)

GlobalExectionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // Identifier bindings go here
      a: < uninitialized >,
      b: < uninitialized >,
      multiply: < func >
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },
  VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // Identifier bindings go here
      c: undefined,
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```

### 2 - Global execution

```javascript
// (PSEUDO-CODE)

GlobalExectionContext = {
LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // Identifier bindings go here
      a: 20,
      b: 30,
      multiply: < func >
    }
    outer: <null>,
    ThisBinding: <Global Object>
  },
VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // Identifier bindings go here
      c: undefined,
    }
    outer: <null>,
    ThisBinding: <Global Object>
  }
}
```

### 3 - `multiply`  context creation

```javascript
// (PSEUDO-CODE)

FunctionExectionContext = {
LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      Arguments: {0: 20, 1: 30, length: 2},
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>,
  },
VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      g: undefined
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  }
}
```

### 4 - `multiply` execution

```javascript
// (PSEUDO-CODE)

FunctionExectionContext = {
LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      Arguments: {0: 20, 1: 30, length: 2},
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>,
  },
VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // Identifier bindings go here
      g: 20
    },
    outer: <GlobalLexicalEnvironment>,
    ThisBinding: <Global Object or undefined>
  }
}
```

## Articles and books used for this document

- Standard ECMA-262
  - [ES5 execution context](http://ecma-international.org/ecma-262/5.1/#sec-10.3)
  - [ES6 execution context](http://www.ecma-international.org/ecma-262/6.0/#sec-execution-contexts)
- Harold Abelson and Gerald Jay Sussman with Julie Sussman
  - [SICP: chapter 5.5.6 Lexical Addressing](https://web.mit.edu/alexmv/6.037/sicp.pdf)
- Dmitry Soshnikov
  - [execution-contexts](http://dmitrysoshnikov.com/ecmascript/chapter-1-execution-contexts/)
  - [variable-object](http://dmitrysoshnikov.com/ecmascript/chapter-2-variable-object/)
  - [lexical-environments-common-theory](http://dmitrysoshnikov.com/ecmascript/es5-chapter-3-1-lexical-environments-common-theory/)
  - [lexical-environments-ecmascript-implementation](http://dmitrysoshnikov.com/ecmascript/es5-chapter-3-2-lexical-environments-ecmascript-implementation/)
- Michael McMillan
  - [An easy intro to Lexical Scoping in JavaScript](https://medium.freecodecamp.org/lexical-scoping-in-javascript-e876cd221b74)
- Sukhjinder Arora
  - [Understanding Execution Context and Execution Stack in Javascript.](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0)
- Rupesh Mishra
  - [Execution context, Scope chain and JavaScript internals](https://hackernoon.com/execution-context-in-javascript-319dd72e8e2c)
- Bilal Alam
  - [Javascript Scope Chain and Execution Context simplified](https://medium.com/koderlabs/javascript-scope-chain-and-execution-context-simplified-ffb54fc6ad02)