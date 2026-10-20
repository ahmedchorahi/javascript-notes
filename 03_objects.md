# JavaScript Objects: Master Interview Guide

An **Object** in JavaScript is a non-primitive, composite data type used to store collections of key-value pairs. Keys (properties) are strings or Symbols, while values can be any valid JavaScript type (primitives, other objects, or functions/methods).

---

## 1. Object Creation Patterns

```text
                  OBJECT CREATION APPROACHES
+--------------------+------------------------------------+--------------------------+
| Pattern            | Syntax                             | Best Use Case            |
+--------------------+------------------------------------+--------------------------+
| 1. Object Literal  | const obj = { key: "val" };        | Standard, clean, common  |
| 2. Object Constructor | const obj = new Object();       | Verbose (Rarely used)    |
| 3. Object.create() | const obj = Object.create(proto);  | Prototype-based creation |
| 4. Constructor Fn  | function User(n){ this.n = n; }    | Object blueprints        |
| 5. ES6 Classes     | class User { ... }                 | Modern OOP               |
+--------------------+------------------------------------+--------------------------+
```

```javascript
// 1. Object Literal (Preferred)
const user = {
  name: "Ahmed",
  age: 24,
  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
};

// 2. Object.create(prototypeObject) -> Sets prototype explicitly
const proto = { role: "Student" };
const student = Object.create(proto);
student.name = "Ali";
console.log(student.role); // "Student" (Inherited from prototype)
```

---

## 2. Property Access: Dot Notation vs. Bracket Notation

```javascript
const user = {
  name: "Ahmed",
  "full address": "Karachi, Pakistan",
  10: "Numeric Key"
};

// 1. Dot Notation (Used when property is a valid JS identifier)
console.log(user.name); // "Ahmed"

// 2. Bracket Notation (MANDATORY for dynamic keys, spaces, and symbols)
console.log(user["full address"]); // "Karachi, Pakistan"
console.log(user[10]);             // "Numeric Key" (Key coerced to string "10")

const dynamicKey = "name";
console.log(user[dynamicKey]);      // "Ahmed"
console.log(user.dynamicKey);       // undefined (Looks literally for a property named "dynamicKey")
```

---

## 3. Symbols as Object Keys (Guaranteed Unique / Hidden Keys)

`Symbol` creates immutable and guaranteed unique primitive keys that prevent property collisions in libraries:

```javascript
const uniqueId = Symbol("id");

const employee = {
  name: "Ahmed",
  [uniqueId]: "EMP_12345" // Computed property syntax using square brackets []
};

// Accessing the symbol property:
console.log(employee[uniqueId]); // "EMP_12345"

// ⚠️ Symbols are excluded from normal object iterations:
console.log(Object.keys(employee)); // ["name"] (Symbol is invisible!)
for (let key in employee) {
  console.log(key); // Only prints "name"
}

// To retrieve Symbols explicitly:
console.log(Object.getOwnPropertySymbols(employee)); // [ Symbol(id) ]
```

---

## 4. Merging Objects: Spread (`...`) vs. `Object.assign()`

```javascript
const target = { a: 1, b: 2 };
const source1 = { b: 99, c: 3 }; // 'b' will overwrite earlier 'b'
const source2 = { d: 4 };

// Method 1: Spread Operator (Modern Standard)
const mergedSpread = { ...target, ...source1, ...source2 };
console.log(mergedSpread); // { a: 1, b: 99, c: 3, d: 4 }

// Method 2: Object.assign(target, ...sources)
// Mutates the first argument! Pass an empty {} to create a fresh object.
const mergedAssign = Object.assign({}, target, source1, source2);
console.log(mergedAssign); // { a: 1, b: 99, c: 3, d: 4 }
```

> [!WARNING]
> Both Spread and `Object.assign()` perform **shallow copies**. Nested objects share memory references between copies!

---

## 5. Object Inspection Methods: `keys`, `values`, `entries`, `fromEntries`

```javascript
const product = { id: 101, title: "Laptop", price: 1200 };

// 1. Object.keys() -> Array of string keys
console.log(Object.keys(product)); // ['id', 'title', 'price']

// 2. Object.values() -> Array of property values
console.log(Object.values(product)); // [101, 'Laptop', 1200]

// 3. Object.entries() -> Array of [key, value] pairs (Great for loops!)
console.log(Object.entries(product)); 
// [ ['id', 101], ['title', 'Laptop'], ['price', 1200] ]

// 4. Object.fromEntries() -> Transforms [key, value] array back into an Object
const entries = [['name', 'Ahmed'], ['age', 24]];
console.log(Object.fromEntries(entries)); // { name: 'Ahmed', age: 24 }
```

---

## 6. Checking Property Existence: `hasOwn` vs. `in`

