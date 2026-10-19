# DOM (Document Object Model) in JavaScript

The **Document Object Model (DOM)** is a programming interface for web documents. It represents an HTML document as a hierarchical tree of nodes (element nodes, text nodes, and attribute nodes), allowing JavaScript to dynamically access, manipulate, add, or remove elements, attributes, and styles.

---

## 1) Window and Document Hierarchy

In the browser environment, `window` is the global object representing the browser tab. `document` is a property of `window` that represents the loaded HTML page.

```text
Window (Global Execution Context)
└── Document (window.document)
    └── HTML Root Node (<html>)
        ├── Head (<head>)
        │   ├── Title (<title>)
        │   └── Meta (<meta>)
        └── Body (<body>)
            ├── <div>
            ├── <h1>
            └── <p>
```

---

## 2) The DOM Tree & Node Types

Every element, piece of text, or comment in an HTML document is represented as a **Node**.

### Sample HTML Reference
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div class="bg-black">
        <h1 id="title" class="heading">
            This is the DOM Topic <span style="display: none;">test content</span>
        </h1>
        <h2>Heading 1 of h2</h2>
        <h2>Heading 2 of h2</h2>
        <h2>Heading 3 of h2</h2>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
        <ul>
            <li class="list-item">one</li>
            <li class="list-item">two</li>
            <li class="list-item">three</li>
        </ul>
    </div>
</body>
</html>
```

### Node Representation Diagram
```text
Document (Node)
└── html [Element Node] lang="en"
    ├── head [Element Node]
    │   ├── meta [Element Node] charset="UTF-8"
    │   └── title [Element Node]
    │       └── "Document" [Text Node]
    └── body [Element Node]
        └── div [Element Node] class="bg-black"
            ├── h1 [Element Node] id="title" class="heading"
            │   ├── "This is the DOM Topic " [Text Node]
            │   └── span [Element Node] style="display: none;"
            │       └── "test content" [Text Node]
            ├── h2 [Element Node]
            │   └── "Heading 1 of h2" [Text Node]
            └── ul [Element Node]
                └── li [Element Node] class="list-item"
                    └── "one" [Text Node]
```

---

## 3) Selecting DOM Elements

JavaScript provides several methods to select elements from the DOM:

### A. `document.getElementById('id')`
Selects a single element by its unique `id` attribute. Returns `null` if not found.

```js
const title = document.getElementById('title');
console.log(title); // <h1 id="title" class="heading">...</h1>
```

### B. `document.getElementsByClassName('className')`
Selects all elements with the specified class name. Returns an **`HTMLCollection`** (not an array).

```js
const headings = document.getElementsByClassName('heading');
console.log(headings); // HTMLCollection [h1#title.heading]
```

### C. `document.getElementsByTagName('tagName')`
Selects all elements matching the HTML tag name. Returns an **`HTMLCollection`**.

```js
const allParagraphs = document.getElementsByTagName('p');
```

### D. `document.querySelector('selector')`
Selects the **first element** that matches any valid CSS selector (`tag`, `.class`, `#id`, `[attribute]`, or nested selector). Returns `null` if no match is found.

```js
// By tag name
const firstH1 = document.querySelector('h1');

// By ID
const titleById = document.querySelector('#title');

// By Class name
const headingByClass = document.querySelector('.heading');

// By nested CSS selector
const myUl = document.querySelector('ul');
const firstLi = myUl.querySelector('li');
firstLi.style.backgroundColor = 'green';
```

### E. `document.querySelectorAll('selector')`
Selects **all elements** matching the given CSS selector. Returns a **`NodeList`**.

```js
const allListItems = document.querySelectorAll('li');
console.log(allListItems); // NodeList(3) [li, li, li]
```

---

## 4) Attributes and Styling

Once an element is selected, you can inspect and modify its attributes and inline CSS styles.

### A. Reading and Setting Attributes

