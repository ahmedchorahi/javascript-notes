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

> **Tip:** `let` and `const` are block-scoped, but `var` is not. Avoid using `var because it is an older technique.

# 2. Data Types in JavaScript

```js
let number = 5; // number: range up to 2^53 - 1
let string = "ahmed"; // string
let boolean = true; // boolean: true or false
let empty = null; // null: a standalone value
let notDefined; // undefined
let object = { name: "ahmed", age: 20 }; // object
```

Common JavaScript data types include:
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
> **Tip:** `console.log(typeof null)` returns `"object"` because of a legacy behavior in JavaScript. `null` is not actually an object.

Data type conversion is the process of converting a variable from one data type to another. For example, converting a string to a number or vice versa.

Example:
```js
let score = "100";
console.log(typeof score); // output: string
let valueToNumber = Number(score);
console.log(typeof valueToNumber); // output: number
console.log(valueToNumber); // output: 100

let score2 = "100abc";
console.log(typeof score2); // output: string
let valueToNumber2 = Number(score2);
console.log(typeof valueToNumber2); // output: number
console.log(valueToNumber2); // output: NaN 

// "100abc" cannot be converted to a number, so the result is NaN (Not a Number).
```     

# 3. Operations in JavaScript
Basic arithmetic operations can be performed using the following operators:

```js
let value = 3;
let negValue = -value; // negValue = -3
console.log(negValue); // output: -3

console.log(3 + 2); // output: 5
console.log(3 - 2); // output: 1
console.log(3 * 2); // output: 6
console.log(3 ** 2); // output: 9 exponentiation operator
console.log(3 / 2); // output: 1.5
console.log(3 % 2); // output: 1 modulus operator
``` 

String concatenation can be done using the `+` operator.

```js
let str1 = "Hello";
let str2 = " World";
console.log(str1 + str2); // output: Hello World
console.log("1" + "2"); // output: 12
console.log("1" + 2); // output: 12
console.log(1 + "2"); // output: 12
console.log(1 + 2); // output: 3
console.log("1" + 2 + 3); // output: 123
console.log(1 + 2 + "3"); // output: 33 addition is performed first, then concatenation
```

Prefix and postfix operators increment or decrement a variable by 1. The prefix operator changes the variable before returning its value, while the postfix operator returns the original value before changing it.

```js
let a = 5;
console.log(a++); // output: 5 (postfix increment)
console.log(a); // output: 6
console.log(++a); // output: 7 (prefix increment)
```

# 4. Comparison Operators in JavaScript
Comparison operators are used to compare two values and return a boolean value (true or false).

Basic comparison operators include:

```js
console.log(3 > 2); // output: true
console.log(3 < 2); // output: false
console.log(3 >= 2); // output: true
console.log(3 <= 2); // output: false
console.log(3 == 2); // output: false
console.log(3 != 2); // output: true
```

### Comparison with strings

```js
console.log("a" > "b"); // output: false
console.log("a" < "b"); // output: true
console.log("a" > 1); // output: false ("a" is converted to NaN)
console.log("1" > 2); // output: false ("1" is converted to 1)
```

### Strict comparison operators

```js
console.log(3 === 2); // output: false
console.log(3 !== 2); // output: true
console.log(3 === "3"); // output: false (different data types)
console.log(3 !== "3"); // output: true (different data types)
```

## 5. Primitive and Reference Types

> **Interview note:** Based on how values are stored and accessed, JavaScript data types are commonly described as primitive or reference types.

### Primitive data types

Primitive values are copied by value. When one variable is assigned to another, the values are independent.

The seven primitive data types are:

- String
- Number
- Boolean
- `null`
- `undefined`
- Symbol
- BigInt

Example:
```js
let a = 10;
let b = a; // b is assigned the value of a
b = 20; // changing b does not affect a
console.log(a); // output: 10
console.log(b); // output: 20
```

### Reference types / Non Primitives

Objects are reference values. Assigning an object to another variable copies the reference, so both variables can refer to the same object.

Reference types include:
- Object
- Array
- Function

Example:
```js
let obj1 = { name: "ahmed" };
let obj2 = obj1; // obj2 is assigned the reference of obj1
obj2.name = "ali"; // changing obj2 affects obj1
console.log(obj1.name); // output: ali
console.log(obj2.name); // output: ali
```

> **Tip:** stack(primitive data types) and heap(reference data types) 

### `typeof` return values

| Value | `typeof` result |
| --- | --- |
| `1` | `number` |
| `"abc"` | `string` |
| `null` | `object` |
| `undefined` | `undefined` |
| `function () {}` | `function` |
| `[]` | `object` |
| `{}` | `object` |

## 6. Static and Dynamic Typing

In statically typed languages, a variable's data type is determined at compile time and generally cannot change at runtime. Examples include Java, C++, and C#.

JavaScript is dynamically typed, so a variable can hold values of different types at different times:

```js
let value = 10;
value = "ten";
```

# 7. Strings in JavaScript

## 1. Template Literals (String Interpolation)
Template literals use backticks (`` ` ``) instead of single or double quotes. They allow you to embed variables and expressions directly into strings using the `${expression}` syntax.

Example:
```js
let name = "ahmed";
let age = 24;

console.log(`Hello, my name is ${name} and I am ${age} years old.`);
// output: Hello, my name is ahmed and I am 24 years old.
```

## 2. Accessing Characters
Strings act like arrays, meaning their individual characters can be accessed using zero-based indices.

Example:
```js
let str = "ahmed";
console.log(str[0]); // output: a
console.log(str[1]); // output: h
console.log(str[2]); // output: m
console.log(str[3]); // output: e
console.log(str[4]); // output: d
```

## 3. String Methods
Here are some of the most commonly used string methods in JavaScript:

```js
let name = "muhammad ahmed";

// 1. Length of the string
console.log(name.length); // output: 14

// 2. Convert to uppercase
console.log(name.toUpperCase()); // output: MUHAMMAD AHMED

// 3. Convert to lowercase
console.log(name.toLowerCase()); // output: muhammad ahmed

// 4. Remove whitespace from both sides
console.log(name.trim()); // output: muhammad ahmed

// 5. Slice / Extract a section of a string
console.log(name.slice(0, 8)); // output: muhammad

// 6. Replace a substring
console.log(name.replace("muhammad", "ahmed")); // output: ahmed ahmed

// 7. Concatenate strings
console.log(name.concat(" chorahi")); // output: muhammad ahmed chorahi
```

# 8. Numbers and Math in JavaScript

## 1. Declaring Numbers
In JavaScript, numbers can be declared as primitive values or instantiated as objects using the `Number` constructor.

Example:
```js
let age = 24; // primitive number
let score = new Number(100); // number object

console.log(age); // output: 24
console.log(score); // output: [Number: 100]
console.log(typeof age); // output: number
console.log(typeof score); // output: object
```

## 2. Number Methods
You can convert a number to a string to access string methods, or format it with fixed decimals, precision, and localization.

Example:
```js
let number = 100;
let otherNumber = 123.456;
let money = 1000000000;
let numObj = new Number(100);

// 1. Convert to string and get length
console.log(number.toString().length); // output: 3

// 2. Format with fixed decimal places
console.log(number.toFixed(2)); // output: 100.00

// 3. Format to a specific precision (total significant digits)
console.log(otherNumber.toPrecision(4)); // output: 123.5
console.log(number.toPrecision(2)); // output: 1.0e+2

// 4. Format to exponential notation
console.log(number.toExponential(2)); // output: 1.0e+2

// 5. Format to local currency/number format
console.log(money.toLocaleString()); // output: 1,000,000,000 (depends on locale)

// 6. Get primitive value from Number object
console.log(numObj.valueOf()); // output: 100
```

## 3. Math Object
The built-in `Math` object provides properties and methods for mathematical constants and operations.

Example:
```js
console.log(Math.abs(-100)); // output: 100 (absolute value)
console.log(Math.round(100.5)); // output: 101 (rounds to nearest integer)
console.log(Math.ceil(100.2)); // output: 101 (rounds up)
console.log(Math.floor(100.9)); // output: 100 (rounds down)
console.log(Math.min(100, 200, 50)); // output: 50 (minimum value)
console.log(Math.max(100, 200, 50)); // output: 200 (maximum value)
console.log(Math.random()); // output: random value between 0 (inclusive) and 1 (exclusive)

// Generating a random number between a range [min, max]:
const min = 1;
const max = 100;
console.log(Math.floor(Math.random() * (max - min + 1) + min)); // output: random integer between 1 and 100
```

# 9. Date and Time in JavaScript

JavaScript `Date` objects represent a single moment in time. Dates are stored as milliseconds elapsed since the Unix Epoch (January 1, 1970 UTC).

## 1. Creating Date Objects
There are multiple ways to initialize a new `Date` object:

Example:
```js
// 1. Current date and time
let currentDate = new Date();
console.log(currentDate);

// 2. From a date string
let specificDate1 = new Date("2026-08-28");
console.log(specificDate1.toDateString()); // output: Fri Aug 28 2026

// 3. From arguments: (year, monthIndex, day, hours, minutes, seconds, milliseconds)
// Note: Month is 0-indexed (0 = January, 11 = December)
let specificDate2 = new Date(2026, 7, 28, 12, 30, 0); 
console.log(specificDate2.toLocaleString()); // output: 8/28/2026, 12:30:00 PM (format depends on locale)
```

> **Tip:** In JavaScript, the month index is 0-based: `0` is January and `11` is December.

## 2. Getting Date and Time Values
You can retrieve specific components of a date using standard getter methods:

Example:
```js
let date = new Date("2026-08-28T12:44:00");

console.log(date.getFullYear()); // output: 2026
console.log(date.getMonth()); // output: 7 (August, 0-indexed)
console.log(date.getDate()); // output: 28 (day of the month)
console.log(date.getDay()); // output: 5 (Friday, 0-indexed starting with Sunday as 0)
console.log(date.getHours()); // output: 12
console.log(date.getMinutes()); // output: 44
console.log(date.getSeconds()); // output: 0
console.log(date.getTime()); // output: 1787903040000 (timestamp in milliseconds)
```

## 3. Formatting Dates
Here are common methods used to format dates into human-readable strings:

Example:
```js
let date = new Date("2026-08-28T12:44:00");

console.log(date.toString()); // output: Fri Aug 28 2026 12:44:00 GMT...
console.log(date.toDateString()); // output: Fri Aug 28 2026 (only the date part)
console.log(date.toTimeString()); // output: 12:44:00 GMT... (only the time part)
console.log(date.toISOString()); // output: 2026-08-28T07:44:00.000Z (standard ISO format)
console.log(date.toLocaleString()); // output: 8/28/2026, 12:44:00 PM (depends on locale)
```

## 4. Timestamps and Comparison
Timestamps represent the milliseconds elapsed since the Unix Epoch. They are highly efficient for comparing dates.

Example:
```js
// 1. Get the current timestamp
let currentTimestamp = Date.now();
console.log(currentTimestamp);

// 2. Get the timestamp of a specific date
let dateTimestamp = new Date("2026-08-28").getTime();
console.log(dateTimestamp);

// 3. Compare two dates
let date1 = new Date("2026-08-28");
let date2 = new Date("2026-08-29");

console.log(date1 < date2); // output: true
```


