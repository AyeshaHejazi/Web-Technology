# JavaScript Notes

## 1. Variables

**Concept:** `let`, `const`, and `var` declare variables with different scoping rules.

**Syntax:**
```javascript
let count = 0;       // block-scoped, reassignable
const name = "Ayesha"; // block-scoped, cannot be reassigned
var old = true;       // function-scoped, avoid in modern code
```

**Gotchas:**
- `const` prevents reassignment, not mutation — `const arr = []; arr.push(1);` is fine.
- `var` is hoisted and function-scoped, which causes bugs inside loops/conditionals — prefer `let`/`const` always.

---

## 2. Functions

**Concept:** Reusable blocks of logic — declared multiple ways with different behaviors.

**Syntax:**
```javascript
function greet(name) {
    return `Hello, ${name}`;
}

const greetArrow = (name) => `Hello, ${name}`;

const obj = {
    name: "Ayesha",
    greet() { return `Hi, ${this.name}`; }
};
```

**Gotchas:**
- Arrow functions don't have their own `this` — they inherit it from the enclosing scope. Don't use them as object methods if you need `this` to refer to the object.
- Function declarations are hoisted (usable before defined in code); function expressions/arrow functions are not.

---

## 3. Arrays & Common Methods

**Concept:** Ordered collections with built-in methods for transformation.

**Syntax:**
```javascript
const nums = [1, 2, 3, 4];

nums.map(n => n * 2);        // [2,4,6,8] — transform
nums.filter(n => n % 2 === 0); // [2,4] — select
nums.reduce((acc, n) => acc + n, 0); // 10 — accumulate
nums.forEach(n => console.log(n));  // side effects, no return
```

**Gotchas:**
- `map`/`filter`/`reduce` return new arrays/values — they don't mutate the original.
- `forEach` always returns `undefined` — don't try to chain after it.
- `sort()` mutates in place and sorts as strings by default (`[10, 2].sort()` → `[10, 2]` stays wrong order) — pass a compare function: `.sort((a,b) => a-b)`.

---

## 4. Objects & Destructuring

**Concept:** Key-value data structures; destructuring pulls values out concisely.

**Syntax:**
```javascript
const user = { name: "Ayesha", role: "developer" };
const { name, role } = user;

const [first, second] = [10, 20];
```

**Gotchas:**
- Destructuring a property that doesn't exist gives `undefined`, not an error.
- Nested destructuring can get unreadable fast — sometimes plain dot access is clearer.

---

## 5. Asynchronous JavaScript

**Concept:** Handling operations that take time (API calls, timers) without blocking execution.

**Syntax:**
```javascript
// Promises
fetch("/api/data")
    .then(res => res.json())
    .then(data => console.log(data))
    .catch(err => console.error(err));

// Async/await
async function getData() {
    try {
        const res = await fetch("/api/data");
        const data = await res.json();
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}
```

**Gotchas:**
- `await` only works inside an `async` function (or top-level in modules).
- Forgetting `try/catch` around `await` means failed requests throw unhandled errors.
- `fetch()` doesn't reject on HTTP error status codes (404, 500) — only on network failure. Check `res.ok` manually.

---

## 6. DOM Interaction Basics

**Concept:** JavaScript reads/modifies the page via the DOM API.

**Syntax:**
```javascript
const btn = document.querySelector("#submit");
btn.addEventListener("click", (e) => {
    e.preventDefault();
    console.log("Clicked!");
});
```

**Gotchas:**
- `querySelector` returns the *first* match; `querySelectorAll` returns a NodeList (not a full array — no `.map()` without `[...list]` or `Array.from()`).
- Event listeners added before the DOM is ready silently fail to attach — wrap in `DOMContentLoaded` or place scripts at the end of `<body>`.

---

## 7. Equality & Type Coercion

**Concept:** JS has both strict (`===`) and loose (`==`) equality.

**Syntax:**
```javascript
0 == "0"   // true  (coerces types)
0 === "0"  // false (checks type too)
```

**Gotchas:**
- Always default to `===`/`!==` unless you have a specific reason for loose comparison.
- `NaN === NaN` is `false` — use `Number.isNaN()` to check.

---

## Quick Reference: Common Mistakes
- Confusing `map` (returns new array) with `forEach` (returns nothing).
- Not handling promise rejections.
- Relying on `var` and running into scoping bugs.
- Comparing objects/arrays with `===` and expecting deep equality (it checks reference, not contents).
