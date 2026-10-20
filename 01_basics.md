# JavaScript Basics & Core Fundamentals: Master Interview Guide

JavaScript is a high-level, dynamically typed, interpreted (JIT-compiled), multi-paradigm programming language. It is single-threaded and executes within host environments like Web Browsers and Node.js.

---

## 1. Variables in JavaScript: `var`, `let`, and `const`

JavaScript provides three keywords to declare variables: `var`, `let`, and `const`.

```text
                  VARIABLE DECLARATION COMPARISON
+----------+----------------+---------------+---------------+--------------------+
| Keyword  | Scope          | Re-assignable | Re-declarable | Hoisted (Usable)?  |
+----------+----------------+---------------+---------------+--------------------+
| var      | Function/Global| Yes           | Yes           | Yes (undefined)    |
| let      | Block Scope {} | Yes           | No            | TDZ (ReferenceErr) |
| const    | Block Scope {} | No (Immutable)| No            | TDZ (ReferenceErr) |
+----------+----------------+---------------+---------------+--------------------+
```

### 1. `const` (Constant)
* **Block-scoped**: Only accessible within the enclosing curly braces `{}`.
* Must be initialized at declaration time.
* Cannot be reassigned.

```javascript
const taxRate = 0.15;
// taxRate = 0.20; // ❌ TypeError: Assignment to constant variable.

// NOTE: For objects & arrays declared with const, internal contents CAN be mutated!
const user = { name: "Ahmed" };
user.name = "Ali"; // ✅ Allowed (object reference is unchanged)
```

### 2. `let` (Block-Scoped Variable)
* **Block-scoped**: Exists only within the enclosing `{}` block.
* Can be reassigned, but **cannot be re-declared** within the same scope.

```javascript
let score = 100;
score = 150; // ✅ Allowed

// let score = 200; // ❌ SyntaxError: Identifier 'score' has already been declared
```

### 3. `var` (Legacy Function-Scoped)
* **Function-scoped** (or global): Ignores block `{}` boundaries such as `if`, `for`, `while`.
* Can be re-declared and reassigned anywhere.
* **Why avoid `var`**: Causes variable leakage and unpredictable bugs in loops.

```javascript
if (true) {
  var leakedVar = "I leaked out of the if-block!";
  let safeVar = "I stay inside!";
}

console.log(leakedVar); // ✅ Prints: "I leaked out of the if-block!"
// console.log(safeVar);   // ❌ ReferenceError: safeVar is not defined
```

> [!WARNING]
> **Strict Interview Rule**: Never declare variables without a keyword (e.g. `x = 10;`). This attaches `x` directly to the global object (`window.x` or `global.x`), polluting global scope.

---

## 2. JavaScript Data Types (Primitive vs. Non-Primitive)

JavaScript has **8 built-in data types**, divided into **Primitives** (stored in Stack memory by value) and **Reference / Non-Primitives** (stored in Heap memory by reference).

```text
+-------------------------------------------------------------------------+
|                           JAVASCRIPT DATA TYPES                         |
|                                                                         |
|      1. PRIMITIVE (Stack / Value)          2. NON-PRIMITIVE (Heap / Ref)|
|      ----------------------------          ---------------------------- |
|      * String                              * Object ({})                |
|      * Number (integers & floats)          * Array ([])                 |
|      * BigInt (integers > 2^53 - 1)        * Function (function(){})    |
|      * Boolean (true / false)              * Date, RegExp, Map, Set     |
|      * undefined (declared, not assigned)                               |
|      * null (intentional empty value)                                   |
|      * Symbol (guaranteed unique key)                                   |
+-------------------------------------------------------------------------+
```

### Memory Allocation: Stack vs. Heap

```text
       STACK (Primitives - Fast, Fixed)             HEAP (Reference - Dynamic)
     +-----------------------------------+        +----------------------------+
     | a = 10                            |        |                            |
     | b = a (copies value: 10)          |        | Address #001:              |
     +-----------------------------------+        | { name: "Ali" }            |
     | userOne = Address #001 ---------->+------->|                            |
     | userTwo = Address #001 ---------->+        +----------------------------+
     +-----------------------------------+
```

```javascript
// Primitive Example (Pass-by-value):
let x = 10;
let y = x; // y gets a COPY of value 10
y = 20;    // Modifying y does NOT affect x
console.log(x); // 10
console.log(y); // 20

// Reference Example (Pass-by-reference):
let userOne = { name: "Ahmed" };
let userTwo = userOne; // userTwo copies the MEMORY REFERENCE address!
userTwo.name = "Ali";  // Modifying via userTwo modifies the shared Heap object
console.log(userOne.name); // "Ali"
```

---

## 3. The `typeof` Operator & Historical Oddities

