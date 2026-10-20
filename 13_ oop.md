# Object-Oriented JavaScript & Prototypes: Master Interview Guide

JavaScript is primarily a **prototype-based, multi-paradigm language**. While ES6 (ECMAScript 2015) introduced the `class` syntax, JavaScript classes are fundamentally **syntactic sugar** built on top of JavaScript's existing prototype-based inheritance model.

---

## 1. Core Concepts: Prototypes vs. Traditional Classes

```text
               TRADITIONAL CLASS (Java / C++) vs. JAVASCRIPT PROTOTYPE
+------------------------------------+------------------------------------+
| Classical OOP (Java / C++)         | Prototypal OOP (JavaScript)        |
+------------------------------------+------------------------------------+
| Classes are rigid blueprints.      | Objects are created directly.      |
| Instances copy members from class. | Objects inherit directly from other|
|                                    | live objects via prototype links.  |
| Class-instance divide is strict.   | Everything is an object or delegates|
|                                    | to Object.prototype.               |
+------------------------------------+------------------------------------+
```

### What is an Object?
An **Object** in JavaScript is a collection of key-value pairs (properties) and functions (methods). Almost everything in JavaScript—including Arrays, Functions, and Dates—is an object or behaves like one.

---

## 2. Core Building Blocks of OOP in JavaScript

```text
+-------------------+---------------------------------------------------------+
| Part              | Description                                             |
+-------------------+---------------------------------------------------------+
| 1. Object Literal | Simple, direct creation of a single object: const u = {}|
| 2. Constructor Fn | Blueprint function invoked with 'new' to spawn objects  |
| 3. Prototypes     | Shared memory mechanism for methods across instances    |
| 4. Classes        | ES6 syntactic sugar organizing constructors & prototypes|
| 5. Instances      | Concrete object created using 'new' or Object.create()  |
+-------------------+---------------------------------------------------------+
```

### 1. Object Literal (Single Use)
```javascript
const user = {
  username: "ahmed",
  loginCount: 8,
  signIn: true,
  getUserDetails() {
    console.log(`Username: ${this.username}, Logins: ${this.loginCount}`);
  }
};

user.getUserDetails(); // "Username: ahmed, Logins: 8"
```

### 2. Constructor Functions (Blueprint for Multiple Instances)
```javascript
function User(username, loginCount, signIn) {
  // 'this' refers to the newly created instance
  this.username = username;
  this.loginCount = loginCount;
  this.signIn = signIn;

  // Implicitly returns 'this'
}

const userOne = new User("Ahmed", 12, true);
const userTwo = new User("Ali", 5, false);

console.log(userOne.username); // "Ahmed"
console.log(userTwo.username); // "Ali"
```

---

## 3. The `new` Keyword: What Happens Under the Hood?

> [!IMPORTANT]
> **Essential Technical Interview Question**:
> *"Explain step-by-step what the `new` keyword actually does in JavaScript."*

When you execute `new ConstructorFunction(args)`, the JavaScript engine performs **4 sequential steps**:

```text
                           new User("Ahmed", 12)
                                     |
               1. CREATE: Allocates fresh empty object: {}
                                     |
               2. LINK PROTOTYPE: Sets newObj.__proto__ = Constructor.prototype
                                     |
               3. BIND 'this': Executes Constructor with 'this' bound to newObj
                                     |
               4. RETURN: Automatically returns newObj (unless explicit object returned)
```

1. **Creates a new empty object**: A fresh memory space `{}` is allocated in the Heap.
2. **Links the Prototype**: The internal `[[Prototype]]` (accessible as `__proto__`) of the new object is linked to the constructor function's `prototype` object (`newObj.__proto__ === User.prototype`).
3. **Binds `this` & Executes**: The constructor function is executed with `this` pointing to the newly created object. Properties assigned to `this` are attached to the new object.
4. **Returns the Object**: Returns the newly constructed object automatically (unless the constructor explicitly returns its own non-primitive object).

---

## 4. Prototypal Behavior & The Prototype Chain

Every function in JavaScript automatically receives a special property called **`.prototype`**. When you instantiate an object using `new`, the object gets an internal link `[[Prototype]]` (`__proto__`) to that constructor's `.prototype`.

### The Prototype Chain Architecture:
```text
           Array Instance [1, 2]
                     | (__proto__)
                     v
              Array.prototype (.map, .filter, .push)
                     | (__proto__)
                     v
             Object.prototype (.hasOwnProperty, .toString)
                     | (__proto__)
                     v
                    null (End of the Chain)
```

