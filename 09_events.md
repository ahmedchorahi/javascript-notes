# JavaScript Events: Master Interview & Revision Guide

An **Event** in JavaScript is a notification sent by the browser to your script indicating that something significant has occurred—such as a user clicking a button, pressing a key, moving the mouse, or a resource finishing loading.

JavaScript uses an **event-driven architecture**, allowing scripts to listen for specific events and execute callback functions (**event handlers / event listeners**) in response.

---

## 1. Reference HTML Structure

This markup serves as the working example for all scenarios in these notes (image gallery with event delegation, propagation, and default prevention):

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>DOM Events Practice - Image Gallery</title>
</head>
<body style="background-color: #212121; color: #fff;">
  <h2>Amazing Images (DOM Events Practice)</h2>

  <ul id="images">
    <li id="item-1"><img id="photo-1" src="https://images.pexels.com/photos/18968399/pexels-photo-18968399.jpeg" alt="Photo 1" data-placeholder="Photo 1"></li>
    <li id="item-2"><img id="photo-2" src="https://images.pexels.com/photos/18968399/pexels-photo-18968399.jpeg" alt="Photo 2" data-placeholder="Photo 2"></li>
    <li id="item-3"><img id="photo-3" src="https://images.pexels.com/photos/5669817/pexels-photo-5669817.jpeg" alt="Photo 3" data-placeholder="Photo 3"></li>
    <li id="item-4"><img id="photo-4" src="https://images.pexels.com/photos/8715129/pexels-photo-8715129.jpeg" alt="Photo 4" data-placeholder="Photo 4"></li>
    <li id="item-5"><img id="photo-5" src="https://images.pexels.com/photos/29279361/pexels-photo-29279361.jpeg" alt="Photo 5" data-placeholder="Photo 5"></li>
    <li id="item-google" class="link-item"><a id="google" href="https://www.google.com" target="_blank">Visit Google</a></li>
  </ul>
</body>
</html>
```

---

## 2. Evolution of Event Handlers: 3 Ways to Attach Listeners

In JavaScript, there are 3 main approaches to listen for events. Interviewers frequently test why modern development strictly uses the 3rd approach.

### Approach 1: Inline HTML Event Attribute (Deprecated / Anti-pattern)
```html
<img id="photo-5" src="..." onclick="alert('photo 5 clicked')">
```
* **Why to avoid**:
  * Violates **Separation of Concerns** (mixes JS logic inside HTML structure).
  * Hard to maintain in large codebases.
  * Security concerns (violates strict Content Security Policy - CSP).
  * Can only bind a single statement or function.

---

### Approach 2: Traditional DOM Property
```javascript
const photo5 = document.querySelector('#photo-5');

photo5.onclick = function() {
  console.log("First listener");
};

// Overwrites the first handler!
photo5.onclick = function() {
  console.log("Second listener (First is lost!)");
};
```
* **Limitation**: Only **one** handler can be assigned per event type. Assigning a new function overwrites the previous one. Does not allow specifying bubbling vs. capturing.

---

### Approach 3: Modern `addEventListener` (Standard Best Practice)
```javascript
const photo5 = document.querySelector('#photo-5');

photo5.addEventListener('click', function(e) {
  console.log("Handler 1 executed");
}, false);