| Value | `typeof` Return Value | Interview Notes / Quirks |
| :--- | :--- | :--- |
| `42` / `3.14` | `"number"` | All numbers (integers & floats) are 64-bit IEEE 754 floats. |
| `"Hello"` | `"string"` | Immutable sequence of UTF-16 code units. |
| `true` / `false` | `"boolean"` | Logical truth value. |
| `undefined` | `"undefined"` | Variable declared but never assigned a value. |
| `null` | **`"object"`** | ⚠️ **Famous Bug in JS**: Original 1995 JS representation bug kept for backwards compatibility. `null` is a primitive, NOT an object! |
| `Symbol('id')`| `"symbol"` | Unique identifier. |
| `100n` | `"bigint"` | Arbitrary precision integer. |
| `{ name: "A" }`| `"object"` | Object literal. |
| `[1, 2, 3]` | **`"object"`** | Arrays are objects under the hood (`Array.isArray(arr)` to test). |
| `function(){}`| **`"function"`** | Functions are objects (callable objects). |

---

## 4. Type Conversion & Coercion (Interview Traps)

JavaScript performs two types of conversion:
1. **Explicit Type Casting**: You manually convert (e.g. `Number("123")`, `String(50)`).
2. **Implicit Type Coercion**: JavaScript automatically converts types behind the scenes.

### Explicit Conversion (`Number()`, `String()`, `Boolean()`)
```javascript
// String to Number
console.log(Number("100"));    // 100
console.log(Number("100abc")); // NaN (Not a Number - type is still "number")
console.log(Number(true));     // 1
console.log(Number(false));    // 0
console.log(Number(null));     // 0
console.log(Number(undefined));// NaN
```

### Implicit Coercion (String Concatenation Rules)
```javascript
console.log("1" + 2);     // "12"  (Number coerced to string)
console.log(1 + "2");     // "12"
console.log("1" + 2 + 2); // "122" (Evaluated left-to-right: "1" + 2 -> "12", "12" + 2 -> "122")
console.log(1 + 2 + "2"); // "32"  (1 + 2 -> 3, 3 + "2" -> "32")

// Subtraction, multiplication, division always coerce to numbers!
console.log("6" - 2);     // 4
console.log("6" * "2");   // 12
console.log("6" / "2");   // 3
console.log("6" - "abc"); // NaN
```

---

## 5. Comparison Operators: Loose (`==`) vs. Strict (`===`)

| Operator | Comparison Type | Does Type Coercion? | Example | Result |
| :---: | :--- | :---: | :--- | :---: |
| `==` | Loose Equality | **Yes** | `5 == "5"` | `true` |
| `===` | Strict Equality | **No** (Checks Value AND Type) | `5 === "5"` | `false` |
| `!=` | Loose Inequality | **Yes** | `5 != "5"` | `false` |
| `!==` | Strict Inequality | **No** | `5 !== "5"` | `true` |

```javascript
// Weird JS Coercion Quirks:
console.log(null == undefined); // true (Special rule in JS spec)
console.log(null === undefined);// false (Different types)
console.log(null > 0);          // false (null coerced to 0: 0 > 0 is false)
console.log(null == 0);         // false (null only equals null/undefined)
console.log(null >= 0);         // true  (null coerced to 0: 0 >= 0 is true)
```

> [!TIP]
> Always use strict equality (`===` and `!==`) to avoid unexpected type coercion bugs.

---

## 6. Strings & Essential String Methods

Strings in JavaScript are immutable (cannot be modified in place; all methods return new strings).

### Modern Template Literals (String Interpolation)
```javascript
const name = "Ahmed";
const role = "Developer";
console.log(`Hello, my name is ${name} and I am a ${role}.`);
```

### Essential String Methods Reference

| Method | What it does | Example | Output |
| :--- | :--- | :--- | :--- |
| `str.length` | Returns character count (property, not method) | `"Ahmed".length` | `5` |
| `str.toUpperCase()` | Converts to uppercase | `"ahmed".toUpperCase()` | `"AHMED"` |
| `str.toLowerCase()` | Converts to lowercase | `"AHMED".toLowerCase()` | `"ahmed"` |
| `str.trim()` | Strips leading & trailing whitespace | `"  hi  ".trim()` | `"hi"` |
| `str.slice(start, end)` | Extracts section (supports negative indices!) | `"JavaScript".slice(0, 4)` | `"Java"` |
| `str.substring(start, end)` | Extracts section (negative indices treated as 0) | `"JavaScript".substring(4, 10)` | `"Script"` |
| `str.includes(sub)` | Checks if substring exists | `"Hello World".includes("World")` | `true` |
| `str.replace(target, replacement)` | Replaces first occurrence | `"foo bar".replace("foo", "baz")` | `"baz bar"` |
| `str.replaceAll(target, replacement)` | Replaces all occurrences | `"a-b-a".replaceAll("a", "x")` | `"x-b-x"` |
| `str.split(separator)` | Splits string into array | `"a,b,c".split(",")` | `["a", "b", "c"]` |

