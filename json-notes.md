# JSON Notes

## 1. What JSON Is

**Concept:** JSON (JavaScript Object Notation) is a lightweight, text-based data format used for exchanging data — especially between a client and a server.

**Syntax:**
```json
{
    "name": "Ayesha",
    "role": "developer",
    "isStudent": true,
    "skills": ["HTML", "CSS", "JavaScript"],
    "address": {
        "city": "Meerut",
        "country": "India"
    }
}
```

**Gotchas:**
- JSON keys and string values **must** use double quotes — single quotes are invalid JSON.
- No trailing commas allowed after the last item.
- No comments allowed in valid JSON (unlike JS objects).
- `undefined`, functions, and `Date` objects aren't valid JSON values.

---

## 2. Converting Between JSON and JavaScript

**Concept:** JS provides built-in methods to convert JS objects to JSON text and back.

**Syntax:**
```javascript
const obj = { name: "Ayesha", role: "developer" };

// Object → JSON string
const jsonString = JSON.stringify(obj);

// JSON string → Object
const parsedObj = JSON.parse(jsonString);

// Pretty-print with indentation
JSON.stringify(obj, null, 2);
```

**Gotchas:**
- `JSON.parse()` throws a `SyntaxError` on invalid JSON — always wrap in `try/catch` when parsing external data (like API responses or user input).
- `JSON.stringify()` silently drops keys with `undefined` values or function values.
- Circular references (an object referencing itself) will throw an error on `stringify`.

---

## 3. Fetching JSON from an API

**Concept:** Most REST APIs return JSON; `fetch` + `.json()` parses it automatically.

**Syntax:**
```javascript
async function getUsers() {
    const res = await fetch("https://api.example.com/users");
    if (!res.ok) throw new Error(`HTTP error: ${res.status}`);
    const data = await res.json();
    return data;
}
```

**Gotchas:**
- `res.json()` itself returns a Promise — must be awaited or `.then()`-chained.
- Calling `.json()` twice on the same response will fail — the body stream can only be read once.

---

## 4. Working with Nested JSON

**Concept:** Real-world JSON is often deeply nested — arrays of objects, objects of arrays.

**Syntax:**
```javascript
const data = {
    students: [
        { name: "Ayesha", scores: [90, 85, 92] },
        { name: "Riya", scores: [78, 88, 95] }
    ]
};

// Get all student names
const names = data.students.map(s => s.name);

// Get average score for first student
const avg = data.students[0].scores.reduce((a, b) => a + b, 0) / data.students[0].scores.length;
```

**Gotchas:**
- Accessing a deeply nested property that doesn't exist throws `Cannot read properties of undefined`. Use optional chaining: `data?.students?.[0]?.name`.
- When rendering nested JSON to the DOM, always plan the loop structure (outer `.map()` for arrays, then handle nested arrays inside).

---

## Quick Reference: Common Mistakes
- Using single quotes in a `.json` file — will fail validation/parsing.
- Forgetting `try/catch` around `JSON.parse()` on untrusted or API data.
- Assuming an API always returns the expected shape — always check for `null`/missing fields before accessing nested properties.
