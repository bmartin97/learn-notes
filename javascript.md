# JavaScript

__Start Date__: 2024. 03. 19.
__End Date__: 2024. 03. 26.

__To Learn__:
```
+ var vs let vs const
+ scopes
+ hoisting
+ IIFE
o callbacks
o async/await
o promises
o closures
o currying
o event loop
o prototype, object
o import, export EJS
```


## Declaration vs Initialization

```js
var x; // Declaration 

x = 10; // Initialization 

var y = 20; // Declaration and initialization
```


## Declaration types
__var__: variable, initialization is optional.
__let__: block-scoped variable, initialization is optional.
__const__: block-scoped read-only named constant, Initialization is required.

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

## Scopes
### Global scope
The default scope. In web pages global object is `window`  (or `globalThis`).


#### Example
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

### Module scope 
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

#### Module scoped variables in modules
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

#### Global scoped variables in modules
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

### Function scope 
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

### Block scope
The scope created with a pair of curly braces (a block).
```js
{
  let age = 10;
  const name = "Sarah"

  console.log(age); // Output: 10
  console.log(name) // Output: "Sarah"
}

console.log(age); // ReferenceError: age is not defined
console.log(name) // ReferenceError: name is not defined
```

# Hoisting
Variable and function declarations are moved to the top of their containing scope during the compilation phase, before the code is executed.

### Hoisting of `var` declared variables
Access a variable before it's declared, the value is always undefined
```js
console.log(age); // Output: undefined

var age = 10;

console.log(age); // Output: 10
```

### Hoisting  of `let` declared variables and `const` constants
Access the variable before its declaration results in a ReferenceError, indicating that the variable exists but has not been initialized yet.
`Reference Error` in this case is evidence of hoisting behavior.
```js
console.log(age); // ReferenceError: Cannot access 'age' before initialization

let age = 10;

console.log(age); // Output: 10
```

### Hoisting fuctions declarations
```js
myFunction(); // Output: 10

function myFunction() {
    console.log(10);
}
```

### Hoisting function expressions
```js
myFunction(); // ReferenceError: Cannot access 'myFunction' before initialization

const myFunction = () => {
    return 10;
};
```

### Hoisting example with function declaration and `var` variable
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

### Hoisting example with modules
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
console.log(age) // Output: undefined 

// Global age is still available through the `window` and `globalThis` objects
console.log(window.age) // Output: 10 

var age = 20; // Module scoped variable
console.log(age); // Output: 20
```

### Hoisting precedence order
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


## Functions

### Function Declaration
```js
function myFunction() { 
    return 10;
}
```

### Function Expression
```js
const myFunction = function() {
    return 10;
}
```

### Arrow Function Expression
```js
const myFunction = () => {
    return 10;
}
```

### Shorthand Arrow Function Expression (implicit return)
```js
const myFunction = () => 10;
```
### IIFE (Immediately Invoked Function Expression)
```js
(function() {
    console.log(10);
})();
```
