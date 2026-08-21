# 1. Variables in JavaScript

The main variable declaration techniques are `var`, `let`, and `const`.

## 1. `const`
`const` is used to declare a constant variable. Once assigned, its value cannot be changed.

Example:
```js
const a = 10;
// a = 20;
// console.log(a); // output: Uncaught TypeError: Assignment to constant variable.
```

## 2. `let`
`let` is used to declare a variable whose value can be changed later.

Example:
```js
let a = 10;
a = 20;
// console.log(a); // output: 20
```

## 3. `var`
`var` is the old way of declaring variables. It can be changed later, but it is not block-scoped, so it is not recommended to use it.

Example:
```js
var a = 10;
a = 20;
// console.log(a); // output: 20
```

## 4. Declaring a variable without any declaration keyword
You can also declare a variable without using `var`, `let`, or `const`, but this is not recommended.

Example:
```js
a = 10;
// console.log(a); // output: 10
```

Tip: `let` and `const` are block-scoped, but `var` is not. So avoid using `var` because it is an older technique.

# 2. Data Types in JavaScript

```js
let number = 5; // number: range up to 2^53 - 1
let string = "ahmed"; // string
let boolean = true; // boolean: true or false
let empty = null; // null: a standalone value
let notDefined; // undefined
let object = { name: "ahmed", age: 20 }; // object
```

Common JavaScript data types:
- Number
- String
- Boolean
- Null
- Undefined
- Object

`typeof` operator can be used to check the data type of a variable.

Example:
```js
let number = 5;
console.log(typeof number); // output: number
console.log(typeof "ahmed"); // output: string
```
Tip: `console.log(typeof null)` will return "object" because null is considered an object in JavaScript. 