| Method | Checks Own Properties? | Checks Prototype Chain? | Safe on `Object.create(null)`? |
| :--- | :---: | :---: | :---: |
| **`Object.hasOwn(obj, prop)`** *(ES2022 - Best)* | **Yes** | **No** | **Yes** |
| **`obj.hasOwnProperty(prop)`** | **Yes** | **No** | ❌ Fails on objects without prototype |
| **`'prop' in obj`** | **Yes** | **Yes** | **Yes** |

```javascript
const parent = { inheritedProp: "I am from prototype" };
const child = Object.create(parent);
child.ownProp = "I belong to child";

// Object.hasOwn vs 'in':
console.log(Object.hasOwn(child, "ownProp"));        // true
console.log(Object.hasOwn(child, "inheritedProp"));  // false (Filters out prototypes!)
console.log("inheritedProp" in child);               // true (Traverses prototype chain!)
```

---

## 7. Levels of Object Immutability

JavaScript provides three progressive levels to protect objects from modification:

```text
               OBJECT IMMUTABILITY HIERARCHY
+---------------------------+------------+------------+--------------------+
| Method                    | Add Props? | Delete?    | Modify Values?     |
+---------------------------+------------+------------+--------------------+
| Object.preventExtensions()| ❌ No       | ✅ Yes     | ✅ Yes             |
| Object.seal()             | ❌ No       | ❌ No      | ✅ Yes             |
| Object.freeze()           | ❌ No       | ❌ No      | ❌ No (Read-Only)  |
+---------------------------+------------+------------+--------------------+
```

```javascript
const config = { apiEndpoint: "https://api.v1", port: 3000 };

// Object.freeze makes an object completely read-only (shallow)
Object.freeze(config);
config.port = 8080;   // Silently fails (or throws TypeError in strict mode)
config.newProp = "x"; // Silently fails
delete config.port;   // Silently fails
console.log(config);  // { apiEndpoint: 'https://api.v1', port: 3000 }

console.log(Object.isFrozen(config)); // true
```

> [!NOTE]
> `Object.freeze()` is **shallow**. If the object contains nested objects, nested properties can still be modified unless you deep-freeze recursively.

---

## 8. Object Destructuring (Clean Data Extraction)

```javascript
const course = {
  courseName: "JavaScript Interview Prep",
  price: 999,
  instructor: {
    fullName: "Hitesh Choudhary",
    platform: "YouTube"
  }
};

// 1. Basic Destructuring & Renaming
const { courseName: title, price } = course;
console.log(title); // "JavaScript Interview Prep"
console.log(price); // 999

// 2. Default Values (Used if property is undefined)
const { duration = "10 Hours" } = course;
console.log(duration); // "10 Hours"

// 3. Nested Destructuring
const { instructor: { fullName } } = course;
console.log(fullName); // "Hitesh Choudhary"
```

---

## 9. Top 7 Interview Questions & Exact Answers

### Q1: What is the difference between `Object.freeze()` and `Object.seal()`?
> **Answer**: 
> - `Object.seal()` prevents adding new properties and deleting existing properties, but **allows modifying** existing property values.
> - `Object.freeze()` prevents adding, deleting, **and modifying** existing values, making all own properties completely read-only.

---

### Q2: Why should you use `Object.hasOwn(obj, prop)` over `obj.hasOwnProperty(prop)`?
> **Answer**: `obj.hasOwnProperty()` can fail if the object does not inherit from `Object.prototype` (e.g. `Object.create(null)` has no `hasOwnProperty` method), or if an object property intentionally overwrites the name `hasOwnProperty: false`. `Object.hasOwn()` is a safe static method introduced in ES2022 that works on all objects.

---

### Q3: How do you achieve a true Deep Copy of an object?
> **Answer**: Use `structuredClone(obj)` (built-in modern standard). Older workarounds include `JSON.parse(JSON.stringify(obj))` (which drops functions, `undefined`, and Symbols) or recursive deep copy utilities (Lodash `_.cloneDeep`).

---

### Q4: Why are Symbols useful as object keys?
> **Answer**: Symbols are guaranteed to be unique primitives. Using a Symbol as a key guarantees no chance of naming collisions, and the property is hidden from standard `Object.keys()` and `for...in` loops, providing pseudo-private properties.

---

### Q5: How do you check if an object is completely empty `{}`?
> **Answer**: `Object.keys(obj).length === 0 && obj.constructor === Object`. Comparing `obj === {}` fails because two separate objects in memory never share the same reference address.

---

### Q6: What is the difference between shallow copy and deep copy in objects?
> **Answer**: A shallow copy (`{...obj}` or `Object.assign({}, obj)`) only duplicates top-level properties. Any nested objects or arrays are copied by reference, meaning modifications to nested data affect both copies. A deep copy recursively duplicates every nested layer, creating an entirely independent copy in memory.

---

### Q7: What does `Object.entries()` return and what is it used for?
> **Answer**: It returns an array of the object's own enumerable string-keyed `[key, value]` pairs. It is commonly used to iterate over objects using `for...of`, or to transform objects with array methods like `.map()` or `.filter()`, and rebuild them with `Object.fromEntries()`.