photo5.addEventListener('click', function(e) {
  console.log("Handler 2 executed");
}, false);
```

#### Syntax:
```javascript
target.addEventListener(eventType, callbackFunction, optionsOrUseCapture);
```

#### Why `addEventListener` is superior:
1. **Multiple Handlers**: Can attach multiple independent listeners to the same event on the same element.
2. **Propagation Control**: Supports the 3rd parameter (`useCapture: boolean` or options object).
3. **Advanced Options**: Supports `{ once: true, passive: true, signal: abortSignal }`.
4. **Clean Removal**: Handlers can be cleanly detached using `removeEventListener`.

---

## 3. The Event Object (`e` / `event`)

When an event triggers, the browser automatically creates an **Event Object** and passes it as the first argument to the callback function. It contains comprehensive metadata about the event.

```javascript
document.querySelector('#photo-5').addEventListener('click', (e) => {
  console.log(e); // MouseEvent object
});
```

### Essential Event Object Properties Reference Table

| Property | Type | Description | Example / Usage |
| :--- | :--- | :--- | :--- |
| `e.type` | `string` | The case-sensitive name of the event fired (e.g., `'click'`, `'keydown'`). | `if (e.type === 'click') { ... }` |
| `e.timeStamp` | `number` | Milliseconds elapsed from document creation to the event creation. | `const elapsed = e.timeStamp;` |
| `e.defaultPrevented` | `boolean` | `true` if `e.preventDefault()` was invoked on this event; otherwise `false`. | `if (!e.defaultPrevented) { ... }` |
| `e.target` | `Element` | The **deepest / innermost element** that originated the event (where the user actually clicked). | `e.target.tagName === 'IMG'` |
| `e.currentTarget` | `Element` | The element to which the **event listener was attached** (`this` in non-arrow functions). | `e.currentTarget.id === 'images'` |
| `e.srcElement` | `Element` | Non-standard legacy alias for `e.target` (used in older Internet Explorer). | Use `e.target` in modern JS. |
| `e.toElement` | `Element` | Legacy IE property for mouse events indicating the element the cursor moved into. | Modern standard is `e.relatedTarget`. |

---

### Coordinate & Positioning Properties (Mouse / Pointer Events)

Interviewers love asking the exact differences between coordinate pairs:

```text
+-------------------------------------------------------------------+  <-- Screen Top (screenX, screenY)
| Monitor Screen                                                    |
|    +---------------------------------------------------------+    |
|    | Browser Window / Viewport (clientX, clientY)             |    |
|    |    +-----------------------------------------------+    |    |
|    |    | Full Document Page (pageX, pageY with scroll) |    |    |
|    |    |    +--------------------+                     |    |    |
|    |    |    | Element (offsetX,  |                     |    |    |
|    |    |    |          offsetY)  |                     |    |    |
|    |    |    +--------------------+                     |    |    |
|    |    +-----------------------------------------------+    |    |
|    +---------------------------------------------------------+    |
+-------------------------------------------------------------------+
```

| Coordinate Property | Measured Relative To | Considers Scroll? | Interview Explanation |
| :--- | :--- | :--- | :--- |
| **`clientX` / `clientY`** | **Browser Viewport** (visible area of current tab). | **No** (fixed to visible viewport). | Value stays between 0 and viewport width/height, regardless of page scroll. |
| **`pageX` / `pageY`** | **Entire HTML Document** from top-left. | **Yes** (`pageX = clientX + scrollLeft`). | Increases as the user scrolls down the page. |
| **`screenX` / `screenY`** | **Physical Monitor Screen** top-left corner. | **N/A** (physical display coordinates). | Measures distance from physical monitor border. |
| **`offsetX` / `offsetY`** | **Padding edge of the target element** itself. | **N/A** (local element coordinates). | Measures where inside the clicked element the cursor was positioned (0 to element width). |

```javascript
document.querySelector('#photo-5').addEventListener('click', (e) => {
  console.log(`Viewport: (${e.clientX}, ${e.clientY})`);
  console.log(`Document: (${e.pageX}, ${e.pageY})`);
  console.log(`Screen:   (${e.screenX}, ${e.screenY})`);
  console.log(`Element:  (${e.offsetX}, ${e.offsetY})`);
});
```

---

### Keyboard Events & Modifier Key Properties

| Property | Type | Description |
| :--- | :--- | :--- |
| `e.altKey` | `boolean` | `true` if the <kbd>Alt</kbd> (or <kbd>Option</kbd> on macOS) key was pressed during the event. |
| `e.ctrlKey` | `boolean` | `true` if the <kbd>Ctrl</kbd> key was pressed. |
| `e.shiftKey` | `boolean` | `true` if the <kbd>Shift</kbd> key was pressed. |
| `e.metaKey` | `boolean` | `true` if the <kbd>Meta</kbd> key was pressed (<kbd>Windows</kbd> key on Windows, <kbd>Command ⌘</kbd> on macOS). |
| `e.key` | `string` | The **character value** produced by the key (e.g., `'Enter'`, `'a'`, `'A'`, `'Escape'`). *(Modern standard)* |
| `e.code` | `string` | The **physical key code** on the keyboard (e.g., `'KeyA'`, `'Digit1'`, `'Enter'`). Does not change with keyboard layout. |
| `e.keyCode` | `number` | **Deprecated** numeric ASCII code (e.g., `13` for Enter, `27` for Esc). Avoid in modern code. |

```javascript
window.addEventListener('keydown', (e) => {
  if (e.ctrlKey && e.key === 's') {
    e.preventDefault(); // Prevent browser save dialog
    console.log("Custom save triggered!");
  }
});
```

---

## 4. Event Propagation: Bubbling & Capturing

When an event occurs on an element inside nested DOM containers, the browser doesn't just trigger that single element. The event travels through the DOM tree in **three distinct phases**:

```text
                      DOCUMENT
                         |
                1. CAPTURING PHASE (Trickling Down)
                         v
                    <ul id="images">
                         v
                    <li id="item-5">
                         v
                2. TARGET PHASE: <img id="photo-5">
                         |
                 3. BUBBLING PHASE (Bubbling Up)
                         |
                    <li id="item-5">
                         ^
                    <ul id="images">
                         ^
                      DOCUMENT
