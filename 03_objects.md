# Objects in JavaScript - Interview Prep Notes

## 1. What is an object?

An object is a collection of key-value pairs. It is used to store related data and behavior together.

- Key = property name
- Value = data or function
- Objects represent real-world entities like user, product, car, etc.

```js
const user = {
  name: 'Ahmed',
  age: 24,
  city: 'Karachi'
};
```

### Interview answer
> An object in JavaScript is a non-primitive data type used to store structured data. It contains properties and methods, and can represent real-world entities.

---

## 2. Ways to create objects

### A. Object literal
This is the most common and beginner-friendly method.

```js
const person = {
  name: 'Ahmed',
  age: 24,
  greet() {
    console.log(`Hello, I am ${this.name}`);
  }
};
```

### B. Using `new Object()`
This creates an object instance using the constructor syntax.

```js
const obj = new Object();
obj.name = 'Ahmed';
obj.age = 24;
```

### Important interview point
- Object literal is more common in real-world JavaScript.
- `new Object()` is less preferred in modern code.

---

## 3. Accessing object properties

### Dot notation
Used when the property name is valid identifier.

```js
console.log(user.name);
```

### Bracket notation
Used when the key contains spaces or special characters, or is dynamic.

```js
console.log(user['age']);
console.log(user['full name']);
```

### Example

```js
const user = {
  name: 'Ahmed',
  age: 24,
  'full name': 'Ahmed Mohamed',
  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
};

console.log(user.name);         // Ahmed
console.log(user['age']);       // 24
console.log(user['full name']); // Ahmed Mohamed
user.greet();                   // Hello, my name is Ahmed
```

---

## 4. Symbol as object key

Symbols are unique and immutable values. They can be used as property keys.

```js
const mySymbol = Symbol('mySymbol1');

const myObject = {
  [mySymbol]: 'This is a symbol property'
};

console.log(myObject[mySymbol]); // This is a symbol property
```

### Interview point
- Symbols create hidden or unique keys.
- They are useful when you do not want a key to clash with other property names.

---

## 5. Adding and modifying properties

```js
const myObj = {};

myObj.name = 'Ahmed';
myObj.age = 24;

console.log(myObj); // { name: 'Ahmed', age: 24 }
```

You can also create nested objects.

```js
const anotherObj = {
  email: 'ahmed@gmail.com',
  address: 'House 1, Street 2',
  fullname: {
    firstname: 'Muhammad Ahmed',
    lastname: 'Chorahi'
  }
};

console.log(anotherObj.fullname.firstname); // Muhammad Ahmed
```

---

## 6. Merging objects

### Using spread operator

```js
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };
const obj3 = { d: 5 };

const combinedObj = { ...obj1, ...obj2, ...obj3 };
console.log(combinedObj); // { a: 1, b: 3, c: 4, d: 5 }
```

### Using `Object.assign()`

```js
const combinedObj2 = Object.assign({}, obj1, obj2, obj3);
console.log(combinedObj2); // { a: 1, b: 3, c: 4, d: 5 }
```

### Interview note
- Later properties overwrite earlier ones when keys are the same.
- `Object.assign()` and spread are common ways to merge objects.

---

## 7. Object keys, values, and entries

```js
const person = {
  name: 'John',
  age: 30,
  city: 'New York'
};

console.log(Object.keys(person));   // [ 'name', 'age', 'city' ]
console.log(Object.values(person)); // [ 'John', 30, 'New York' ]
console.log(Object.entries(person));
// [ [ 'name', 'John' ], [ 'age', 30 ], [ 'city', 'New York' ] ]
```

### When to use them?
- `Object.keys()` => get all property names
- `Object.values()` => get all property values
- `Object.entries()` => get key-value pairs as arrays

---

## 8. `hasOwnProperty()`

Used to check whether an object has a specific property.

```js
const person = {
  name: 'John',
  age: 30
};

console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('email')); // false
```

### Interview point
- It checks only own properties of the object.
- This is useful before reading a property to avoid errors.

---

## 9. Destructuring objects

Destructuring makes object data easier to extract and read.

```js
const person = {
  personName: 'Ahmed',
  age: 24,
  education: "Bachelor's Degree"
};

const { personName, age } = person;
console.log(personName); // Ahmed
console.log(age);       // 24
```

### Renaming while destructuring

```js
const { personName: name } = person;
console.log(name); // Ahmed
```

### Interview point
- Destructuring improves readability.
- It is commonly used in React, API responses, and function parameters.

---

## 10. Singleton concept

A singleton is an instance that is created only once and reused.

```js
const singletonObject = Object.freeze({
  name: 'Ahmed'
});
```

In JavaScript, singleton behavior is often associated with objects created using patterns like constructor functions or modules, where only one instance is maintained.

### Interview explanation
> A singleton is an object that is instantiated only once in the application lifecycle, ensuring shared state and a single source of truth.

---

## 11. Quick interview summary

### Key facts to remember
- Objects store data as key-value pairs.
- Use dot notation for valid identifiers and bracket notation for dynamic keys.
- Spread and `Object.assign()` can merge objects.
- `Object.keys()`, `Object.values()`, and `Object.entries()` help inspect object data.
- Destructuring is a clean way to extract properties.
- Nested objects are objects inside other objects.
- Symbols can be used as unique property keys.

---

## 12. Common interview questions

### Q1. What is an object in JavaScript?
Answer: An object is a collection of properties and methods used to represent data and behavior.

### Q2. Difference between dot notation and bracket notation?
Answer: Dot notation is for static property names; bracket notation is used for dynamic or special-character keys.

### Q3. How do you merge objects?
Answer: Use spread operator or `Object.assign()`.

### Q4. What is destructuring?
Answer: It is a way to unpack values from objects into variables.

### Q5. Why do we use `Object.keys()`?
Answer: To get the list of property names of an object.

---

## 13. Final short revision snippet

```js
const student = {
  name: 'Ahmed',
  age: 24,
  course: 'JavaScript',
  address: {
    city: 'Karachi'
  },
  greet() {
    console.log(`Hi, I am ${this.name}`);
  }
};

console.log(student.name);
console.log(student['course']);
console.log(Object.keys(student));
console.log(student.address.city);
const { name, age } = student;
console.log(name, age);
student.greet();
```

This is the core concept you should be ready to explain in an interview.

