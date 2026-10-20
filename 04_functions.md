# JavaScript Functions: Master Interview Guide

In JavaScript, **functions are first-class citizens**. This means functions can be treated like any other variable: stored in variables, passed as arguments into other functions, and returned from other functions.

---

## 1. The 3 Main Function Types Compared

```text
                        FUNCTION TYPES IN JAVASCRIPT
+---------------------+-------------------------------+-------------------------------+
| Type                | Syntax                        | Hoisted? & 'this' Behavior    |
+---------------------+-------------------------------+-------------------------------+
| 1. Declaration      | function add(a, b) { ... }    | ✅ Fully hoisted with body    |
|                     |                               | Has its own dynamic 'this'    |
| 2. Expression       | const add = function(a, b){}; | ❌ Variable hoisted (TDZ)     |
|                     |                               | Has its own dynamic 'this'    |
| 3. Arrow Function   | const add = (a, b) => a + b;  | ❌ Variable hoisted (TDZ)     |
|                     |                               | 🎯 Lexical 'this' (Inherited) |
+---------------------+-------------------------------+-------------------------------+
```

### 1. Function Declaration (Hoisted)
Can be invoked **before** its definition in the source code:
```javascript
console.log(calculateSum(5, 5)); // ✅ Works! Output: 10

function calculateSum(a, b) {
  return a + b;
}
```

### 2. Function Expression (Not Hoisted)
Stored in a variable. Cannot be called before declaration:
```javascript
// console.log(multiply(2, 3)); // ❌ ReferenceError: Cannot access 'multiply' before initialization

const multiply = function(a, b) {
  return a * b;
};
```

### 3. Arrow Function (ES6)
Concise syntax with implicit return and lexical `this`:
```javascript
// Explicit Return (requires curly braces and 'return')
const addExplicit = (a, b) => {
  return a + b;
};

// Implicit Return (no braces, single expression)
const addImplicit = (a, b) => a + b;

// Returning an Object Literal (must wrap in parentheses to avoid ambiguity with {})
const makeUser = (name, age) => ({ name, age });
console.log(makeUser("Ahmed", 24)); // { name: 'Ahmed', age: 24 }
```

---

## 2. Why Were Arrow Functions Created?

> [!IMPORTANT]
> **Major Interview Question**:
> *"Why did ES6 introduce Arrow Functions if we already had regular functions?"*

### 1. Lexical `this` Binding (Solved the Biggest Bug in JS)
In regular functions, the `this` keyword is **dynamic** and depends on *how the function is called*. Inside callbacks (like `setTimeout` or event listeners), regular functions lost their outer object context, forcing developers to write `var self = this;` or use `.bind(this)`.

Arrow functions **do not have their own `this`**. They capture `this` lexically from their enclosing scope:

```javascript
const counter = {
  count: 0,
  startRegular() {
    setTimeout(function() {
      // ⚠️ Regular function creates its own 'this' pointing to window/global!
      console.log("Regular this.count:", this.count); // undefined (or NaN)
    }, 500);
  },
  startArrow() {
    setTimeout(() => {
      // ✅ Arrow function inherits 'this' from startArrow() -> the counter object!
      console.log("Arrow this.count:", this.count); // 0
    }, 500);
  }
};

counter.startRegular();
counter.startArrow();
```

### 2. Concise Syntax
Cleaner syntax with implicit returns, particularly beneficial in functional array methods (`arr.map(x => x * 2)`).

### 3. Non-Constructible
Arrow functions do not possess a `[[Construct]]` method or a `.prototype` property. They **cannot be called with `new`**, preventing accidental use as constructor functions.

---

## 3. Parameters, Arguments, Rest & Default Values

```text
Parameter = The variable listed in the function definition (Placeholder)
Argument  = The real value passed to the function when invoked
```

### 1. Default Parameters
```javascript
// Provides fallback if argument is undefined
function greetUser(username = "Guest") {
  return `Hello, ${username}!`;
}

console.log(greetUser("Ahmed")); // "Hello, Ahmed!"
console.log(greetUser());        // "Hello, Guest!"
console.log(greetUser(null));    // "Hello, null!" (Default only triggers on undefined!)
```