---

## 7. Numbers and the `Math` Object

### Number Methods
```javascript
const num = 123.4567;

// toFixed: rounds to decimal places and returns STRING
console.log(num.toFixed(2)); // "123.46"

// toPrecision: formats to total significant digits
console.log(num.toPrecision(4)); // "123.5"

// toLocaleString: formats with regional comma separations
const largeNumber = 1000000;
console.log(largeNumber.toLocaleString('en-US')); // "1,000,000"
console.log(largeNumber.toLocaleString('en-IN')); // "10,00,000" (Indian system)
```

### Essential `Math` Object Methods
```javascript
console.log(Math.round(4.6)); // 5 (Rounds to nearest integer)
console.log(Math.ceil(4.1));  // 5 (Always rounds UP)
console.log(Math.floor(4.9)); // 4 (Always rounds DOWN)
console.log(Math.abs(-25));   // 25 (Absolute positive value)
console.log(Math.min(2, 5, 1));// 1
console.log(Math.max(2, 5, 1));// 5

// Generating random integer in range [min, max]:
const min = 10;
const max = 20;
const randomInt = Math.floor(Math.random() * (max - min + 1)) + min;
console.log(randomInt); // Integer between 10 and 20
```

---

## 8. Date and Time in JavaScript

JavaScript `Date` counts milliseconds elapsed since **January 1, 1970 UTC (Unix Epoch)**.

```javascript
// 1. Current Date & Time
const now = new Date();

// 2. Specific Date (Note: Month is 0-indexed: 0 = Jan, 11 = Dec!)
const customDate = new Date(2026, 7, 28, 12, 30, 0); // August 28, 2026

// 3. Date Getters
console.log(now.getFullYear()); // e.g. 2026
console.log(now.getMonth());    // 0 to 11
console.log(now.getDate());     // Day of month (1-31)
console.log(now.getDay());      // Day of week (0 = Sunday, 6 = Saturday)
console.log(now.getTime());     // Milliseconds timestamp

// 4. Formatting
console.log(now.toDateString());     // "Fri Aug 28 2026"
console.log(now.toISOString());      // "2026-08-28T07:30:00.000Z"
console.log(now.toLocaleDateString());// "8/28/2026"

// 5. Fast Timestamp for Benchmarking
const start = Date.now(); // Current timestamp in ms
```

---

## 9. Top 7 Interview Questions & Exact Answers

### Q1: What is the Temporal Dead Zone (TDZ)?
> **Answer**: The Temporal Dead Zone is the period between entering a scope and variable declaration where `let` and `const` variables cannot be accessed. Attempting to access them results in a `ReferenceError`. While `let` and `const` are hoisted, they are uninitialized until the line of declaration is executed.

---

### Q2: What is the difference between `null` and `undefined`?
> **Answer**: 
> - `undefined`: A variable has been declared, but has not yet been assigned a value. It is JavaScript's default unassigned state.
> - `null`: An intentional assignment value representing the deliberate absence of any object value. `typeof null === "object"` due to a historical bug in JS.

---

### Q3: Why does `0.1 + 0.2 !== 0.3` in JavaScript?
> **Answer**: JavaScript uses IEEE 754 64-bit floating-point representation. Decimals like `0.1` and `0.2` cannot be represented precisely in binary base-2 fraction, causing a precision drift: `0.1 + 0.2 === 0.30000000000000004`. Solution: Use `Number.EPSILON` or `toFixed()` for comparisons.

---

### Q4: What is the difference between Primitive and Reference types in memory?
> **Answer**:
> - **Primitives** (Number, String, Boolean, null, undefined, Symbol, BigInt) are stored directly on the **Stack**. Variables hold the actual data, and assignment creates an independent copy (pass-by-value).
> - **Reference types** (Objects, Arrays, Functions) are allocated on the **Heap**. Variables on the stack hold a pointer/memory address pointing to the heap object. Assignment copies the reference pointer, so both variables modify the same underlying object.

---

### Q5: What is the output of `"b" + "a" + + "a" + "a"`?
> **Answer**: `"baNaNa"`. The unary plus `+ "a"` attempts to coerce the string `"a"` into a number, which produces `NaN`. Concatenated with the other letters, it becomes `"b" + "a" + "NaN" + "a"` = `"baNaNa"`.

---

### Q6: How does `slice()` differ from `substring()`?
> **Answer**: `slice()` accepts negative indices (counting backwards from the end of the string, e.g. `str.slice(-3)`). `substring()` treats negative indices as `0` and swaps start and end arguments if `start > end`.

---

### Q7: Why is `typeof NaN === "number"`?
> **Answer**: `NaN` stands for "Not-a-Number", but according to the IEEE 754 floating-point specification, it is a numeric data type representing an undefined or unrepresentable numerical result (such as `0 / 0` or `Math.sqrt(-1)`).