```

### The 3 Phases:
1. **Capturing Phase (Trickling)**: The event descends from `window` → `document` → parent elements down to the target.
2. **Target Phase**: The event reaches the actual target element that triggered the action.
3. **Bubbling Phase**: The event ascends from the target element up through all ancestors to `window`.

---

### Controlling the Phase via `useCapture` (3rd Parameter)

```javascript
// Third argument = false (DEFAULT: Event Bubbling)
document.querySelector('#images').addEventListener('click', (e) => {
  console.log('UL clicked (Bubbling)');
}, false);

document.querySelector('#photo-5').addEventListener('click', (e) => {
  console.log('Photo-5 clicked (Target)');
}, false);

// Clicking Photo 5 outputs:
// 1. "Photo-5 clicked (Target)"
// 2. "UL clicked (Bubbling)"
```

```javascript
// Third argument = true (Event Capturing)
document.querySelector('#images').addEventListener('click', (e) => {
  console.log('UL clicked (Capturing)');
}, true);

document.querySelector('#photo-5').addEventListener('click', (e) => {
  console.log('Photo-5 clicked (Target)');
}, false);

// Clicking Photo 5 outputs:
// 1. "UL clicked (Capturing)"   <-- Parent runs first!
// 2. "Photo-5 clicked (Target)"
```

---

### `e.stopPropagation()` vs `e.stopImmediatePropagation()` vs `e.preventDefault()`

| Method | What it Does | Does it stop bubbling? | Does it stop browser default? |
| :--- | :--- | :--- | :--- |
| **`e.stopPropagation()`** | Stops the event from traveling further up (or down) the DOM tree. | **Yes** | **No** |
| **`e.stopImmediatePropagation()`** | Stops bubbling **AND** prevents any other listeners attached to the *same* element on the same event from running. | **Yes** | **No** |
| **`e.preventDefault()`** | Cancels the browser's default action (e.g. following a link, submitting a form). | **No** | **Yes** |

#### Example: Stopping Propagation
```javascript
document.querySelector('#photo-5').addEventListener('click', (e) => {
  e.stopPropagation(); // Parent #images will NEVER receive this click event
  console.log('Only photo 5 handled this click!');
});
```

#### Example: Preventing Default Action
```javascript
document.querySelector('#google').addEventListener('click', (e) => {
  e.preventDefault(); // Prevents navigating to https://google.com
  console.log('Google link navigation blocked!');
});
```

---

## 5. Event Delegation (Crucial Architectural Pattern)

### What is Event Delegation?
Instead of adding individual event listeners to 100 different child elements (`<li>` or `<img>`), you attach a **single event listener** to their common parent container (`<ul id="images">`). Because events bubble up, the parent intercepts clicks from all its children.

### Benefits:
1. **Memory Optimization**: 1 listener in memory instead of hundreds.
2. **Dynamic Elements**: Automatically works for newly added children without re-binding listeners.

---

### The Practical Gallery Example: Deleting Clicked Images

#### Naive Implementation (Has a Bug!):
```javascript
const imagesList = document.querySelector('#images');

