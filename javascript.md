# JavaScript Learn Notes

<!-- vscode-markdown-toc -->

- 1. [Declaration vs Initialization](#DeclarationvsInitialization)
- 2. [Declaration types](#Declarationtypes)
- 3. [Scopes](#Scopes)
  - 3.1. [Global scope](#Globalscope)
    - 3.1.1. [Example](#Example)
  - 3.2. [Module scope](#Modulescope)
    - 3.2.1. [Module scoped variables in modules](#Modulescopedvariablesinmodules)
    - 3.2.2. [Global scoped variables in modules](#Globalscopedvariablesinmodules)
  - 3.3. [Function scope](#Functionscope)
  - 3.4. [Block scope](#Blockscope)
- 4. [Hoisting](#Hoisting)
  - 4.1. [Hoisting of `var` declared variables](#Hoistingofvardeclaredvariables)
  - 4.2. [Hoisting of `let` declared variables and `const` constants](#Hoistingofletdeclaredvariablesandconstconstants)
  - 4.3. [Hoisting fuctions declarations](#Hoistingfuctionsdeclarations)
  - 4.4. [Hoisting function expressions](#Hoistingfunctionexpressions)
  - 4.5. [Hoisting example with function declaration and `var` variable](#Hoistingexamplewithfunctiondeclarationandvarvariable)
  - 4.6. [Hoisting example with modules](#Hoistingexamplewithmodules)
  - 4.7. [Hoisting precedence order](#Hoistingprecedenceorder)
- 5. [Functions](#Functions)
  - 5.1. [Function Declaration](#FunctionDeclaration)
  - 5.2. [Function Expression](#FunctionExpression)
  - 5.3. [Arrow Function Expression](#ArrowFunctionExpression)
  - 5.4. [Shorthand Arrow Function Expression (implicit return)](#ShorthandArrowFunctionExpressionimplicitreturn)
  - 5.5. [IIFE (Immediately Invoked Function Expression)](#IIFEImmediatelyInvokedFunctionExpression)
- 6. [Callback](#Callback)
- 7. [Synchronus callback](#Synchronuscallback)
- 8. [Asynchronus callback](#Asynchronuscallback)
- 9. [Currying](#Currying)
- 10. [Closure](#Closure)
  - 10.1. [Emulate Private Variables With Clousure](#EmulatePrivateVariablesWithClousure)
- 11. [Promise](#Promise)
- 12. [Async/Await](#AsyncAwait)
- 13. [Resources:](#Resources:)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

## 1. <a name='DeclarationvsInitialization'></a>Declaration vs Initialization

```js
var x; // Declaration

x = 10; // Initialization

var y = 20; // Declaration and initialization
```

## 2. <a name='Declarationtypes'></a>Declaration types

**var**: variable, initialization is optional.
**let**: block-scoped variable, initialization is optional.
**const**: block-scoped read-only named constant, Initialization is required.

`const` prevents to assign a new value.

```js
const x = 5;
x = 10; // TypeError: Assignment to constant variable.
```

`let` and `const` declarations do check for identifiers in the current scope. If an identifier has already been declared in the same scope, attempting to declare it again with let or const will result in a syntax error.

```js
let x = 5;
let x = 10; // SyntaxError: Identifier 'x' has already been declared
```

```js
function a() {
  return 10;
}

let a = 22; // SyntaxError: Identifier 'a' has already been declared
```

## 3. <a name='Scopes'></a>Scopes

### 3.1. <a name='Globalscope'></a>Global scope

The default scope. In web pages global object is `window` (or `globalThis`).

#### 3.1.1. <a name='Example'></a>Example

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="./script1.js"></script>
    <script src="./script2.js"></script>
  </head>
</html>
```

`script1.js`

```js
var age = 10;

console.log(age); // Output: 10
console.log(window.age); // Output: 10
console.log(globalThis.age); // Output: 10
```

`script2.js`

```js
console.log(age); // Output: 10
console.log(window.age); // Output: 10
console.log(globalThis.age); // Output: 10
```

### 3.2. <a name='Modulescope'></a>Module scope

The scope for code running in module mode.

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="./module1.js" type="module"></script>
    <script src="./module2.js" type="module"></script>
  </head>
</html>
```

#### 3.2.1. <a name='Modulescopedvariablesinmodules'></a>Module scoped variables in modules

`module1.js`

```js
var age = 10; // module scope

console.log(age); // Output: 10
console.log(window.page); // Output: undefined
```

`module2.js:`

```js
console.log(age); // ReferenceError: age is not defined
console.log(window.page); // Output: undefined
```

#### 3.2.2. <a name='Globalscopedvariablesinmodules'></a>Global scoped variables in modules

`module1.js`

```js
window.age = 10;

console.log(age); // Output: 10
console.log(window.age); // Output: 10
```

`module2.js:`

```js
console.log(age); // Output: 10
console.log(window.age); // Output: 10
```

### 3.3. <a name='Functionscope'></a>Function scope

The scope created with a function.

```js
function myFunction() {
  var age = 10;
  console.log(age); // Output: 10
}
myFunction();

console.log(age); // ReferenceError: age is not defined
```

```js
var age = 5; // Global scope (or module)

function myFunction() {
  var age = 10; // Function scope
  console.log(age); // Output: 10
}
myFunction();

console.log(age); // Output: 5
```

### 3.4. <a name='Blockscope'></a>Block scope

The scope created with a pair of curly braces (a block).

```js
{
  let age = 10;
  const name = "Sarah";

  console.log(age); // Output: 10
  console.log(name); // Output: "Sarah"
}

console.log(age); // ReferenceError: age is not defined
console.log(name); // ReferenceError: name is not defined
```

## 4. <a name='Hoisting'></a>Hoisting

Variable and function declarations are moved to the top of their containing scope during the compilation phase, before the code is executed.

### 4.1. <a name='Hoistingofvardeclaredvariables'></a>Hoisting of `var` declared variables

Access a variable before it's declared, the value is always undefined

```js
console.log(age); // Output: undefined

var age = 10;

console.log(age); // Output: 10
```

### 4.2. <a name='Hoistingofletdeclaredvariablesandconstconstants'></a>Hoisting of `let` declared variables and `const` constants

Access the variable before its declaration results in a ReferenceError, indicating that the variable exists but has not been initialized yet.
`Reference Error` in this case is evidence of hoisting behavior.

```js
console.log(age); // ReferenceError: Cannot access 'age' before initialization

let age = 10;

console.log(age); // Output: 10
```

### 4.3. <a name='Hoistingfuctionsdeclarations'></a>Hoisting fuctions declarations

```js
myFunction(); // Output: 10

function myFunction() {
  console.log(10);
}
```

### 4.4. <a name='Hoistingfunctionexpressions'></a>Hoisting function expressions

```js
myFunction(); // ReferenceError: Cannot access 'myFunction' before initialization

const myFunction = () => {
  return 10;
};
```

### 4.5. <a name='Hoistingexamplewithfunctiondeclarationandvarvariable'></a>Hoisting example with function declaration and `var` variable

```js
myFunction(); // Output: undefined

function myFunction() {
  console.log(age);
}

var age = 10;
```

```js
var age = 10;

myFunction(); // Output: 10

function myFunction() {
  console.log(age);
}
```

### 4.6. <a name='Hoistingexamplewithmodules'></a>Hoisting example with modules

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="./module1.js" type="module"></script>
    <script src="./module2.js" type="module"></script>
  </head>
</html>
```

`module1.js`

```js
window.age = 10; // Globally set age variable

console.log(age); // Output: 10
console.log(window.age); // Output: 10
```

`module2.js`

```js
// Age declared and initialized in line 4, so instead of the global age variable
// it results in the hoisted `undefined` value of the age module scoped variable
console.log(age); // Output: undefined

// Global age is still available through the `window` and `globalThis` objects
console.log(window.age); // Output: 10

var age = 20; // Module scoped variable
console.log(age); // Output: 20
```

### 4.7. <a name='Hoistingprecedenceorder'></a>Hoisting precedence order

Function declarations are hoisted over variable declarations but not over variable assignments.

```js
//  Hoisted the function by precedence order
console.log(typeof a); // Output: function

var a = 5;

// The hoisted function is overwritten by the variable with an assigned value
console.log(typeof a); // Output: number

function a() {
  return 10;
}
```

```js
//  Hoisted the function by precedence order
console.log(typeof a); // Output: function

var a;

// The hoisted function is hoisted over the variable declaration.
console.log(typeof a); // Output: function

function a() {
  return 10;
}
```

`let` and `const` declarations do check for identifiers in the current scope. If an identifier has already been declared in the same scope, attempting to declare it again with let or const will result in a syntax error.

```js
// Identifier error since the 'a' identifier already exists in the scope (the hoisted function)
let a = 22; // SyntaxError: Identifier 'a' has already been declared

function a() {
  return 10;
}
```

## 5. <a name='Functions'></a>Functions

### 5.1. <a name='FunctionDeclaration'></a>Function Declaration

```js
function myFunction() {
  return 10;
}
```

### 5.2. <a name='FunctionExpression'></a>Function Expression

```js
const myFunction = function () {
  return 10;
};
```

### 5.3. <a name='ArrowFunctionExpression'></a>Arrow Function Expression

```js
const myFunction = () => {
  return 10;
};
```

### 5.4. <a name='ShorthandArrowFunctionExpressionimplicitreturn'></a>Shorthand Arrow Function Expression (implicit return)

```js
const myFunction = () => 10;
```

### 5.5. <a name='IIFEImmediatelyInvokedFunctionExpression'></a>IIFE (Immediately Invoked Function Expression)

```js
(function () {
  console.log(10);
})();
```

## 6. <a name='Callback'></a>Callback

A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.

```js
function great(name, formatCallback) {
  console.log("Hi!" + formatCallback(name));
}

function upperCaseFormatter(name) {
  return name.toUpperCase();
}

great("Sarah", upperCaseFormatter);
```

or with shorthand arrow function

```js
great("Sarah", (s) => s.toUpperCase());
```

## 7. <a name='Synchronuscallback'></a>Synchronus callback

Called immediately after the invocation of the outer function

e.g: `Array.prototype.map(callbackFn)`, `Array.prototype.forEach(callbackFn)`

## 8. <a name='Asynchronuscallback'></a>Asynchronus callback

Called at some point later, after an asynchronous operation has completed

e.g: `setTimeout(callbackFn, delay)`, `Promise.prototype.then(callbackFn)`

> Understanding whether the callback is synchronously or asynchronously called is particularly important when analyzing side

## 9. <a name='Currying'></a>Currying

Currying is a functional programming technique in JavaScript where a function with multiple arguments is transformed into a sequence of nested functions, each taking a single argument.

`f(x,y,z)` => `f(x)(y)(z)`

```js
// Original function with multiple arguments
function add(x, y, z) {
  return x + y + z;
}

// Curried version of the add function
function curriedAdd(x) {
  return function (y) {
    return function (z) {
      return x + y + z;
    };
  };
}

// Usage of the add function
console.log(add(5, 10, 15)); // Output: 30
// Usage of the curriedAdd function
console.log(curriedAdd(5)(10)(15)); // Output: 30
```

Creating a curried function can be done by a currying function like:

```js
function curry(f) {
  return function (x) {
    return function (y) {
      return function (z) {
        return f(x, y, z);
      };
    };
  };
}

const curredAdd = curry(add);

console.log(curriedAdd(5)(10)(15)); // Output: 30
```

or with shorthand arrow function

```js
const curry = (f) => (x) => (y) => (z) => f(x, y, z);
const uncurry = (f) => (x, y, z) => f(x)(y)(z);

uncurry(curry(add))(5, 10, 25);
```

> JavaScript implementations usually both keep the function callable normally and return the partial if the arguments count is not enough.

## 10. <a name='Closure'></a>Closure

Function bundled together (enclosed) with references to its surrounding state (the lexical environment).

```js
function createCounter() {
  let _count = 0; // Local variable within createCounter function

  // Inner function with access to the outer function's scope (closure)
  function increment() {
    _count++;
    console.log("Count:", _count);
  }

  // Return the inner function
  return increment;
}

// Create a counter using the createCounter function
const counter = createCounter();

// Each time you call the counter function, it increments the count variable
counter(); // Output: Count: 1
counter(); // Output: Count: 2
counter(); // Output: Count: 3
```

### 10.1. <a name='EmulatePrivateVariablesWithClousure'></a>Emulate Private Variables With Clousure

```js
const counter = (function () {
  let _count = 0; // "private" variable

  function increment() {
    _count++;
  }

  function decrement() {
    _count--;
  }

  function value() {
    return _count;
  }

  return {
    increment,
    decrement,
    value,
  };
})();

console.log(counter.value()); // Output: Count: 0

counter.increment();
console.log(counter.value()); // Output: Count: 1

counter.decrement();
console.log(counter.value()); // Output: Count: 0
```

## 11. <a name='Promise'></a>Promise

The `Promise` object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

A Promise is in one of these states:

- **pending**: initial state, neither fulfilled nor rejected.
- **fulfilled**: meaning that the operation was completed successfully.
- **rejected**: meaning that the operation failed.

Promise callbacks are handled as a `microtask` whereas `setTimeout()` callbacks are handled as task queues.

Promise Object Internal slots

```js
[[PromiseState]] : 'pending' | 'fulfilled' | 'rejected'
[[PromiseResult]]:  reject(PromiseResult) | resolve(PromiseResult)
[[PromiseFulfillReactions]]: then(fullfillReaction).then(fullfillReaction2) // added to the microtask queue
[[PromiseRejectReactions]]: catch(rejectReaction)
[[PromiseIsHandled]]: true | false
```

```js
myPromise
  .then(handleFulfilledA)
  .then(handleFulfilledB)
  .then(handleFulfilledC)
  .catch(handleRejectedAny);
```

Read More: https://www.lydiahallie.com/blog/promise-execution

## 12. <a name='AsyncAwait'></a>Async/Await

The async function declaration creates a binding of a new async function to a given name. The await keyword is permitted within the function body, enabling asynchronous, promise-based behavior to be written in a cleaner style and avoiding the need to explicitly configure promise chains.

```js
async function myFunction() {
  try {
    const resultMyPromise = await myPromise();
    const resultA = await handleFulfilledA(resultMyPromise);
    const resultB = await handleFulfilledB(resultA);
    const resultC = await handleFulfilledC(resultB);
  } catch (error) {
    handleRejectedAny(error);
  }
}
```

## Event Loop

https://www.youtube.com/watch?v=eiC58R16hb8
https://www.youtube.com/watch?v=8aGhZQkoFbQ

### Concurrency Model

The concurrency model of JavaScript is based on the concept of an “event loop”. This model enables JavaScript to carry out non-blocking I/O operations despite being a single-threaded language.

### Heap

Objects are allocated in a heap which is just a name to denote a large (mostly unstructured) region of memory.

### Queue

A JavaScript runtime uses a message queue, which is a list of messages to be processed. Each message has an associated function that gets called to handle the message.

## 13. <a name='Resources:'></a>Resources:

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#declarations
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions
- https://www.lydiahallie.com/blog/promise-execution
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures#emulating_private_methods_with_closures
- https://developer.mozilla.org/en-US/docs/Glossary/Hoisting
- https://javascript.info/currying-partials
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises
- https://developer.mozilla.org/en-US/docs/Glossary/Callback_function
- https://www.youtube.com/watch?v=eiC58R16hb8
