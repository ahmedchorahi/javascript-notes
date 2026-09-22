# Loops & Higher-Order Iteration in JavaScript

Loops are control flow statements used to repeatedly execute a block of code as long as a specified condition remains `true`. In modern JavaScript, we have traditional loops, collection-specific loops (`for...of`, `for...in`), and higher-order array iteration methods (`forEach`, `map`, `filter`, `reduce`).

---

## 1) Traditional For Loop

A loop that executes a block of code a specific number of times.

### Syntax
```js
for (initialization; condition; increment/decrement) {
    // code block to be executed
}
```

### Basic Example
```js
for (let i = 0; i <= 10; i++) {
    console.log(i);
}
```

### Conditionals Inside Loops
```js
for (let i = 1; i <= 10; i++) {
    if (i === 5) {
        console.log("Detected 5!");
    }
    console.log(`Value of i is ${i}`);
}
```

### Nested Loop (e.g., Multiplication Tables)
```js
for (let i = 1; i <= 3; i++) {
    console.log(`\nOuter loop value: ${i}`);
    for (let j = 1; j <= 3; j++) {
        console.log(`  Inner loop: ${i} * ${j} = ${i * j}`);
    }
}
```

---

## 2) Loop Control Statements: `break` and `continue`

| Keyword | Purpose | Behavior |
| :--- | :--- | :--- |
| `break` | Terminate loop immediately | Stops the loop completely and jumps execution to the first statement after the loop. |
| `continue` | Skip current iteration | Skips the rest of the code in the current iteration and advances to the next step. |

### Example: `break`
```js
for (let i = 1; i <= 10; i++) {
    if (i === 5) {
        console.log("Detected 5, breaking loop!");
        break; // Loop terminates here
    }
    console.log(i);
}
// Prints: 1, 2, 3, 4
```

### Example: `continue`
```js
for (let i = 1; i <= 10; i++) {
    if (i === 5) {
        console.log("Detected 5, skipping iteration!");
        continue; // Skips 5, moves to i = 6
    }
    console.log(i);
}
// Prints: 1, 2, 3, 4, (skips 5), 6, 7, 8, 9, 10
```

---

## 3) While and Do-While Loops

### A. `while` Loop
Evaluates the condition **before** entering the loop body. If the condition is false initially, the body will never run.

```js
let index = 0;

while (index <= 10) {
    console.log(`Index is: ${index}`);
    index += 2;
}
// Output: 0, 2, 4, 6, 8, 10
```

### B. `do...while` Loop
Evaluates the condition **after** executing the loop body. **Guaranteed to run at least once**, even if the condition is initially false.

```js
let score = 11;

do {
    console.log(`Score is: ${score}`);
    score++;
} while (score <= 10);

// Output: Score is: 11 (executed once despite score > 10)
```

---

## 4) `for...of` Loop (Iterating Iterables)

Used to iterate directly over **values** of an iterable object (Arrays, Strings, Maps, Sets).

### A. On Arrays
```js
const numbers = [1, 2, 3, 4, 5];

for (const num of numbers) {
    console.log(num);
}
```

### B. On Strings
```js
const greetings = "Hello!";

for (const char of greetings) {
    console.log(char);
}
// Prints each character on a new line
```

---

## 5) JavaScript `Map` Object & `for...of`

A `Map` holds key-value pairs and **remembers the original insertion order** of keys. Unlike plain objects, any value (objects, functions, primitives) can be used as a key.

```js
const map = new Map();

map.set("PK", "Pakistan");
map.set("IN", "India");
map.set("FR", "France");
map.set("PK", "Islamic Republic of Pakistan"); // Overwrites "PK" (keys are unique)

console.log(map);
// Output: Map(3) { 'PK' => 'Islamic Republic of Pakistan', 'IN' => 'India', 'FR' => 'France' }
```

### Iterating over a Map with `for...of`
```js
// Destructuring [key, value]
for (const [key, value] of map) {
    console.log(`${key} :- ${value}`);
}
```

### Interview Point: Can we use `for...of` on plain Objects?
**No.** Plain JavaScript objects (`{}`) are **not iterable** by default using `for...of` (they do not implement the `Symbol.iterator` method). Doing so throws `TypeError: obj is not iterable`.

---

## 6) `for...in` Loop (Iterating Keys)

Used to iterate over the **enumerable property keys (or indices)** of an object or array.

### A. On Plain Objects (Primary Use Case)
```js
const languages = {
    cpp: "C++",
    js: "JavaScript",
    py: "Python",
    rb: "Ruby"
};

for (const key in languages) {
    console.log(`${key} is shortcut for ${languages[key]}`);
}
// cpp is shortcut for C++
// js is shortcut for JavaScript
// py is shortcut for Python
// rb is shortcut for Ruby
```

### B. On Arrays
```js
const programming = ["js", "py", "cpp", "java"];

for (const key in programming) {
    console.log(`Index: ${key}, Value: ${programming[key]}`);
}
// key gives index: 0, 1, 2, 3
```

### Interview Point: `for...of` vs `for...in`
- **`for...in`** iterates over **keys / property names / indices**. Best suited for **Objects**.
- **`for...of`** iterates directly over **values**. Best suited for **Arrays, Strings, Maps, Sets**.

---

## 7) Array Higher-Order Methods

A **higher-order function** is a function that accepts another function as an argument (callback) or returns a function.

---

### A. `forEach()` Method
Executes a provided callback function once for each array element.

```js
const coding = ["js", "ruby", "java", "python", "cpp"];

// 1. Regular callback function
coding.forEach(function (item) {
    console.log(item);
});

// 2. Arrow function callback
coding.forEach((item) => {
    console.log(item);
});

// 3. Passing a pre-defined function reference
function printMe(item) {
    console.log(item);
}
coding.forEach(printMe); // Pass function reference, DO NOT call it printMe()
```

