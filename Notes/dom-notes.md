# DOM (Document Object Model) Notes

## 1. What the DOM Is

**Concept:** The DOM is the browser's in-memory tree representation of the HTML page — JavaScript uses it to read and change what's on screen.

**Syntax:**
```javascript
console.log(document); // the whole document tree
console.log(document.body); // the <body> node
```

**Gotchas:**
- The DOM is not the same as the HTML source — it reflects the *current* state, including changes made by JS after page load.
- `document` is only fully available after the HTML has been parsed — scripts placed in `<head>` without `defer`/`DOMContentLoaded` may run too early.

---

## 2. Selecting Elements

**Concept:** Multiple ways to grab references to DOM nodes.

**Syntax:**
```javascript
document.getElementById("header");
document.querySelector(".card");        // first match
document.querySelectorAll(".card");     // all matches (NodeList)
document.getElementsByClassName("card"); // live HTMLCollection
```

**Gotchas:**
- `getElementsByClassName`/`getElementsByTagName` return **live** collections (auto-update as DOM changes); `querySelectorAll` returns a **static** snapshot.
- `querySelectorAll` result isn't a real array — use `Array.from(list)` or `[...list]` to use array methods like `.map()`.

---

## 3. Modifying Content & Attributes

**Concept:** Change what's displayed or an element's properties.

**Syntax:**
```javascript
const el = document.querySelector("#title");

el.textContent = "New Text";     // safe, treats as plain text
el.innerHTML = "<b>Bold</b>";    // parses as HTML — careful with user input

el.setAttribute("data-id", "42");
el.classList.add("active");
el.classList.toggle("hidden");
```

**Gotchas:**
- Using `innerHTML` with untrusted/user-provided input is an XSS risk — use `textContent` unless you specifically need to inject HTML.
- `classList` methods (`add`, `remove`, `toggle`, `contains`) are safer/cleaner than manually editing `className` as a string.

---

## 4. Creating & Removing Elements

**Concept:** Build new nodes dynamically and insert/remove them.

**Syntax:**
```javascript
const li = document.createElement("li");
li.textContent = "New item";
document.querySelector("ul").appendChild(li);

// Remove
li.remove();
```

**Gotchas:**
- `appendChild` moves a node if it already exists elsewhere in the DOM (rather than copying) — use `cloneNode(true)` if you need a duplicate.
- Repeatedly appending elements one at a time in a loop causes multiple reflows — for many items, build a `DocumentFragment` first and append once.

---

## 5. Events

**Concept:** Responding to user interaction (clicks, input, submit, etc.).

**Syntax:**
```javascript
document.querySelector("#btn").addEventListener("click", (event) => {
    event.preventDefault();
    console.log("clicked", event.target);
});
```

**Gotchas:**
- `event.target` is the actual element clicked; `event.currentTarget` is the element the listener is attached to — they differ with event delegation.
- Forgetting `event.preventDefault()` on a form submit button causes an unwanted page reload.
- Removing an event listener requires the *same function reference* used in `addEventListener` — anonymous inline functions can't be removed later.

---

## 6. Event Delegation

**Concept:** Attach one listener to a parent instead of many listeners to children — useful for dynamic lists.

**Syntax:**
```javascript
document.querySelector("ul").addEventListener("click", (e) => {
    if (e.target.tagName === "LI") {
        console.log("List item clicked:", e.target.textContent);
    }
});
```

**Gotchas:**
- Relies on event *bubbling* — doesn't work for events that don't bubble (e.g., `focus`, `blur` — use `focusin`/`focusout` instead).
- Great for lists where items are added/removed dynamically, since new items don't need their own listener.

---

## Quick Reference: Common Mistakes
- Manipulating the DOM before it's fully loaded (missing `DOMContentLoaded`).
- Using `innerHTML` where `textContent` would be safer.
- Querying the DOM repeatedly inside a loop instead of caching the reference once.
- Not removing event listeners when elements are removed, causing memory leaks in long-running apps.
