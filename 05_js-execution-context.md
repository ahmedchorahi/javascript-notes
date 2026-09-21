# JavaScript Execution Context & Call Stack

> **Interview Revision Notes**
>
> Topic: How JavaScript executes code, creates execution contexts, and manages function calls.

---

## 1. How JavaScript Executes Code

JavaScript execution is commonly explained using two major phases:

1. **Creation Phase** — memory is prepared for variables, functions, and other declarations.
2. **Execution Phase** — the code is executed, usually from top to bottom.

Before JavaScript starts executing the code, the JavaScript engine creates an initial execution context called the **Global Execution Context**.

### High-level execution flow

```text
JavaScript Source Code
         |
         v
Global Execution Context
         |
         +----------------------+
         |                      |
         v                      v
  Creation Phase         Execution Phase
  (Prepare memory)       (Run the code)
         |                      |
         +----------+-----------+
                    |
                    v
              Call Stack
                    |
                    v
          Function Execution Contexts
```

---

## 2. What Is an Execution Context?

An **execution context** is the environment in which JavaScript code is evaluated and executed.

It contains the information needed by the JavaScript engine to run a particular piece of code, such as:

- Variables and their values
- Function declarations
- Function parameters
- Access to the outer lexical environment
- The value of `this`, where applicable

Think of an execution context as a **workspace** created by the JavaScript engine for running code.

### Main types of execution contexts

| Execution Context | Description |
|---|---|
| Global Execution Context | Created when the JavaScript program starts |
| Function Execution Context | Created whenever a function is called |
| Eval Execution Context | Created when code is executed using `eval()`; rarely used in normal development |

For most interviews and everyday JavaScript development, focus on:

1. Global Execution Context
2. Function Execution Context

---

## 3. Global Execution Context

The **Global Execution Context (GEC)** is created when a JavaScript program begins execution.

There is generally one global execution context per JavaScript realm.

The global context is responsible for executing top-level code.

### The `this` value and the global object

The global execution context has a `this` value, but the exact behavior depends on the environment and the type of script.

| Environment | Global object |
|---|---|
| Browser | `window` |
| Node.js | `global` |
| Modern JavaScript standard | `globalThis` provides a standard way to access the global object |

> **Important:** In browsers, `this` at the top level of a classic script usually refers to `window`. In an ES module, top-level `this` is `undefined`. In Node.js, top-level `this` in a CommonJS module is not the same as the `global` object.

### Recommended modern approach

```js
console.log(globalThis);
```

`globalThis` is the portable way to access the global object across JavaScript environments.

---

## 4. The Two Phases of Execution

JavaScript execution can be understood through two phases.

## Phase 1: Creation Phase

During the creation phase, the engine prepares the execution environment.

Depending on the declaration type, it may:

- Allocate memory for variables
- Register function declarations
- Create bindings for variables and constants
- Set up the lexical environment
- Establish the value of `this`
- Prepare the environment for executing the code

### Important behavior of declarations

| Declaration | Behavior before initialization |
|---|---|
| `var` | Binding is initialized to `undefined` |
| `let` | Binding exists but is in the Temporal Dead Zone |
| `const` | Binding exists but is in the Temporal Dead Zone |
| Function declaration | Function can generally be called before its declaration in the same scope |

Example:

```js
console.log(a); // undefined
var a = 10;
```

```js
console.log(b); // ReferenceError
let b = 20;
```

```js
sayHello(); // Works

function sayHello() {
  console.log("Hello");
}
```

> **Interview tip:** It is more accurate to say that `let` and `const` are created during environment setup but remain uninitialized in the **Temporal Dead Zone (TDZ)** until execution reaches their declarations.

---

## Phase 2: Execution Phase

During the execution phase:

- Statements are evaluated
- Values are assigned to variables
- Functions are called
- Expressions are evaluated
- Results are returned
- Control moves through the program

Example:

```js
let age = 24;
let nextAge = age + 1;

console.log(nextAge); // 25
```

Conceptually:

```text
Creation Phase:
age      -> uninitialized
nextAge  -> uninitialized

Execution Phase:
age      -> 24
nextAge  -> 25
```

---

# 5. Complete Example

Consider the following code:

```js
let val1 = 10;
let val2 = 20;

function add(num1, num2) {
  let total = num1 + num2;
  return total;
}

let result1 = add(val1, val2);
let result2 = add(30, 40);

console.log(result1); // 30
console.log(result2); // 70
```

We can understand this code by separating the global context from the function contexts.

---

## 6. Step-by-Step Execution

## Step 1: Create the Global Execution Context

When the program starts, the JavaScript engine creates the Global Execution Context.

```text
+--------------------------------------------------+
|              GLOBAL EXECUTION CONTEXT             |
|                                                  |
|  this                                            |
|  Global environment                               |
|  Top-level variables and functions               |
+--------------------------------------------------+
```