#### Accessing Item, Index, and Full Array
The callback accepts 3 parameters: `(item, index, array)`
```js
coding.forEach((item, index, arr) => {
    console.log(`Index: ${index} | Item: ${item} | Full Array:`, arr);
});
```

#### Iterating an Array of Objects (Real-World Pattern)
```js
const myCoding = [
    { languageName: "JavaScript", languageFile: "js" },
    { languageName: "Python", languageFile: "py" },
    { languageName: "Java", languageFile: "java" }
];

myCoding.forEach((item) => {
    console.log(`${item.languageName} uses .${item.languageFile} extension`);
});
```

### Critical Interview Question: What does `forEach()` return?
`forEach()` **always returns `undefined`**. It does not return a new array, and you cannot assign its output or chain other array methods directly after it.

```js
const values = coding.forEach((item) => {
    return item;
});
console.log(values); // Output: undefined
```

---

### B. `filter()` Method
Returns a **new array** containing only the elements that satisfy the given condition (`true`).

```js
const myNums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Implicit return (no curly braces)
const newNums = myNums.filter((num) => num > 4);
console.log(newNums); // Output: [5, 6, 7, 8, 9, 10]

// Explicit return (MUST use 'return' keyword inside curly braces {})
const newNumsExplicit = myNums.filter((num) => {
    return num > 4;
});
```

#### Practical Example: Filtering Real-world Datasets
```js
const books = [
    { title: "1984", genre: "Dystopian", publishingYear: 1949, editionYear: 2021 },
    { title: "Pride and Prejudice", genre: "Romance", publishingYear: 1813, editionYear: 2022 },
    { title: "To Kill a Mockingbird", genre: "Fiction", publishingYear: 1960, editionYear: 2020 },
    { title: "The Great Gatsby", genre: "Tragedy", publishingYear: 1925, editionYear: 2022 },
    { title: "The Hobbit", genre: "Fantasy", publishingYear: 1937, editionYear: 2012 },
    { title: "The Alchemist", genre: "Adventure", publishingYear: 1988, editionYear: 2014 }
];

// Query 1: Books published after 1950
const modernBooks = books.filter((bk) => bk.publishingYear > 1950);
console.log(modernBooks);

// Query 2: Adventure books published after 1980
const adventureBooks = books.filter((bk) => {
    return bk.genre === "Adventure" && bk.publishingYear >= 1980;
});
console.log(adventureBooks);
```

---

### C. `map()` Method
Transforms every element in an array and returns a **new array** of the exact same length.

```js
const myNumbers = [1, 2, 3, 4, 5];

const addedNums = myNumbers.map((num) => num + 10);
console.log(addedNums); // Output: [11, 12, 13, 14, 15]
```

#### Method Chaining
Methods can be chained one after another. The output of one method becomes the input to the next:

```js
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const result = numbers
    .map((num) => num * 10)       // Multiply each by 10
    .map((num) => num + 1)        // Add 1 to each ([11, 21, 31, ...])
    .filter((num) => num >= 40);  // Keep only elements >= 40

console.log(result); // Output: [41, 51, 61, 71, 81, 91, 101]
```

---

### D. `reduce()` Method
Executes a "reducer" callback function on each element of the array, resulting in a **single accumulated output value**.

### Syntax
```js
array.reduce((accumulator, currentValue) => {
    return accumulator + currentValue;
}, initialValue);
```

- **`accumulator`**: Stores the accumulated result returned from the previous iteration (starts with `initialValue` on first iteration).
- **`currentValue`**: The current element being processed in the array.
- **`initialValue`**: The value to pass as the accumulator in the first iteration.

### Example 1: Sum and Product of Numbers
```js
const nums = [1, 2, 3, 4];

// Summing numbers with initial value 0
const totalSum = nums.reduce((acc, curr) => {
    console.log(`acc: ${acc} and curr: ${curr}`);
    return acc + curr;
}, 0);
console.log("Total sum:", totalSum); // Output: 10

// Multiplying numbers with initial value 1
const product = nums.reduce((acc, curr) => acc * curr, 1);
console.log("Product:", product); // Output: 24
```

### Example 2: Shopping Cart Total (Real-World Use Case)
```js
const shoppingCart = [
    { itemName: "JavaScript Course", price: 2999 },
    { itemName: "Python Course", price: 1999 },
    { itemName: "Mobile Dev Course", price: 5999 },
    { itemName: "Data Science Course", price: 12999 }
];

const priceToPay = shoppingCart.reduce((acc, item) => acc + item.price, 0);
console.log(`Total checkout amount: ₹${priceToPay}`); // Output: ₹23996
```

---

## Summary / Quick Interview Cheat Sheet

| Loop / Method | Target / Input | Output / Return | Key Characteristic |
| :--- | :--- | :--- | :--- |
| **`for`** | Any counter / range | None | Manual index control (`break` and `continue` supported). |
| **`while`** | Conditional loops | None | Checks condition **before** body execution. |
| **`do...while`** | Conditional loops | None | Executes **at least once** before checking condition. |
| **`for...of`** | Iterables (Array, String, Map) | Values | Iterates directly over **values**. Plain `{}` objects not supported. |
| **`for...in`** | Objects & Arrays | Keys / Indices | Iterates over **enumerable property names / keys**. |
| **`forEach()`** | Array | `undefined` | Performs side-effects for each element. Cannot break/return early. |
| **`filter()`** | Array | New filtered Array | Keeps elements where callback returns `true`. |
| **`map()`** | Array | New transformed Array | Transforms each element; returns an array of the same length. |
| **`reduce()`** | Array | Single accumulated value | Aggregates all elements into a single result (sum, object, tally). |