### 2. Rest Parameters (`...rest`)
Collects an indefinite number of remaining arguments into an actual JavaScript Array:
```javascript
function calculateCartTotal(discount, tax, ...prices) {
  console.log("Discount:", discount); // 10
  console.log("Tax:", tax);           // 5
  console.log("Prices Array:", prices); // [100, 200, 300]

  const subtotal = prices.reduce((acc, curr) => acc + curr, 0);
  return subtotal - discount + tax;
}

console.log(calculateCartTotal(10, 5, 100, 200, 300)); // 295
```

---

## 4. The `this` Keyword Context Summary

| Context | `this` in Regular Function | `this` in Arrow Function |
| :--- | :--- | :--- |
| **Global Scope** | `window` (Browser) / `{}` (Node module) | `window` / `{}` |
| **Object Method** | The object before the dot (`user.greet()` ➔ `user`) | Outer enclosing lexical context |
| **Standalone Function** | `window` (Non-strict) / `undefined` (Strict Mode) | Outer enclosing lexical context |
| **DOM Event Listener** | The DOM element receiving the event | Outer enclosing scope (usually `window`) |

---

## 5. IIFE (Immediately Invoked Function Expression)

An **IIFE** is a function that runs immediately upon definition.

```javascript
// 1. Named IIFE
(function connectDB() {
  console.log("Database connected immediately!");
})(); // ⚠️ Semicolon is MANDATORY here to prevent syntax errors with next statement!

// 2. Unnamed / Arrow IIFE with Arguments
((dbName) => {
  console.log(`Connected to cluster: ${dbName}`);
})("Production_DB");
```

### Why use an IIFE?
1. **Prevent Global Scope Pollution**: Variables declared inside an IIFE stay private and do not leak into `window` or collide with external scripts.
2. **Module Pattern / Encapsulation**: Historically used to create private state before ES6 modules (`import`/`export`) existed.

---

## 6. Higher-Order Functions & Callbacks

* **Callback Function**: A function passed into another function as an argument.
* **Higher-Order Function (HOF)**: A function that receives another function as a parameter or returns a function.

```javascript
// Higher-Order Function
function transformNumbers(arr, operationCallback) {
  const result = [];
  for (let num of arr) {
    result.push(operationCallback(num));
  }
  return result;
}

// Callbacks
const double = (n) => n * 2;
const square = (n) => n * n;

console.log(transformNumbers([1, 2, 3], double)); // [2, 4, 6]
console.log(transformNumbers([1, 2, 3], square)); // [1, 4, 9]
```

---

## 7. Top 7 Interview Questions & Exact Answers

### Q1: What is the difference between Function Declaration and Function Expression?
> **Answer**: Function declarations are hoisted completely into memory during the creation phase, allowing them to be called before they appear in code. Function expressions are assigned to variables (`let`, `const`, or `var`), so only the variable declaration is hoisted; invoking them before definition throws a `ReferenceError` (or `TypeError` if declared with `var`).

---

### Q2: Why don't Arrow Functions have their own `this`?
> **Answer**: Arrow functions were intentionally designed to capture `this` lexically from their enclosing scope at the time of creation. They do not have their own internal `this`, `arguments`, `super`, or `new.target` bindings.

---

### Q3: What is the `arguments` object in JavaScript, and does it exist in Arrow Functions?
> **Answer**: `arguments` is an array-like object available inside regular functions containing all passed arguments. It does **not** exist in arrow functions; in arrow functions, you must use Rest parameters (`(...args) => {}`) instead.

---

### Q4: Why is a semicolon mandatory after an IIFE?
> **Answer**: If you define an IIFE without a terminating semicolon, and another statement or IIFE follows it, the JavaScript parser will treat the second parenthesis as an argument call on the return value of the first function, causing a `TypeError: (intermediate value)(...) is not a function`.

---

### Q5: What is the difference between Parameters and Arguments?
> **Answer**: 
> - **Parameters** are the named variables specified in the function declaration signature (`function foo(x, y)`).
> - **Arguments** are the actual concrete values supplied to the function when it is executed (`foo(5, 10)`).

---

### Q6: Can arrow functions be used as constructors with `new`?
> **Answer**: **No.** Arrow functions lack the internal `[[Construct]]` method and do not possess a `.prototype` property. Attempting to call `new MyArrowFn()` throws `TypeError: MyArrowFn is not a constructor`.

---

### Q7: What is a First-Class Function?
> **Answer**: A programming language is said to have First-Class Functions when functions are treated as first-class values: they can be assigned to variables, passed as arguments to other functions, returned from functions, and have properties assigned to them just like any other object.