```text
    Function / String / Array  ──>  Object.prototype  ──>  null
```

```javascript
// Adding a shared method to User prototype (Memory Efficient!)
User.prototype.greet = function() {
  console.log(`Hello, my name is ${this.username}`);
};

// Both instances share ONE copy of greet in memory!
userOne.greet(); // "Hello, my name is Ahmed"
userTwo.greet(); // "Hello, my name is Ali"

console.log(userOne.__proto__ === User.prototype); // true
console.log(User.prototype.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__); // null (The ultimate root)
```

> [!TIP]
> **Why define methods on `.prototype` instead of inside the constructor?**
> If defined inside the constructor (`this.greet = function...`), every single instance duplicates the function in memory. Defining it on `.prototype` stores the method **once** in memory, and all instances share it.

---

## 5. The 4 Pillars of Object-Oriented Programming in JavaScript

```text
+-------------------------------------------------------------------------------+
|                             THE 4 PILLARS OF OOP                              |
|                                                                               |
|  1. ENCAPSULATION           2. ABSTRACTION          3. INHERITANCE            |
|  Bundling state & methods;  Hiding complexity;      Reusing features from a   |
|  hiding internal private    exposing clean, high-   parent blueprint via      |
|  variables.                 level interface.        extends / prototype.      |
|                                                                               |
|                                4. POLYMORPHISM                                |
|             Ability for different classes to share the same method            |
|             interface while executing their own distinct behavior.            |
+-------------------------------------------------------------------------------+
```

### 1. Encapsulation (Private Fields `#`)
```javascript
class BankAccount {
  // Private field (introduced in ES2022)
  #balance;

  constructor(initialDeposit) {
    this.#balance = initialDeposit;
  }

  deposit(amount) {
    if (amount > 0) this.#balance += amount;
  }

  getBalance() {
    return `Your balance is $${this.#balance}`;
  }
}

const account = new BankAccount(100);
account.deposit(50);
console.log(account.getBalance()); // "Your balance is $150"
// console.log(account.#balance);   // ❌ SyntaxError: Private field '#balance' must be declared
```

### 2. Abstraction
Exposing simple methods like `.startEngine()` while hiding complex internal mechanics:
```javascript
class Car {
  startEngine() {
    this.#injectFuel();
    this.#igniteSpark();
    console.log("Engine running smoothly!");
  }

  #injectFuel() { /* Complex internal logic */ }
  #igniteSpark() { /* Complex internal logic */ }
}

const myCar = new Car();
myCar.startEngine(); // Clean interface, details hidden
```

### 3. Inheritance (`extends` and `super`)
```javascript
class Vehicle {
  constructor(brand) {
    this.brand = brand;
  }
  honk() {
    console.log(`${this.brand} says: Beep beep!`);
  }
}

class ElectricCar extends Vehicle {
  constructor(brand, batteryCapacity) {
    super(brand); // Calls the parent Vehicle constructor
    this.batteryCapacity = batteryCapacity;
  }
  charge() {
    console.log(`Charging ${this.brand} (${this.batteryCapacity} kWh)...`);
  }
}

const tesla = new ElectricCar("Tesla", 100);
tesla.honk();   // Inherited: "Tesla says: Beep beep!"
tesla.charge(); // Own method: "Charging Tesla (100 kWh)..."
```

### 4. Polymorphism (Many Forms)
```javascript
class Shape {
  draw() {
    console.log("Drawing a generic shape");
  }
}

class Circle extends Shape {
  draw() {
    console.log("Drawing a Circle ⭕"); // Overrides parent draw()
  }
}

class Square extends Shape {
  draw() {
    console.log("Drawing a Square ⬛"); // Overrides parent draw()
  }
}

const shapes = [new Circle(), new Square()];
shapes.forEach((shape) => shape.draw());
// Circle ⭕
// Square ⬛
```

---

## 6. Controlling Function Context: `call()`, `apply()`, and `bind()`

Functions can borrow methods or manually specify what `this` points to:

| Method | Execution | Argument Format | Return Value |
| :--- | :--- | :--- | :--- |
| **`fn.call(thisArg, arg1, arg2)`** | **Immediate** | Comma-separated list | Result of the function |
| **`fn.apply(thisArg, [argsArray])`**| **Immediate** | Single Array of arguments | Result of the function |
| **`fn.bind(thisArg, arg1, arg2)`** | **Deferred** | Comma-separated list | A **brand new function** with bound `this` |

```javascript
const person = {
  fullName(city, country) {
    return `${this.firstName} ${this.lastName} from ${city}, ${country}`;
  }
};

