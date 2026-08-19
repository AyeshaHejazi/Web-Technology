# HTML Notes

## 1. Document Structure

**Concept:** Every HTML page needs a base skeleton that tells the browser how to interpret the content.

**Syntax:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page Title</title>
</head>
<body>
    <!-- content -->
</body>
</html>
```

**Gotchas:**
- Forgetting the viewport meta tag breaks mobile responsiveness.
- `<!DOCTYPE html>` must be the very first line — no whitespace before it.

---

## 2. Forms

**Concept:** Forms collect user input and can submit it to a server or be handled with JavaScript.

**Syntax:**
```html
<form action="/submit" method="POST">
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" required>

    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>

    <button type="submit">Submit</button>
</form>
```

**Gotchas:**
- `for` on `<label>` must match the `id` of its input — this is what makes clicking the label focus the field.
- `name` attribute is what gets sent in form data; `id` is just for DOM/CSS targeting. Missing `name` = field silently not submitted.
- `type="submit"` vs `type="button"`: `submit` triggers form submission, `button` does nothing by default.

---

## 3. Semantic Elements

**Concept:** Tags that describe meaning, not just appearance — improves accessibility and SEO.

**Syntax:**
```html
<header>...</header>
<nav>...</nav>
<main>
    <article>...</article>
    <section>...</section>
</main>
<footer>...</footer>
```

**Gotchas:**
- `<div>` and `<span>` carry no meaning — prefer semantic tags where one fits.
- `<section>` should generally contain a heading; if it doesn't, a `<div>` is often more appropriate.

---

## 4. Tables

**Concept:** For tabular data only — not layout (use CSS Grid/Flexbox for layout).

**Syntax:**
```html
<table>
    <thead>
        <tr><th>Name</th><th>Score</th></tr>
    </thead>
    <tbody>
        <tr><td>Ayesha</td><td>95</td></tr>
    </tbody>
</table>
```

**Gotchas:**
- `<th>` inside `<thead>` for headers; `<td>` for data cells.
- Old-school table-based layouts are an anti-pattern today.

---

## 5. Frames (legacy, but in your practice list)

**Concept:** Splits the browser window into multiple independently loadable sections.

**Syntax:**
```html
<frameset cols="25%,75%">
    <frame src="nav.html">
    <frame src="content.html">
</frameset>
```

**Gotchas:**
- `<frameset>`/`<frame>` are deprecated in HTML5 — not usable inside a normal `<body>`.
- Modern equivalent: `<iframe>` embedded within a normal page, or CSS Grid/Flexbox layouts.

---

## 6. Common Input Types

| Type | Use case |
|---|---|
| `text` | Single-line text |
| `email` | Validates email format |
| `password` | Masks input |
| `number` | Numeric input with spinners |
| `date` | Date picker |
| `checkbox` | Multiple selectable options |
| `radio` | Single choice from a group |
| `file` | File upload |

**Gotcha:** Radio buttons need the *same* `name` attribute across the group to work as a set; checkboxes don't.

---

## Quick Reference: Common Mistakes
- Not closing self-closing-looking tags like `<img>`, `<input>`, `<br>` consistently (HTML5 doesn't require the `/`, but be consistent).
- Nesting block-level elements inside inline elements (e.g., `<div>` inside `<span>`) — invalid.
- Using multiple `<h1>` tags per page without a good reason — hurts document outline/SEO.