imagesList.addEventListener('click', (e) => {
  console.log("Clicked:", e.target.tagName);
  const removeIt = e.target.parentNode;
  removeIt.remove();
});
```

> [!CAUTION]
> **The Edge-Case Bug**:
> - If you click on an `<img>`, `e.target` is the `<img>`, and `e.target.parentNode` is the `<li>`. The `<li>` gets removed as intended.
> - **BUT** if you click on the `<li id="item-google">` or the gap/padding between list items, `e.target` is the `<li>`, so `e.target.parentNode` is the entire `<ul id="images">`!
> - The entire gallery gets destroyed!

---

### Robust Solution 1: Checking `e.target.tagName`
```javascript
const imagesList = document.querySelector('#images');

imagesList.addEventListener('click', (e) => {
  // Only remove if an image specifically was clicked
  if (e.target.tagName === 'IMG') {
    console.log(`Removing item with id: ${e.target.id}`);
    const listItem = e.target.parentNode;
    listItem.remove();
  }
});
```

---

### Robust Solution 2: Using `.closest()` (Production Best Practice)
```javascript
const imagesList = document.querySelector('#images');

imagesList.addEventListener('click', (e) => {
  // Find the closest ancestor list item
  const listItem = e.target.closest('li');

  // Prevent deleting the link item or clicks outside an li
  if (listItem && !listItem.classList.contains('link-item')) {
    listItem.remove();
  }
});
```

---

## 6. Removing Event Listeners & Preventing Memory Leaks

To prevent memory leaks in Single Page Applications (SPAs) like React, Vue, or vanilla JS components, listeners must be cleaned up when elements are destroyed.

```javascript
function handleClick(e) {
  console.log('Clicked!');
}

const photo1 = document.querySelector('#photo-1');

// Adding listener
photo1.addEventListener('click', handleClick);

// Removing listener (MUST use the exact same function reference)
photo1.removeEventListener('click', handleClick);
```

> [!WARNING]
> **Common Trap**: You **CANNOT** remove an anonymous function:
> ```javascript
> photo1.addEventListener('click', () => console.log('Hi'));
> photo1.removeEventListener('click', () => console.log('Hi')); // FAILS! Different memory reference!
> ```

---

## 7. Top 6 Interview Questions & Quick Answers

### Q1: What is the difference between `e.target` and `e.currentTarget`?
* **`e.target`**: The actual DOM element where the event originated (the clicked element).
* **`e.currentTarget`**: The DOM element that is currently handling the event (where `addEventListener` was registered). Inside a standard function, `this === e.currentTarget`.

---

### Q2: How does Event Bubbling differ from Event Capturing?
* **Capturing (Trickling)**: Event travels from top of document down to target element (`useCapture: true`).
* **Bubbling**: Event travels from target element up to document root (`useCapture: false` - default).

---

### Q3: What is Event Delegation and why is it useful?
* A pattern where a single event listener on a parent element handles events for all current and future child elements using bubbling.
* **Benefits**: Drastically reduces memory consumption, simplifies code, and automatically handles dynamically created elements.

---

### Q4: What does `e.preventDefault()` do, and does it stop bubbling?
* It prevents the browser's default behavior (e.g. submitting a form, checking a checkbox, following an `<a>` tag).
* It **does NOT** stop propagation/bubbling. To stop bubbling, you must call `e.stopPropagation()`.

---

### Q5: What is the difference between `clientX`, `pageX`, and `screenX`?
* **`clientX/Y`**: Position relative to visible **browser viewport**.
* **`pageX/Y`**: Position relative to **entire HTML document** (includes scroll offset).
* **`screenX/Y`**: Position relative to physical **monitor screen**.

---

### Q6: What does the `{ once: true }` option do in `addEventListener`?
```javascript
btn.addEventListener('click', handleInitialClick, { once: true });
```
* Automatically invokes `removeEventListener` immediately after the handler executes for the first time, ensuring the event runs only once without manual cleanup.
    