```js
const title = document.getElementById('title');

// 1. Direct property access
console.log(title.id);        // Output: "title"
console.log(title.className); // Output: "heading" (Notice: className, not class)

// 2. getAttribute()
console.log(title.getAttribute('id'));    // Output: "title"
console.log(title.getAttribute('class')); // Output: "heading"

// 3. setAttribute() - Replaces existing attribute value
title.setAttribute('class', 'test');               // Replaces "heading" with "test"
title.setAttribute('class', 'test heading');       // Sets multiple classes
title.setAttribute('data-topic', 'javascript-dom'); // Sets custom data attribute
```

### B. Modifying Inline CSS Styles
Styles applied via `.style` are set as **inline styles** on the element. CSS property names with hyphens are written in **camelCase**:

```js
const title = document.getElementById('title');

title.style.backgroundColor = 'green';
title.style.borderRadius = '15px';
title.style.padding = '20px';
title.style.color = '#ffffff';
```

---

## 5) Getting Content: `innerText` vs `textContent` vs `innerHTML`

This is one of the most frequently asked DOM interview questions.

Suppose we have this HTML:
```html
<h1 id="title" class="heading">
    This is the DOM Topic <span style="display: none;">test content</span>
</h1>
```

Let's compare the three properties on this element:

```js
const title = document.getElementById('title');

console.log(title.innerText);
// Output: "This is the DOM Topic"

console.log(title.textContent);
// Output: "This is the DOM Topic test content"

console.log(title.innerHTML);
// Output: "This is the DOM Topic <span style=\"display: none;\">test content</span>"
```

### Key Differences & Interview Breakdown

| Property | What it returns | Respects CSS Styling? | Includes HTML tags? | Performance |
| :--- | :--- | :--- | :--- | :--- |
| **`innerText`** | Only the **visible, rendered text** displayed on the screen. | **Yes** (Hides text inside `display: none` or `visibility: hidden`). | No | Slower (triggers browser reflow/layout calculation). |
| **`textContent`** | **All text content** inside the node and all its descendants. | **No** (Shows text even if hidden with CSS). | No | Faster (reads raw text directly from the DOM tree). |
| **`innerHTML`** | Entire content including **inner HTML tags and markup**. | N/A (Returns raw HTML string). | **Yes** | Use with caution (can cause **XSS vulnerabilities** if handling untrusted user input). |

---

## 6) Collection Types: `NodeList` vs `HTMLCollection` vs `Array`

A major source of bugs and interview questions is understanding how DOM query results behave.

### A. Common Pitfall: Trying to Style a Collection Directly
```js
const myLi = document.querySelectorAll('li'); // Returns NodeList(3)

// ❌ WRONG: NodeList is a collection, not a single element!
// myLi.style.color = 'blue'; 
// Uncaught TypeError: Cannot set properties of undefined (setting 'color')

// ✅ CORRECT: Access by index
myLi[0].style.color = 'blue';

// ✅ CORRECT: Iterate through the collection
myLi.forEach((item) => {
    item.style.color = 'red';
});
```

---

### B. `NodeList` vs `HTMLCollection`

```js
// querySelectorAll returns a NodeList
const nodeItems = document.querySelectorAll('li');

// getElementsByClassName returns an HTMLCollection
const htmlItems = document.getElementsByClassName('list-item');
```