The global context is placed on the **call stack**.

```text
CALL STACK

+----------------------------------+
| Global Execution Context         |
+----------------------------------+
```

---

## Step 2: Global Creation Phase

During this phase, the engine prepares the global environment.

For this example:

```js
let val1 = 10;
let val2 = 20;

function add(num1, num2) {
  let total = num1 + num2;
  return total;
}

let result1 = add(val1, val2);
let result2 = add(30, 40);
```

A simplified conceptual representation is:

```text
Global Environment
--------------------------------
val1     -> uninitialized
val2     -> uninitialized
add      -> function definition
result1  -> uninitialized
result2  -> uninitialized
```

> This is a simplified model. `let` and `const` bindings are not initialized to `undefined`; they remain uninitialized until their declarations execute.

---

## Step 3: Global Execution Phase

The engine begins executing the code.

### 3.1 Assign values to `val1` and `val2`

```js
val1 = 10;
val2 = 20;
```

The environment now conceptually contains:

```text
Global Environment
--------------------------------
val1     -> 10
val2     -> 20
add      -> function definition
result1  -> uninitialized
result2  -> uninitialized
```

### 3.2 Reach the first function call

```js
let result1 = add(val1, val2);
```

Before the function can execute, JavaScript creates a new **Function Execution Context** for this particular call.

> A function call creates a new execution context. It does **not** create a new operating-system thread.

---

# 7. Function Execution Context

A **Function Execution Context (FEC)** is created every time a function is called.

Each function call gets its own local execution environment.

For the first call:

```js
add(val1, val2);
```

The values passed to the function are:

```js
num1 = 10;
num2 = 20;
```

## Function Creation Phase

A simplified view of the new function context is:

```text
Function Execution Context: add()
--------------------------------
num1   -> parameter binding
num2   -> parameter binding
total  -> uninitialized
this   -> function-call-specific value
```

## Function Execution Phase

The function body runs:

```js
let total = num1 + num2;
return total;
```

The steps are:

```text
num1  -> 10
num2  -> 20
total -> 10 + 20
total -> 30
return 30
```

The function returns `30` to the statement that called it.

After the function finishes, its execution context is removed from the call stack, assuming no other references or suspended execution are keeping related state alive.

---

## 8. First Function Call: Visualized

### Before the call

```text
CALL STACK

+----------------------------------+
| Global Execution Context         |
|                                  |
| Executing: add(val1, val2)       |
+----------------------------------+
```

### During the function call

```text
CALL STACK

+----------------------------------+
| Function Execution Context      |
| add(10, 20)                      |
|                                  |
| num1 = 10                        |
| num2 = 20                        |
| total = 30                       |
+----------------------------------+
| Global Execution Context         |
+----------------------------------+
```

The function context is placed **on top** of the global context because it is the currently executing context.

### After the function returns

```text
CALL STACK

+----------------------------------+
| Global Execution Context         |
|                                  |
| result1 = 30                     |
+----------------------------------+
```

The function execution context has been popped from the call stack.

---

## 9. Second Function Call

The next statement is:

```js
let result2 = add(30, 40);
```

A new function execution context is created for this call.

### Function creation phase

```text
Function Execution Context: add()
--------------------------------
num1   -> parameter binding
num2   -> parameter binding
total  -> uninitialized
```

### Function execution phase

```text
num1  -> 30
num2  -> 40
total -> 30 + 40
total -> 70
return 70
```

The function returns `70`.

The global environment is then updated:

```text
Global Environment
--------------------------------
val1     -> 10
val2     -> 20
add      -> function definition
result1  -> 30
result2  -> 70
```

Final output:

```text
30
70
```

---

# 10. Complete Call Stack Visualization

The entire process can be summarized as follows:

```text
PROGRAM STARTS
      |
      v
Create Global Execution Context
      |
      v
Push Global Context onto Call Stack
      |
      v
Run global code
      |
      v
Call add(10, 20)
      |
      v
Push add() Context
      |
      v
Execute add()
      |
      v
Return 30
      |
      v
Pop add() Context
      |
      v
Store result1 = 30
      |
      v
Call add(30, 40)
      |
      v
Push new add() Context
      |
      v
Execute add()
      |
      v
Return 70
      |
      v
Pop add() Context
      |
      v
Store result2 = 70
      |
      v
Global code finishes
      |
      v
Program completes
```

---

# 11. What Is the Call Stack?

The **call stack** is a data structure used by the JavaScript engine to keep track of active execution contexts.

It follows the **LIFO** principle:

> **Last In, First Out**

The last execution context pushed onto the stack is the first one removed.

## Call stack operations

