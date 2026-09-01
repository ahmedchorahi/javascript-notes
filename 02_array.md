# Arrays in JavaScript

## Key Concepts

**Definition:** Array object enables collection of multiple items under a single variable name. In JavaScript, arrays are dynamic and can hold elements of different types.

### Properties
- ✅ Resizable and can store different data types
- ✅ Use numeric indexes starting from 0 (not named indexes like objects)
- ⚠️ Copy operations are shallow copies by default

### Shallow Copy vs Deep Copy
| Type | Description | Impact |
|------|-------------|--------|
| **Shallow Copy** | New array references same elements | Changes affect both arrays |
| **Deep Copy** | New array with own copy of elements | Changes isolated to new array |

---

## Array Declaration

```js
// Method 1: Array Literal (Recommended)
const array = [1, 2, 3, 4, 5];
const array = ["ahmed", "ali"];

// Method 2: Array Constructor
const array = new Array(1, 2, 3, 4, 5);
```

---

## Array Methods (Quick Reference)

### Mutating Methods (Modify Original Array)
| Method | What it does | Example |
|--------|-------------|---------|
| **push()** | Add to end | `array.push(6)` → `[1,2,3,4,5,6]` |
| **pop()** | Remove from end | `array.pop()` → `[1,2,3,4,5]` |
| **unshift()** | Add to beginning | `array.unshift(0)` → `[0,1,2,3,4,5]` |
| **shift()** | Remove from beginning | `array.shift()` → `[1,2,3,4,5]` |
| **splice(start, deleteCount)** | Remove/replace elements | `array.splice(1,3)` modifies original |

### Non-Mutating Methods (Return New Value)
| Method | What it does | Example |
|--------|-------------|---------|
| **slice(start, end)** | Return portion (end not included) | `array.slice(1,4)` → `[2,3,4]` |
| **concat()** | Combine arrays | `array.concat(array2)` → combined array |
| **includes(item)** | Check if item exists | `array.includes(3)` → `true` |
| **indexOf(item)** | Get index of item | `array.indexOf(3)` → `2`, not found → `-1` |
| **join(separator)** | Convert array to string | `array.join()` → `"1,2,3,4,5"` |
| **flat(depth)** | Flatten nested arrays | `[[1,2],[3,4]].flat()` → `[1,2,3,4]` |
| **isArray()** | Check if value is array | `Array.isArray([1,2])` → `true` |
| **from()** | Convert iterable to array | `Array.from("abc")` → `['a','b','c']` |
| **of()** | Create array from arguments | `Array.of(1,2,3)` → `[1,2,3]` |

### Examples

```js
// MUTATING METHODS
const array = [1, 2, 3, 4, 5];

array.push(6);        // [1,2,3,4,5,6]
array.pop();          // [1,2,3,4,5]
array.unshift(0);     // [0,1,2,3,4,5]
array.shift();        // [1,2,3,4,5]

// NON-MUTATING METHODS
const array = [1, 2, 3, 4, 5];

array.includes(3);              // true
array.includes(6);              // false
array.indexOf(3);               // 2
array.indexOf(6);               // -1

array.join();                   // "1,2,3,4,5" (type: string)

// SLICE vs SPLICE
console.log("Before:", array);  // [1,2,3,4,5]

const sliced = array.slice(1,4);    // Returns [2,3,4]
console.log("After slice:", array); // [1,2,3,4,5] - UNCHANGED

const spliced = array.splice(1,3);  // Returns [2,3,4]
console.log("After splice:", array); // [1,5] - MODIFIED
```

```js
const class1=["ahmed", "ali", "hamza"]
const class2=["qasim", "adnan", "rauf"]

class1.push(class2) //when we try to push array in and other array istead of pusing each item of array it push complete array as a item on a single index
console.log(class1) // ["ahmed", "ali", "hamza", ["qasim", "adnan", "rauf"]]

const cobinedArray = class1.concat(class2) //concat method is used to combine two arrays and save it in a new array
console.log(cobinedArray) // ["ahmed", "ali", "hamza", "qasim", "adnan", "rauf"]

const newCombinedArray = [...class1, ...class2] //spread operator is used to combine two arrays and save it in a new array
console.log(newCombinedArray) // ["ahmed", "ali", "hamza", "qasim", "adnan", "rauf"]

const array1 = [1, 2, 3, 4, 5, [6, 7, 8, 9, 10], [4,3,[1,2]]]
const flatArray = array1.flat(Infinity) //flat method is used to flatten the array. it also take an argument which is the depth of the array to be flattened. Infinity means it will flatten the array completely
console.log(flatArray) // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 4, 3, 1, 2]

console.log(Array.isArray("ahmed")) //false
console.log(Array.from("ahmed")) //["a", "h", "m", "e", "d"] - it will convert any iterable into array

let score1 = 10
let score2 = 20
let score3 = 30

console.log(Array.of(score1, score2, score3)) // [10, 20, 30] - it will create an array from the given arguments

```


### 🎯 Interview Tips
- Remember: **slice() doesn't modify**, **splice() does modify** original array
- Array indexes start at **0**
- Use **join()** to convert array to string
- Use **includes()** or **indexOf()** to search arrays
- Arrays are **dynamic** and can hold **different data types**
- **Shallow copy** by default - nested objects/arrays are still referenced
- Use **spread operator (...)** or **concat()** to combine arrays
- Use **flat()** with **Infinity** to completely flatten nested arrays
- **push()** returns new length, **pop()** returns removed element
- **unshift()** and **shift()** are slower than push/pop on large arrays
- Use **Array.isArray()** to check if a value is actually an array
- **Array.from()** converts iterables (strings, Sets, Maps) to arrays
- **Array.of()** creates arrays from arguments (useful vs Array constructor)

