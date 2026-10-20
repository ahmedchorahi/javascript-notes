<div align="center">

# ⚡ JavaScript Master Interview & Concepts Vault

<p align="center">
  <strong>Comprehensive, interview-ready notes, architectural diagrams, memory models, and mini-projects covering modern JavaScript from core fundamentals to advanced runtime internals.</strong>
</p>

[![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=for-the-badge)](https://github.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

</div>

---

## 📌 Overview

This repository is an all-in-one **technical revision roadmap** designed for software engineers preparing for frontend, backend (Node.js), and full-stack JavaScript technical interviews.

Every module follows a battle-tested structure:
* **Plain-English Explanations**: Concepts broken down without unnecessary academic jargon.
* **ASCII & Architectural Diagrams**: Call Stack, Memory Heap, Event Loop, V8 Pipeline, and Prototype Chains.
* **Quick-Scan Comparison Tables**: Fast reference matrices for rapid revision before interviews.
* **Commented Code Snippets**: Real-world examples showcasing inputs, outputs, and edge cases.
* **Interview Trap Alerts**: Dedicated callouts (`> [!WARNING]`, `> [!CAUTION]`) for classic interview trick questions.
* **Top 7 Technical Interview Questions & Answers**: High-yield model answers at the end of each module.

---

## 📚 Complete Syllabus & Revision Modules

| # | Topic | Module Guide | Key Concepts Covered |
| :-: | :--- | :--- | :--- |
| **01** | **Core Basics** | [📖 `01_basics.md`](./01_basics.md) | `var` vs `let` vs `const`, Primitive vs Reference, Stack vs Heap, Type Coercion, `typeof` quirks, Strings, Math, Date & Time, Temporal Dead Zone. |
| **02** | **Arrays** | [📖 `02_array.md`](./02_array.md) | Mutating vs Non-mutating methods, `slice()` vs `splice()`, Shallow vs Deep copy (`structuredClone`), `arr.at(-1)`, `Array.from()`, V8 Packed vs Holey arrays. |
| **03** | **Objects** | [📖 `03_objects.md`](./03_objects.md) | Dot vs Bracket notation, **Symbols as unique keys**, `Object.assign` vs Spread, `keys`/`values`/`entries`, `Object.hasOwn()`, Immutability (`freeze` vs `seal`), Destructuring. |
| **04** | **Functions** | [📖 `04_functions.md`](./04_functions.md) | Declarations vs Expressions vs Arrow functions, **Why Arrow functions were created (Lexical `this`)**, Parameters vs Arguments, Rest/Spread, IIFE, Higher-Order Functions. |
| **05** | **Execution Context** | [📖 `05_js-execution-context.md`](./05_js-execution-context.md) | Global vs Function Execution Contexts, Creation Phase (Memory Allocation) vs Execution Phase, Call Stack operations, Hoisting mechanics, Scope Chain. |
| **06** | **Control Flow** | [📖 `06_controll-flow.md`](./06_controll-flow.md) | `if-else`, Switch with fall-through protection, Truthy & Falsy reference list, Nullish Coalescing (`??`) vs OR (`\|\|`), Ternary operator. |
| **07** | **Loops & Iteration** | [📖 `07_loops.md`](./07_loops.md) | `for`, `while`, `do-while`, `for...of` (values) vs `for...in` (keys/indices), and Higher-Order array iteration: `forEach()`, `map()`, `filter()`, `reduce()`. |
| **08** | **DOM Manipulation** | [📖 `08_dom.md`](./08_dom.md) | Window & Document hierarchy, Node tree traversal, DOM selection methods, `innerHTML` vs `innerText` vs `textContent`, Dynamic element creation. |
| **09** | **DOM Events** | [📖 `09_events.md`](./09_events.md) | `addEventListener`, Pointer coordinates (`clientX` vs `pageX` vs `screenX`), Event Bubbling vs Capturing lifecycle, `stopPropagation()` vs `preventDefault()`, Event Delegation. |
| **10** | **Asynchronous JS** | [📖 `10_async-code.md`](./10_async-code.md) | Single-threaded nature, Event Loop architecture, Microtask (VIP) Queue vs Callback Queue priority riddle (`1, 4, 3, 2`), `setTimeout`, `clearTimeout`, `setInterval`. |
| **11** | **APIs & V8 Engine** | [📖 `11_ api-request-and-v8-engine.md`](./11_%20api-request-and-v8-engine.md) | `XMLHttpRequest` 5 `readyState` states, `JSON.parse` vs `stringify`, `fetch` vs XHR, Google V8 Engine (Ignition bytecode, TurboFan JIT compiler, Hot Code, Garbage Collector). |
| **12** | **Promises & Fetch** | [📖 `12_promises.md`](./12_promises.md) | Promise states, Chaining, `async/await`, **Fetch internals architecture diagram** (`[onFulfilled]` vs `[onRejection]`), HTTP 404/500 trap, Combinators (`all`, `allSettled`, `race`, `any`). |
| **13** | **OOP & Prototypes** | [📖 `13_ oop.md`](./13_%20oop.md) | Prototypes vs Classes, **The 4 steps performed by the `new` keyword**, Prototype Chain (`Array -> Object -> null`), 4 Pillars of OOP, `call`/`apply`/`bind`, Closures. |

---

## 🛠️ Hands-on Practice Projects

This repository also includes clean vanilla JavaScript practice projects located in the [`js-projects/`](./js-projects/) directory:

| Project | Folder Link | Focus Topics |
| :--- | :--- | :--- |
| 🧮 **BMI Calculator** | [`js-projects/bmi-calculator/`](./js-projects/bmi-calculator/) | Form submission, input parsing, validation, DOM updating, range categories. |
| 🎯 **Guess The Number** | [`js-projects/guess-the-number/`](./js-projects/guess-the-number/) | Game state management, random number generation, input guards, dynamic UI chips (High/Low). |
| 🎨 **Unlimited Random Colors** | [`js-projects/random-color/`](./js-projects/random-color/) | `setInterval`, `clearInterval`, asynchronous state guards, random hex generator. |
| ⌨️ **Keyboard Key Inspector** | [`js-projects/showkey/`](./js-projects/showkey/) | Keyboard events (`keydown`), `e.key`, `e.keyCode`, `e.code`, dynamic table rendering. |
| 🕒 **Digital Clock** | [`js-projects/clock/`](./js-projects/clock/) | `Date` object, real-time clock updates, formatting. |
| 🌈 **Color Switcher** | [`js-projects/color-changer/`](./js-projects/color-changer/) | DOM event listeners, background style manipulation. |

---

## 🧠 Architectural Diagrams Highlights

### 1. The JavaScript Runtime & Event Loop Architecture
```text
+-------------------------------------------------------------------------------+
|                            BROWSER RUNTIME ENVIRONMENT                        |
|                                                                               |
|  +---------------------------+             +-------------------------------+  |
|  |     JAVASCRIPT ENGINE     |             |           WEB APIs            |  |
|  |                           |             |                               |  |
|  |  +---------------------+  |             |  * setTimeout / setInterval   |  |
|  |  |     Memory Heap     |  |             |  * fetch() (HTTP Requests)    |  |
|  |  |  (Variable storage) |  |             |  * DOM Events (click, load)   |  |
|  |  +---------------------+  |             +---------------+---------------+  |
|  |                           |                             |                  |
|  |  +---------------------+  |                             v                  |
|  |  |     Call Stack      |  |             +-------------------------------+  |
|  |  | (Executes functions)|  |             |      MICROTASK QUEUE (VIP)    |  |
|  |  +----------^----------+  |             |  * Promise callbacks (.then)  |  |
|  +-------------|-------------+             +---------------+---------------+  |
|                |                                           |                  |
|          +-----+------+                                    v                  |
|          | EVENT LOOP | <------------------+-------------------------------+  |
|          +------------+                    |   TASK / CALLBACK QUEUE       |  |
|                                            |  * setTimeout callbacks       |  |
|                                            +-------------------------------+  |
+-------------------------------------------------------------------------------+
```

### 2. The Prototype Chain
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

---

## 🚀 How to Run the Projects Locally

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/your-repo-name.git
   cd your-repo-name
   ```

2. **Open any project in your browser:**
   * Open `index.html` inside any project folder (e.g. `js-projects/bmi-calculator/index.html`) directly in Google Chrome, Edge, or Firefox.
   * Or use the **Live Server** extension in VS Code.

---

## 💡 How to Use This Repository for Interviews

1. **Daily Concept Revision**: Review 2 modules per day from the syllabus table.
2. **Self-Quiz via the Interview Sections**: Jump directly to the **Top 7 Interview Questions & Answers** at the bottom of each file to test your recall.
3. **Inspect the Code**: Run the code snippets in your browser console or Node.js REPL to see the exact outputs and edge cases in real time.
4. **Practice the Projects**: Re-implement the projects in `js-projects/` from scratch to build muscle memory for DOM manipulation, timers, and event handling.

---

## 🌟 Acknowledgments & References

* Built with reference to the authoritative [Chai aur Code: JavaScript Full Course](https://youtu.be/_TjtAyMkiTI?si=RHS41XyzpTNOF3YP) by Hitesh Choudhary.
* Specifications and standards verified against [MDN Web Docs (Mozilla Developer Network)](https://developer.mozilla.org/en-US/docs/Web/JavaScript).

---

<div align="center">
  <sub>Made with ❤️ by Muhammad Ahmed Chorahi • If you find this vault helpful, please give it a ⭐ on GitHub!</sub>
</div>

## 🛠️ Hands-on Practice Projects

This repository also includes clean vanilla JavaScript practice projects located in the [`js-projects/`](./js-projects/) directory:

| Project | Folder Link | Focus Topics |
| :--- | :--- | :--- |
| 🧮 **BMI Calculator** | [`js-projects/bmi-calculator/`](./js-projects/bmi-calculator/) | Form submission, input parsing, validation, DOM updating, range categories. |
| 🎯 **Guess The Number** | [`js-projects/guess-the-number/`](./js-projects/guess-the-number/) | Game state management, random number generation, input guards, dynamic UI chips (High/Low). |
| 🎨 **Unlimited Random Colors** | [`js-projects/random-color/`](./js-projects/random-color/) | `setInterval`, `clearInterval`, asynchronous state guards, random hex generator. |
| ⌨️ **Keyboard Key Inspector** | [`js-projects/showkey/`](./js-projects/showkey/) | Keyboard events (`keydown`), `e.key`, `e.keyCode`, `e.code`, dynamic table rendering. |
| 🕒 **Digital Clock** | [`js-projects/clock/`](./js-projects/clock/) | `Date` object, real-time clock updates, formatting. |
| 🌈 **Color Switcher** | [`js-projects/color-changer/`](./js-projects/color-changer/) | DOM event listeners, background style manipulation. |

---

## 🧠 Architectural Diagrams Highlights

### 1. The JavaScript Runtime & Event Loop Architecture
```text
+-------------------------------------------------------------------------------+
|                            BROWSER RUNTIME ENVIRONMENT                        |
|                                                                               |
|  +---------------------------+             +-------------------------------+  |
|  |     JAVASCRIPT ENGINE     |             |           WEB APIs            |  |
|  |                           |             |                               |  |
|  |  +---------------------+  |             |  * setTimeout / setInterval   |  |
|  |  |     Memory Heap     |  |             |  * fetch() (HTTP Requests)    |  |
|  |  |  (Variable storage) |  |             |  * DOM Events (click, load)   |  |
|  |  +---------------------+  |             +---------------+---------------+  |
|  |                           |                             |                  |
|  |  +---------------------+  |                             v                  |
|  |  |     Call Stack      |  |             +-------------------------------+  |
|  |  | (Executes functions)|  |             |      MICROTASK QUEUE (VIP)    |  |
|  |  +----------^----------+  |             |  * Promise callbacks (.then)  |  |
|  +-------------|-------------+             +---------------+---------------+  |
|                |                                           |                  |
|          +-----+------+                                    v                  |
|          | EVENT LOOP | <------------------+-------------------------------+  |
|          +------------+                    |   TASK / CALLBACK QUEUE       |  |
|                                            |  * setTimeout callbacks       |  |
|                                            +-------------------------------+  |
+-------------------------------------------------------------------------------+
```

### 2. The Prototype Chain
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

---

## 🚀 How to Run the Projects Locally

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/your-repo-name.git
   cd your-repo-name
   ```

2. **Open any project in your browser:**
   * Open `index.html` inside any project folder (e.g. `js-projects/bmi-calculator/index.html`) directly in Google Chrome, Edge, or Firefox.
   * Or use the **Live Server** extension in VS Code.

---

## 💡 How to Use This Repository for Interviews

1. **Daily Concept Revision**: Review 2 modules per day from the syllabus table.
2. **Self-Quiz via the Interview Sections**: Jump directly to the **Top 7 Interview Questions & Answers** at the bottom of each file to test your recall.
3. **Inspect the Code**: Run the code snippets in your browser console or Node.js REPL to see the exact outputs and edge cases in real time.
4. **Practice the Projects**: Re-implement the projects in `js-projects/` from scratch to build muscle memory for DOM manipulation, timers, and event handling.

---

## 🌟 Acknowledgments & References

* Built with reference to the authoritative [Chai aur Code: JavaScript Full Course](https://youtu.be/_TjtAyMkiTI?si=RHS41XyzpTNOF3YP) by Hitesh Choudhary.
* Specifications and standards verified against [MDN Web Docs (Mozilla Developer Network)](https://developer.mozilla.org/en-US/docs/Web/JavaScript).

---

<div align="center">
  <sub>Made with ❤️ by Muhammad Ahmed Chorahi • If you find this vault helpful, please give it a ⭐ on GitHub!</sub>
</div>