| Feature | `NodeList` | `HTMLCollection` |
| :--- | :--- | :--- |
| **Returned by** | `document.querySelectorAll()` | `getElementsByClassName()`, `getElementsByTagName()` |
| **Nodes included** | Can contain element nodes, text nodes, comment nodes | **Element nodes only** |
| **Has built-in `forEach`?** | **Yes** (`nodeItems.forEach(...)` works) | **No** (`htmlItems.forEach` is `undefined`) |
| **Live or Static?** | **Static snapshot** (changes to DOM don't auto-update it) | **Live collection** (auto-updates when DOM elements change) |
| **Access by Name/ID?** | By index only (`list[0]`) | By index (`list[0]`) or named property (`list['id']`) |

---

### C. Converting Collections to Real Arrays

Neither `NodeList` nor `HTMLCollection` have full array methods like `.map()`, `.filter()`, or `.reduce()`. To use array methods, convert them first:

#### Method 1: `Array.from()` (Recommended)
```js
const htmlItems = document.getElementsByClassName('list-item');

// Convert HTMLCollection to Array
const itemsArray = Array.from(htmlItems);

itemsArray.map((li) => {
    li.style.color = 'orange';
});
```

#### Method 2: Spread Operator (`...`)
```js
const itemsArray = [...document.querySelectorAll('li')];

const textContents = itemsArray.map((li) => li.innerText);
console.log(textContents); // Output: ['one', 'two', 'three']
```

---

## 7) DOM Traversal (Parent, Child & Sibling Navigation)

Traversing the DOM means moving from one node to related nodes without re-querying the whole document.

### Sample HTML Reference
```html
<div class="parent">
    <div class="days">Monday</div>
    <div class="days">Tuesday</div>
    <div class="days">Wednesday</div>
    <div class="days">Thursday</div>
</div>
```

### A. Parent to Children Navigation

```js
const parent = document.querySelector('.parent');

// 1. parent.children -> returns HTMLCollection of child elements
console.log(parent.children);
console.log(parent.children[0].innerHTML); // Output: "Monday"

// Iterating over children
for (let i = 0; i < parent.children.length; i++) {
    console.log(parent.children[i].innerHTML);
}

// Styling a specific child
parent.children[1].style.color = 'orange'; // Makes "Tuesday" orange

// 2. firstElementChild and lastElementChild
console.log(parent.firstElementChild.innerHTML); // Output: "Monday"
console.log(parent.lastElementChild.innerHTML);  // Output: "Thursday"
```

### B. Child to Parent Navigation

```js
const dayOne = document.querySelector('.days'); // "Monday"

// Access parent element
console.log(dayOne.parentElement); // <div class="parent">...</div>
```

### C. Sibling Navigation

```js
const dayOne = document.querySelector('.days'); // "Monday"

// Move to next sibling element
console.log(dayOne.nextElementSibling.innerHTML); // Output: "Tuesday"

// Move to previous sibling element (null for first element)
console.log(dayOne.previousElementSibling); // Output: null
```

### Interview Point: `.children` vs `.childNodes`
This is a classic JavaScript interview question:
* **`parent.children`**: Returns an **`HTMLCollection`** of **Element nodes only** (ignores text, comments, and whitespace).
* **`parent.childNodes`**: Returns a **`NodeList`** of **all nodes**, including element nodes, comment nodes, and text nodes representing whitespace and line breaks (`\n`).

```js
console.log(parent.children.length);   // Output: 4 (4 div elements)
console.log(parent.childNodes.length); // Output: 9 (4 divs + 5 line-break text nodes)
```

---

## 8) Creating and Inserting Elements

You can programmatically generate new elements and add them to the page.

### A. Basic Element Creation
```js
// 1. Create element
const div = document.createElement('div');

// 2. Add attributes and styles
div.className = 'main';
div.id = Math.round(Math.random() * 10 + 1);
div.setAttribute('title', 'Generated Title');
div.style.backgroundColor = 'green';
div.style.color = 'white';
div.style.padding = '12px';

// 3. Add content using createTextNode
const addText = document.createTextNode('Ahmed');
div.appendChild(addText);

// 4. Attach to the DOM tree
document.body.appendChild(div);
```

### B. Performance Optimization: `innerHTML` vs `createTextNode`
When creating elements inside a function (e.g. adding list items):

```html
<ul class="languages">
    <li>JavaScript</li>
</ul>
```

```js
// ❌ Approach 1: Less Optimized (uses innerHTML)
function addLanguage(langName) {
    const li = document.createElement('li');
    li.innerHTML = `${langName}`;
    document.querySelector('.languages').appendChild(li);
}
addLanguage('Python');

// ✅ Approach 2: Optimized (uses createTextNode)
function addLanguageOptimized(langName) {
    const li = document.createElement('li');
    li.appendChild(document.createTextNode(langName));
    document.querySelector('.languages').appendChild(li);
}
addLanguageOptimized('C++');
```

### Interview Point: Why is `createTextNode()` more optimized?
- Setting `li.innerHTML = langName` causes the browser's HTML parser to process the string, traverse the DOM tree, and reconstruct internal representations.
- `document.createTextNode(langName)` creates a direct text node object in memory and appends it without triggering the HTML parser, making it significantly faster and safer.

---

## 9) Editing and Replacing Elements

There are multiple ways to replace or update existing elements in the DOM:

```html
<ul class="languages">
    <li>JavaScript</li>
    <li>Python</li>
    <li>C++</li>
</ul>
```

### Method 1: `replaceWith()` (Modern & Recommended)
```js
// Select the second li ("Python")
const secondLi = document.querySelector('li:nth-child(2)');

// Create replacement element
const newLi = document.createElement('li');
newLi.textContent = 'Mojo';

// Replace in-place
secondLi.replaceWith(newLi);
```

### Method 2: `outerHTML`
`outerHTML` replaces the element itself along with its content:
```js
// Select the first li ("JavaScript")
const firstLi = document.querySelector('li:first-child');

// Overwrite the entire element tag and content
firstLi.outerHTML = '<li>TypeScript</li>';
```

### Method 3: `replaceChild()` (Traditional Method)
```js
const list = document.querySelector('.languages');
const target = document.querySelector('li:nth-child(3)');

const updatedLi = document.createElement('li');
updatedLi.textContent = 'Rust';

list.replaceChild(updatedLi, target);
```

---

## 10) Removing Elements from the DOM

### Method 1: `element.remove()` (Modern)
Directly removes the element on which it is called.

```js
const lastLi = document.querySelector('li:last-child');
lastLi.remove();
```

### Method 2: `parentElement.removeChild()` (Traditional)
Removes a specific child node from its parent.

```js
const list = document.querySelector('.languages');
const firstItem = list.firstElementChild;
list.removeChild(firstItem);
```

---

## 11) Summary & Comprehensive Interview Cheat Sheet

### 1. Element Selectors
| Method | Return Type | Live / Static | Notes |
| :--- | :--- | :--- | :--- |
| `getElementById('id')` | `Element` or `null` | Live | Fastest selector |
| `querySelector('css')` | `Element` or `null` | Static | First match only |
| `querySelectorAll('css')` | `NodeList` | Static | Supports `.forEach()` |
| `getElementsByClassName('class')` | `HTMLCollection` | Live | No `.forEach()`, convert with `Array.from()` |
| `getElementsByTagName('tag')` | `HTMLCollection` | Live | No `.forEach()`, convert with `Array.from()` |

### 2. DOM Traversal Methods
| Property | Direction | Description |
| :--- | :--- | :--- |
| `element.children` | Parent &rarr; Children | `HTMLCollection` of child element nodes only |
| `element.childNodes` | Parent &rarr; Children | `NodeList` of all child nodes (includes whitespace & comments) |
| `element.firstElementChild` | Parent &rarr; First Child | First child element node |
| `element.lastElementChild` | Parent &rarr; Last Child | Last child element node |
| `element.parentElement` | Child &rarr; Parent | Parent element node |
| `element.nextElementSibling` | Sibling &rarr; Next Sibling | Next adjacent element |
| `element.previousElementSibling` | Sibling &rarr; Prev Sibling | Previous adjacent element |

### 3. Top 8 Interview Takeaways
1. **`innerText` vs `textContent`**: `innerText` returns only visible rendered text (respects CSS `display: none` and triggers reflow), while `textContent` returns all text regardless of CSS.
2. **`HTMLCollection` vs `NodeList`**: `NodeList` has built-in `.forEach()`; `HTMLCollection` does **not**. Convert collections using `Array.from(col)` or `[...col]`.
3. **`children` vs `childNodes`**: `children` counts only HTML element tags; `childNodes` includes text nodes created by enter/newline whitespace.
4. **`createElement()` + `createTextNode()`**: Preferred over `innerHTML` for creating elements because it avoids invoking the HTML parser and prevents XSS vulnerabilities.
5. **`replaceWith()` & `remove()`**: Modern native methods for DOM manipulation that don't require navigating up to the parent element first.
6. **`outerHTML` vs `innerHTML`**: `innerHTML` replaces content *inside* the tags; `outerHTML` replaces the *element tag itself* plus its content.
7. **`className` vs `class`**: In DOM property access, use `.className` (since `class` is a reserved JavaScript keyword).
8. **Inline Styling**: `.style` properties use camelCase (e.g., `backgroundColor`, not `background-color`).