const userA = { firstName: "Ahmed", lastName: "Chorahi" };

// 1. call: immediate invocation with comma arguments
console.log(person.fullName.call(userA, "Karachi", "Pakistan"));
// "Ahmed Chorahi from Karachi, Pakistan"

// 2. apply: immediate invocation with array arguments
console.log(person.fullName.apply(userA, ["Lahore", "Pakistan"]));
// "Ahmed Chorahi from Lahore, Pakistan"

// 3. bind: returns a new function to be called later
const boundFunc = person.fullName.bind(userA, "Islamabad", "Pakistan");
console.log(boundFunc()); // "Ahmed Chorahi from Islamabad, Pakistan"
```

---

## 7. ES6 Classes, Getters, and Setters

```javascript
class UserAccount {
  constructor(email, password) {
    this.email = email;
    this.password = password; // Triggers the setter!
  }

  // Getter: intercepts reading the property
  get password() {
    return `${this._password.toUpperCase()}@encrypted`;
  }

  // Setter: intercepts updating the property
  // ⚠️ NOTE: Use an internal variable (e.g. _password) to prevent infinite recursion!
  set password(value) {
    this._password = value;
  }
}

const userObj = new UserAccount("ahmed@test.com", "abc123");
console.log(userObj.password); // "ABC123@ENCRYPTED"
```

---

## 8. Lexical Scope & Closures (The Foundation of JS Encapsulation)

* **Lexical Scope**: Inner functions have access to variables declared in their outer parent scopes.
* **Closure**: A function bundled together with its lexical environment. A closure gives an inner function access to an outer function's scope even **after the outer function has returned and popped off the Call Stack**.

```javascript
function createCounter() {
  let count = 0; // Private variable trapped inside the closure

  return {
    increment() {
      count++;
      return count;
    },
    decrement() {
      count--;
      return count;
    },
    getCount() {
      return count;
    }
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
// count is completely private and cannot be modified directly from outside!
```

---

## 9. Chai aur Code Complete Video Roadmap & Timestamps

> [!NOTE]
> **Authoritative Video Reference**: [Chai aur Code: JavaScript Full Course](https://youtu.be/_TjtAyMkiTI?si=RHS41XyzpTNOF3YP)
> 
> Detailed timestamp index for targeted revision:

| Timestamp | Topic & Focus Area | Key Concepts Covered |
| :---: | :--- | :--- |
| **`07:11:41`** | **Magic of Prototype** | Prototypal inheritance, prototype chain, `prototype` vs `__proto__`. |
| **`07:59:47`** | **Call and `this`** | Explicit binding, passing execution context, constructor inheritance. |
| **`08:12:53`** | **Class Constructor** | ES6 classes, `super()`, `extends`, inheritance syntax under the hood. |
| **`08:39:29`** | **Bind** | Hard binding, event handler contexts, delayed execution. |
| **`08:51:07`** | **Now You Know Objects** | Property descriptors (`Object.getOwnPropertyDescriptor`, `writable`, `enumerable`, `configurable`, `Math.PI` override trick). |
| **`09:10:53`** | **Getters and Setters** | Class `get` & `set`, object literal getters/setters, memory call stack overflow trap. |
| **`09:42:16`** | **Lexical Scope and Closure**| Scope chain, closures in memory, practical closure examples. |
| **`10:09:04`** | **JavaScript Ends with a Story** | Summary of modern JS, engineering insights, and future pathways. |

---

## 10. Top 7 Interview Questions & Exact Answers

### Q1: Is JavaScript a class-based or prototype-based language?
> **Answer**: JavaScript is a prototype-based language. Even though ES6 introduced the `class` keyword, classes are syntactic sugar over existing prototype-based inheritance and constructor functions. In JS, objects inherit directly from other objects via prototype links, rather than copying instances from separate class declarations.

---

### Q2: What is the difference between `.prototype` and `__proto__`?
> **Answer**:
> - **`.prototype`**: A property that exists on **functions** (specifically constructor functions). It defines the blueprint of properties and methods that will be shared by instances created by this function with `new`.
> - **`__proto__`** (or `[[Prototype]]`): An internal reference that exists on every **object instance**, pointing to the prototype object from which it inherits.

---

### Q3: What is the difference between `call()`, `apply()`, and `bind()`?
> **Answer**:
> - `call()` invokes the function immediately with an explicit `this` and arguments passed individually (`fn.call(ctx, a, b)`).
> - `apply()` invokes the function immediately with an explicit `this` and arguments passed as an array (`fn.apply(ctx, [a, b])`).
> - `bind()` does not invoke the function immediately; it returns a new bound function with `this` permanently set, which can be invoked later.

---

### Q4: Why can't you overwrite `Math.PI` in JavaScript?
> **Answer**: `Math.PI` has its property descriptor flags configured as `writable: false` and `configurable: false`. Checking `Object.getOwnPropertyDescriptor(Math, "PI")` reveals that the property is permanently locked against reassignment and reconfiguration.

---

### Q5: How do closures achieve Encapsulation?
> **Answer**: When an outer function executes and returns an inner function or object with methods, the outer function's local variables remain accessible only to those returned inner functions via their lexical scope environment. Outside code cannot directly read or alter those variables, achieving true private state.

---

### Q6: What does the Prototype Chain terminate with?
> **Answer**: The prototype chain terminates with **`null`**. `Object.prototype.__proto__ === null`. When JavaScript searches for a property and reaches `null` without finding it, it returns `undefined`.

---

### Q7: Why does a Setter cause an infinite recursion ("Maximum call stack size exceeded") if not using an internal variable?
> **Answer**: Inside a setter like `set password(val) { this.password = val; }`, assigning `this.password = val` calls the setter again, which calls the setter again indefinitely. To avoid this recursive loop, developers use a backing variable with a distinct name, such as `this._password = val`.

### 4. Polymorphism (Many Forms)
```javascript
class Shape {
  draw() {
    console.log("Drawing a generic shape");
  }
}

class Circle extends Shape {
  draw() {
    console.log("Drawing a Circle ⭕"); // Overrides parent draw()
  }
}

class Square extends Shape {
  draw() {
    console.log("Drawing a Square ⬛"); // Overrides parent draw()
  }
}

const shapes = [new Circle(), new Square()];
shapes.forEach((shape) => shape.draw());
// Circle ⭕
// Square ⬛
```

---

## 6. Controlling Function Context: `call()`, `apply()`, and `bind()`

Functions can borrow methods or manually specify what `this` points to:

| Method | Execution | Argument Format | Return Value |
| :--- | :--- | :--- | :--- |
| **`fn.call(thisArg, arg1, arg2)`** | **Immediate** | Comma-separated list | Result of the function |
| **`fn.apply(thisArg, [argsArray])`**| **Immediate** | Single Array of arguments | Result of the function |
| **`fn.bind(thisArg, arg1, arg2)`** | **Deferred** | Comma-separated list | A **brand new function** with bound `this` |

```javascript
const person = {
  fullName(city, country) {
    return `${this.firstName} ${this.lastName} from ${city}, ${country}`;
  }
};

const userA = { firstName: "Ahmed", lastName: "Chorahi" };

// 1. call: immediate invocation with comma arguments
console.log(person.fullName.call(userA, "Karachi", "Pakistan"));
// "Ahmed Chorahi from Karachi, Pakistan"

// 2. apply: immediate invocation with array arguments
console.log(person.fullName.apply(userA, ["Lahore", "Pakistan"]));
// "Ahmed Chorahi from Lahore, Pakistan"

// 3. bind: returns a new function to be called later
const boundFunc = person.fullName.bind(userA, "Islamabad", "Pakistan");
console.log(boundFunc()); // "Ahmed Chorahi from Islamabad, Pakistan"
```

---

## 7. ES6 Classes, Getters, and Setters

```javascript
class UserAccount {
  constructor(email, password) {
    this.email = email;
    this.password = password; // Triggers the setter!
  }

  // Getter: intercepts reading the property
  get password() {
    return `${this._password.toUpperCase()}@encrypted`;
  }

  // Setter: intercepts updating the property
  // ⚠️ NOTE: Use an internal variable (e.g. _password) to prevent infinite recursion!
  set password(value) {
    this._password = value;
  }
}

const userObj = new UserAccount("ahmed@test.com", "abc123");
console.log(userObj.password); // "ABC123@ENCRYPTED"
```

---

## 8. Lexical Scope & Closures (The Foundation of JS Encapsulation)

* **Lexical Scope**: Inner functions have access to variables declared in their outer parent scopes.
* **Closure**: A function bundled together with its lexical environment. A closure gives an inner function access to an outer function's scope even **after the outer function has returned and popped off the Call Stack**.

```javascript
function createCounter() {
  let count = 0; // Private variable trapped inside the closure

  return {
    increment() {
      count++;
      return count;
    },
    decrement() {
      count--;
      return count;
    },
    getCount() {
      return count;
    }
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
// count is completely private and cannot be modified directly from outside!
```

---

## 9. Chai aur Code Complete Video Roadmap & Timestamps

> [!NOTE]
> **Authoritative Video Reference**: [Chai aur Code: JavaScript Full Course](https://youtu.be/_TjtAyMkiTI?si=RHS41XyzpTNOF3YP)
> 
> Detailed timestamp index for targeted revision:

| Timestamp | Topic & Focus Area | Key Concepts Covered |
| :---: | :--- | :--- |
| **`07:11:41`** | **Magic of Prototype** | Prototypal inheritance, prototype chain, `prototype` vs `__proto__`. |
| **`07:59:47`** | **Call and `this`** | Explicit binding, passing execution context, constructor inheritance. |
| **`08:12:53`** | **Class Constructor** | ES6 classes, `super()`, `extends`, inheritance syntax under the hood. |
| **`08:39:29`** | **Bind** | Hard binding, event handler contexts, delayed execution. |
| **`08:51:07`** | **Now You Know Objects** | Property descriptors (`Object.getOwnPropertyDescriptor`, `writable`, `enumerable`, `configurable`, `Math.PI` override trick). |
| **`09:10:53`** | **Getters and Setters** | Class `get` & `set`, object literal getters/setters, memory call stack overflow trap. |
| **`09:42:16`** | **Lexical Scope and Closure**| Scope chain, closures in memory, practical closure examples. |
| **`10:09:04`** | **JavaScript Ends with a Story** | Summary of modern JS, engineering insights, and future pathways. |

---

## 10. Top 7 Interview Questions & Exact Answers

### Q1: Is JavaScript a class-based or prototype-based language?
> **Answer**: JavaScript is a prototype-based language. Even though ES6 introduced the `class` keyword, classes are syntactic sugar over existing prototype-based inheritance and constructor functions. In JS, objects inherit directly from other objects via prototype links, rather than copying instances from separate class declarations.

---

### Q2: What is the difference between `.prototype` and `__proto__`?
> **Answer**:
> - **`.prototype`**: A property that exists on **functions** (specifically constructor functions). It defines the blueprint of properties and methods that will be shared by instances created by this function with `new`.
> - **`__proto__`** (or `[[Prototype]]`): An internal reference that exists on every **object instance**, pointing to the prototype object from which it inherits.

---

### Q3: What is the difference between `call()`, `apply()`, and `bind()`?
> **Answer**:
> - `call()` invokes the function immediately with an explicit `this` and arguments passed individually (`fn.call(ctx, a, b)`).
> - `apply()` invokes the function immediately with an explicit `this` and arguments passed as an array (`fn.apply(ctx, [a, b])`).
> - `bind()` does not invoke the function immediately; it returns a new bound function with `this` permanently set, which can be invoked later.

---

### Q4: Why can't you overwrite `Math.PI` in JavaScript?
> **Answer**: `Math.PI` has its property descriptor flags configured as `writable: false` and `configurable: false`. Checking `Object.getOwnPropertyDescriptor(Math, "PI")` reveals that the property is permanently locked against reassignment and reconfiguration.

---

### Q5: How do closures achieve Encapsulation?
> **Answer**: When an outer function executes and returns an inner function or object with methods, the outer function's local variables remain accessible only to those returned inner functions via their lexical scope environment. Outside code cannot directly read or alter those variables, achieving true private state.

---

### Q6: What does the Prototype Chain terminate with?
> **Answer**: The prototype chain terminates with **`null`**. `Object.prototype.__proto__ === null`. When JavaScript searches for a property and reaches `null` without finding it, it returns `undefined`.

---

### Q7: Why does a Setter cause an infinite recursion ("Maximum call stack size exceeded") if not using an internal variable?
> **Answer**: Inside a setter like `set password(val) { this.password = val; }`, assigning `this.password = val` calls the setter again, which calls the setter again indefinitely. To avoid this recursive loop, developers use a backing variable with a distinct name, such as `this._password = val`.

