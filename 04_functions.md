# Functions in JavaScript

Functions are reusable blocks of code that perform a specific task. They help us write cleaner, modular, and maintainable code.

## 1) Function declaration

A function is defined once and can be called multiple times.

```js
function greet() {
    console.log('Hello, World!');
}

greet(); // Call the function
```

### Interview point
- A function declaration is hoisted.
- It can be called before it is defined in the code.

---

## 2) Parameters and arguments

- Parameters are variables declared in the function definition.
- Arguments are the actual values passed when calling the function.

```js
function add(a, b) {
    return a + b;
}

const result = add(5, 3); // 5 and 3 are arguments
console.log(result); // Output: 8
```

### Example
```js
function welcome(name, age) {
    console.log(`Hello ${name}, you are ${age} years old.`);
}

welcome('Ahmed', 24);
```

---

## 3) Handling undefined parameters

If no argument is passed, the parameter becomes undefined.

```js
function greet(name) {
    return `Hello, ${name || 'Guest'}!`;
}

console.log(greet('Ahmed')); // Output: Hello, Ahmed!
console.log(greet()); // Output: Hello, Guest!
```

### Default parameters

```js
function greet(name = 'Guest') {
    return `Hello, ${name}!`;
}

console.log(greet('Ahmed')); // Output: Hello, Ahmed!
console.log(greet()); // Output: Hello, Guest!
```

### Interview point
- Default parameters are useful when a value may be missing.
- They prevent undefined values from breaking logic.

---

## 4) Rest parameters

The rest parameter collects extra arguments into an array.

```js
function updateCartPrice(val1, val2, ...price) {
    return price;
}

console.log(updateCartPrice(100, 200, 300, 10)); // Output: [300, 10]
```

### Explanation
- `val1` gets `100`
- `val2` gets `200`
- `...price` gets the remaining values: `[300, 10]`

---

## 5) Functions with objects

Objects can be passed into functions and accessed inside them.

```js
const user = {
    name: 'Ahmed',
    age: 24,
    greet: function () {
        console.log(`Hello, my name is ${this.name}`);
    }
};

function handleObject(anyObject) {
    console.log(anyObject.name);
    anyObject.greet();
}

handleObject(user);
// Output:
// Ahmed
// Hello, my name is Ahmed

handleObject({
    name: 'Ali',
    greet: function () {
        console.log(`Hello, my name is ${this.name}`);
    }
});
// Output:
// Ali
// Hello, my name is Ali
```

---

## 6) Functions with arrays

```js
const numbers = [1, 2, 3, 4, 5];

function displayNumbers(arr) {
    arr.forEach(num => console.log(num));
}

displayNumbers(numbers);
// Output: 1, 2, 3, 4, 5

displayNumbers([10, 20, 30]);
// Output: 10, 20, 30
```

### Interview point
- Arrays are often passed to functions for processing.
- `forEach()` is commonly used to iterate over array elements.

---

## 7) Scope: block scope vs global scope

```js
let globalVar = 'I am a global variable';

if (true) {
    let blockVar = 'I am a block-scoped variable';
    console.log(globalVar); // Accessible
    console.log(blockVar); // Accessible
}

console.log(globalVar); // Accessible
console.log(blockVar); // Error: blockVar is not defined
```

### Explanation
- `globalVar` is accessible outside the block.
- `blockVar` is only available inside the block because it is declared with `let`.

---

## 8) Function declaration vs function expression

### Function declaration
```js
function add(x, y) {
    return x + y;
}

console.log(add(2, 3)); // Output: 5
```

### Function expression

Function expressions are assigned to variables.

```js
const multiply = function (x, y) {
    return x * y;
};

console.log(multiply(2, 3)); // Output: 6
```

### Important point
- Function declarations are hoisted.
- Function expressions are not hoisted in the same way.

```js
console.log(add(2, 3)); // Works
function add(x, y) {
    return x + y;
}

console.log(multiply(2, 3)); // Error: multiply is not defined
const multiply = function (x, y) {
    return x * y;
};
```

---

## 9) The `this` keyword

`this` refers to the current object context.

```js
const user = {
    name: 'Ahmed',
    age: 24,
    greet: function () {
        console.log(`Hello, my name is ${this.name}`);
    }
};

user.greet(); // Output: Hello, my name is Ahmed
```

### In different environments
- In the browser, `this` often refers to the `window` object.
- In Node.js, `this` in the top-level scope is not the same as browser `window`.
- Inside an object method, `this` refers to that object.

### Interview tip
- `this` works differently in regular functions, arrow functions, and object methods.

---

## 10) Different ways to define functions

### Normal function declaration
```js
function greet() {
    console.log('hello');
}
```

### Function expression
```js
const greet = function () {
    console.log('hello');
};
```

### Arrow function
```js
const greet = () => {
    console.log('hello');
};
```

### Arrow function with implicit return
```js
const add = (x, y) => x + y;
console.log(add(2, 3)); // Output: 5
```

### Return an object from arrow function
```js
const createUser = (name, age) => ({ name, age });

const user = createUser('Ahmed', 24);
console.log(user); // Output: { name: 'Ahmed', age: 24 }
```

### Interview point
- Arrow functions have shorter syntax.
- They do not bind their own `this` like regular functions.

---

## 11) IIFE (Immediately Invoked Function Expression)

An IIFE is a function that is defined and executed immediately after creation. It is often used to create a private scope and avoid global pollution.

```js
(function DB() {
    console.log('This is an IIFE');
})();
```

### Arrow function IIFE
```js
(() => {
    console.log('This is an IIFE with arrow function');
})();
```

### IIFE with parameters
```js
(function (name) {
    console.log(`Hello, ${name}!`);
})('Ahmed');
```

### Interview point
- IIFE protects variables from being added to the global scope.
- It is useful when creating temporary logic.

---

## Quick interview summary

- Functions are reusable blocks of code.
- Parameters are defined in the function; arguments are passed during execution.
- Default parameters handle missing values.
- Rest parameters collect extra arguments into an array.
- Function declarations are hoisted, while function expressions are not.
- `this` depends on the calling context.
- Arrow functions are shorter and do not bind their own `this`.
- IIFE creates a private scope immediately.

## Common mistakes to avoid

- Using `function` instead of `function` correctly spelled.
- Mixing up parameters and arguments.
- Forgetting that `this` depends on context.
- Calling a function expression before it is assigned.
- Not understanding difference between `var`, `let`, and `const` in scope.


why arrow fuction was created if we already have a regular fuction