| Operation | Meaning |
|---|---|
| Push | Add a new execution context to the top |
| Pop | Remove the top execution context |
| Top | The context currently being executed |

### Example

```js
function first() {
  second();
}

function second() {
  console.log("Hello");
}

first();
```

### Stack progression

#### 1. Program starts

```text
+----------------------------+
| Global Context             |
+----------------------------+
```

#### 2. `first()` is called

```text
+----------------------------+
| first() Context            |
+----------------------------+
| Global Context             |
+----------------------------+
```

#### 3. `second()` is called

```text
+----------------------------+
| second() Context           |
+----------------------------+
| first() Context            |
+----------------------------+
| Global Context             |
+----------------------------+
```

#### 4. `second()` finishes

```text
+----------------------------+
| first() Context            |
+----------------------------+
| Global Context             |
+----------------------------+
```

#### 5. `first()` finishes

```text
+----------------------------+
| Global Context             |
+----------------------------+
```

#### 6. Global code finishes

```text
CALL STACK: EMPTY
```

---

# 12. JavaScript Is Single-Threaded

JavaScript execution is commonly described as **single-threaded** because a JavaScript execution thread generally processes one piece of JavaScript code at a time.

This means that two normal JavaScript statements do not execute simultaneously on the same execution thread.

Example:

```js
console.log("A");
console.log("B");
console.log("C");
```

Output:

```text
A
B
C
```

The statements execute in order.

## Does single-threaded mean JavaScript cannot handle asynchronous tasks?

No.

JavaScript runtimes, such as browsers and Node.js, provide facilities for handling asynchronous operations.

Examples include:

- Timers
- Network requests
- File-system operations in Node.js
- Promises
- Event listeners

The runtime can perform or coordinate work outside the JavaScript execution stack. When the result is ready, a callback or continuation can be scheduled for JavaScript execution.

### Simplified asynchronous model

```text
                 JavaScript Runtime
+--------------------------------------------------+
|                                                  |
|  Call Stack                                      |
|      |                                           |
|      v                                           |
|  JavaScript Code                                 |
|      |                                           |
|      v                                           |
|  Web APIs / Node.js APIs                         |
|      |                                           |
|      v                                           |
|  Timer, Network, File Operation, etc.            |
|      |                                           |
|      v                                           |
|  Task Queue / Microtask Queue                    |
|      |                                           |
|      v                                           |
|  Event Loop                                      |
|      |                                           |
|      +------> Call Stack when it is available    |
|                                                  |
+--------------------------------------------------+
```

### Example

```js
console.log("Start");

setTimeout(() => {
  console.log("Timer finished");
}, 0);

console.log("End");
```

Output:

```text
Start
End
Timer finished
```

### Why?

1. `"Start"` is printed immediately.
2. The timer is registered with the runtime.
3. JavaScript continues executing the next statement.
4. `"End"` is printed.
5. The timer callback is scheduled for later execution.
6. Once the call stack is available, the callback can run.

> **Interview tip:** `setTimeout(..., 0)` does not mean “execute immediately.” It means the callback can be scheduled after the relevant delay and after the runtime allows it to run.

---

# 13. Execution Context vs. Call Stack

These concepts are related but different.

| Concept | Meaning |
|---|---|
| Execution Context | The environment and information needed to execute a piece of code |
| Call Stack | The stack data structure that tracks active execution contexts |
| Function Call | An action that generally creates a new function execution context |
| Event Loop | A runtime mechanism that coordinates when queued callbacks can run |

### Simple analogy

Imagine a restaurant:

- **Execution context:** A work area prepared for a particular task.
- **Call stack:** A stack of tasks currently waiting to be completed.
- **Function call:** A new task is placed on top.
- **Return:** The task is completed and removed.
- **Event loop:** Helps coordinate when queued tasks can be processed.

---

# 14. Important Corrections and Interview Traps

## Trap 1: A new function call creates a new thread

Incorrect:

> Every function call creates a new execution thread.

Correct:

> Every function call generally creates a new function execution context. That context is managed by the call stack. It does not create a new operating-system thread.

---

## Trap 2: Every execution context has a global execution context inside it

Incorrect:

> Every execution context contains a global execution context.

Correct:

> The global execution context is the initial context for top-level code. Function contexts have access to their outer lexical environments, which may eventually lead to the global environment.

---

## Trap 3: `let` and `const` are initialized with `undefined`

Incorrect:

> During creation, `let` and `const` are initialized with `undefined`.

Correct:

> `let` and `const` bindings are created but remain uninitialized in the Temporal Dead Zone until execution reaches their declarations.

---

## Trap 4: The value of `this` is always the global object

Incorrect:

> `this` always contains the global object.

Correct:

> The value of `this` depends on the execution context, the function type, and how the function is called.

Examples:

```js
console.log(this);
```

