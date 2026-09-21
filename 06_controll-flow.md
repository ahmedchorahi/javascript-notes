# Control Flow in JavaScript

Control flow is the order in which individual statements, instructions, or function calls are executed or evaluated in a program. By default, JavaScript executes code sequentially from top to bottom, but we can alter this flow using conditional statements and control structures.

---

## 1) If-Else Statement

The `if-else` statement executes a block of code if a specified condition evaluates to `true`. If the condition evaluates to `false`, the `else` block executes.

```js
const temperature = 41;

if (temperature < 50) {
    console.log("Temperature is less than 50");
} else {
    console.log("Temperature is greater than 50");
}
// Output: Temperature is less than 50
```

### Short-hand notation (Implicit Scope)
```js
const balance = 1000;
// Single-line execution without curly braces (use sparingly for readability)
if (balance > 500) console.log("Account balance is sufficient");
```

---

## 2) Comparison Operators

Comparison operators compare two values and return a boolean (`true` or `false`).

| Operator | Meaning | Example | Result |
| :--- | :--- | :--- | :--- |
| `==` | Loose equality (checks value with type coercion) | `2 == "2"` | `true` |
| `===` | Strict equality (checks value AND data type) | `2 === "2"` | `false` |
| `!=` | Loose inequality | `2 != "2"` | `false` |
| `!==` | Strict inequality | `2 !== "2"` | `true` |
| `>` | Greater than | `5 > 3` | `true` |
| `<` | Less than | `5 < 3` | `false` |
| `>=` | Greater than or equal to | `5 >= 5` | `true` |
| `<=` | Less than or equal to | `4 <= 5` | `true` |

### Interview Point: `==` vs `===`
- `==` performs **type coercion** (converts operands to the same type before comparing).
- `===` checks both **value and type** without coercion. Always prefer `===` in modern JavaScript to avoid unexpected bugs.

---

## 3) Logical Operators

Logical operators are used to combine multiple conditions.

| Operator | Name | Description | Example |
| :--- | :--- | :--- | :--- |
| `&&` | Logical AND | Returns `true` only if **all** conditions are true | `(5 > 2 && 10 > 5)` &rarr; `true` |
| `\|\|` | Logical OR | Returns `true` if **at least one** condition is true | `(5 > 10 \|\| 10 > 5)` &rarr; `true` |
| `!` | Logical NOT | Inverts the boolean value | `!(5 > 2)` &rarr; `false` |

### Example
```js
const userLoggedIn = true;
const hasDebitCard = true;
const loggedInFromGoogle = false;
const loggedInFromEmail = true;

// AND: All conditions must be true
if (userLoggedIn && hasDebitCard) {
    console.log("Allowed to buy course");
}

// OR: At least one condition must be true
if (loggedInFromGoogle || loggedInFromEmail) {
    console.log("User logged in");
}
```

---

## 4) If - Else If - Else Ladder

Used when you need to check multiple conditions sequentially.

```js
const balance = 750;

if (balance < 500) {
    console.log("Less than 500");
} else if (balance < 750) {
    console.log("Less than 750");
} else if (balance < 900) {
    console.log("Less than 900");
} else {
    console.log("Less than 1200");
}
// Output: Less than 900
```

---

## 5) Switch Statement

A cleaner alternative to multiple `if-else if` statements when checking a single value against several known constants.

### Syntax
```js
switch (key) {
    case value1:
        // code to execute
        break;
    case value2:
        // code to execute
        break;
    default:
        // default code if no case matches
        break;
}
```

### Example
```js
const month = 3;

switch (month) {
    case 1:
        console.log("January");
        break;
    case 2:
        console.log("February");
        break;
    case 3:
        console.log("March");
        break;
    case 4:
        console.log("April");
        break;
    default:
        console.log("Default case matched");
        break;
}
// Output: March
```

### Interview Point: Why is `break` necessary?
If you omit the `break` keyword, JavaScript continues executing all subsequent cases until it finds a `break` or hits the end of the switch block (this behavior is called **fall-through**), regardless of whether subsequent conditions match (except `default` in some patterns).

---

## 6) Truthy and Falsy Values

In JavaScript, any value evaluated inside a boolean context (like an `if` condition) is coerced into either `true` or `false`.

### Falsy Values (Only 8 values in JavaScript)
Values that always evaluate to `false`:
1. `false`
2. `0`
3. `-0`
4. `0n` (BigInt zero)
5. `""` (empty string)
6. `null`
7. `undefined`
8. `NaN` (Not a Number)

### Truthy Values
Everything else is considered truthy! Notice these surprising truthy values:
- `"0"` (string containing zero)
- `'false'` (string containing "false")
- `" "` (string containing whitespace)
- `[]` (empty array)
- `{}` (empty object)
- `function(){}` (empty function)

### How to correctly check for empty Array & Object
```js
// 1. Checking an empty array
const emptyArr = [];
if (emptyArr.length === 0) {
    console.log("Array is empty");
}

// 2. Checking an empty object
const emptyObj = {};
if (Object.keys(emptyObj).length === 0) {
    console.log("Object is empty");
}
```

### Quick Revision: Loose Equality Surprises
```js
false == 0      // true
false == ''     // true
0 == ''         // true
```

---

## 7) Nullish Coalescing Operator (`??`)

The Nullish Coalescing Operator (`??`) is a logical operator that returns its right-hand operand when its left-hand operand is **`null`** or **`undefined`**, otherwise returns its left-hand operand.

### Why do we need `??` when we have `||`?
- `||` checks for **any falsy value** (`false`, `0`, `""`, `null`, `undefined`, `NaN`).
- `??` **only** checks for `null` and `undefined`. It preserves valid falsy values like `0` or `""`.

### Example
```js
let val1;

val1 = 5 ?? 10;          // Output: 5
val1 = null ?? 10;       // Output: 10
val1 = undefined ?? 15;  // Output: 15
val1 = null ?? 10 ?? 20; // Output: 10 (returns first defined value)

console.log(val1);

// Comparison with ||
const score = 0;
console.log(score || 50); // Output: 50 (0 is treated as falsy!)
console.log(score ?? 50); // Output: 0  (0 is preserved because it is not null/undefined)
```

> [!NOTE]
> Commonly used when reading data from APIs or databases where a field might return `null` or `undefined`, allowing safe fallback defaults.

---

## 8) Ternary Operator

A concise shorthand for an `if-else` statement.

### Syntax
```js
condition ? expressionIfTrue : expressionIfFalse;
```

### Example
```js
const iceTeaPrice = 100;

iceTeaPrice <= 80 ? console.log("Less than 80") : console.log("More than 80");
// Output: More than 80

// Storing the result in a variable
const userAge = 19;
const canVote = userAge >= 18 ? "Eligible to vote" : "Not eligible";
console.log(canVote); // Output: Eligible to vote
```

---

## Summary / Quick Revision Sheet

| Concept | Syntax / Key takeaway |
| :--- | :--- |
| **Strict Equality** | `===` compares both value and type (always prefer over `==`). |
| **Logical AND (`&&`)** | All conditions must be `true`. |
| **Logical OR (`\|\|`)** | Any one condition must be `true`. |
| **Switch** | Use `break` after each case to prevent fall-through. |
| **Falsy Values** | `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, `NaN`. |
| **Empty Array check** | `arr.length === 0` |
| **Empty Object check** | `Object.keys(obj).length === 0` |
| **Nullish Coalescing (`??`)** | Fallback only on `null` or `undefined`. |
| **Ternary Operator** | `condition ? trueValue : falseValue` |