At the top level, behavior depends on the environment and whether the code is a script or module.

```js
function showThis() {
  console.log(this);
}
```

The value of `this` depends on how `showThis()` is invoked and whether strict mode applies.

Arrow functions do not create their own `this`; they capture it from the surrounding scope.

---

## Trap 5: The call stack handles asynchronous work by itself

Incorrect:

> The call stack waits for asynchronous operations to finish.

Correct:

> The JavaScript call stack executes synchronous code. The runtime coordinates asynchronous operations, and callbacks or continuations are queued for later execution.

---

# 15. Quick Revision Summary

```text
1. JavaScript starts by creating the Global Execution Context.

2. Execution is commonly explained in two phases:
   - Creation Phase
   - Execution Phase

3. The creation phase prepares bindings and function declarations.

4. The execution phase evaluates statements and expressions.

5. A function call generally creates a Function Execution Context.

6. The function context is pushed onto the Call Stack.

7. When the function returns, its context is popped.

8. The Call Stack follows LIFO:
   Last In, First Out.

9. JavaScript normally executes one piece of JavaScript
   at a time on a given execution thread.

10. Asynchronous operations are coordinated by the
    JavaScript runtime, event loop, and queues.

11. `this` is not always the global object.

12. `let` and `const` are subject to the Temporal Dead Zone.
```

---

# 16. Interview Questions and Answers

## Q1. What is an execution context?

An execution context is the environment in which JavaScript code is evaluated and executed. It contains information such as variable bindings, function information, access to outer environments, and the relevant `this` value.

---

## Q2. What are the main types of execution contexts?

The commonly discussed types are:

1. Global Execution Context
2. Function Execution Context
3. Eval Execution Context

The first two are the most important for everyday development and interviews.

---

## Q3. What are the two phases of JavaScript execution?

They are:

1. **Creation Phase:** The engine prepares the execution environment and registers declarations.
2. **Execution Phase:** The engine executes statements and assigns or evaluates values.

---

## Q4. What happens when a function is called?

A new function execution context is generally created. It is pushed onto the call stack, the function body executes, and the context is removed from the stack when the function returns or completes.

---

## Q5. What is the call stack?

The call stack is a LIFO data structure used to track active execution contexts and manage the order in which functions execute and return.

---

## Q6. Is JavaScript single-threaded?

JavaScript execution is commonly single-threaded within a given execution thread. However, browser and Node.js runtimes provide asynchronous APIs and may use other threads or system facilities for background work.

---

## Q7. Does `setTimeout(fn, 0)` execute immediately?

No. It schedules the callback to run later. The callback cannot execute until the current synchronous work has finished and the runtime allows the callback to be processed.

---

## Q8. What is the Temporal Dead Zone?

The Temporal Dead Zone is the period between the creation of a `let`, `const`, or certain other lexical binding and its initialization. Accessing the binding during this period causes a `ReferenceError`.

---

## Q9. Is `this` always equal to `window` in a browser?

No. It depends on the script or module context and how a function is called. In browser classic scripts, top-level `this` generally refers to `window`, but top-level `this` in an ES module is `undefined`.

---

## Q10. What is the difference between an execution context and a thread?

An execution context is a JavaScript concept representing the environment for running code. A thread is an operating-system or runtime execution resource. Creating a function execution context does not create a new thread.

---

# 17. Practice Exercise

Predict the output before running this code:

```js
console.log("1");

function greet() {
  console.log("2");
  calculate();
  console.log("3");
}

function calculate() {
  console.log("4");
}

greet();

console.log("5");
```

### Answer

```text
1
2
4
3
5
```

### Call stack reasoning

```text
Global Context
      |
      v
greet() Context
      |
      v
calculate() Context
      |
      v
calculate() returns
      |
      v
greet() continues
      |
      v
greet() returns
      |
      v
Global code continues
```

---

# 18. Final Mental Model

Remember this simple sequence:

```text
SOURCE CODE
    |
    v
GLOBAL EXECUTION CONTEXT
    |
    v
CREATION PHASE
    |
    v
EXECUTION PHASE
    |
    v
FUNCTION CALL
    |
    v
NEW FUNCTION EXECUTION CONTEXT
    |
    v
PUSH ONTO CALL STACK
    |
    v
EXECUTE FUNCTION
    |
    v
RETURN VALUE
    |
    v
POP FUNCTION CONTEXT
    |
    v
CONTINUE PREVIOUS CONTEXT
```

> **One-line interview answer:**
>
> JavaScript begins by creating a global execution context. During execution, function calls create new function execution contexts that are pushed onto the call stack. JavaScript executes the active context, and when a function returns, its context is removed from the stack. Although JavaScript execution is generally single-threaded, the runtime supports asynchronous operations through mechanisms such as the event loop, task queues, and promises.