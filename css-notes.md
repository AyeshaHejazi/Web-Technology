# CSS Notes

## 1. Selectors

**Concept:** Selectors target which HTML elements a rule applies to.

**Syntax:**
```css
/* Element */
p { color: black; }

/* Class */
.card { padding: 1rem; }

/* ID */
#header { background: navy; }

/* Descendant / Child */
.nav a { text-decoration: none; }
.nav > li { display: inline-block; }

/* Attribute */
input[type="text"] { border: 1px solid gray; }

/* Pseudo-class / pseudo-element */
a:hover { color: red; }
p::first-line { font-weight: bold; }
```

**Gotchas:**
- Specificity order (low → high): element < class < ID < inline style < `!important`.
- Overusing IDs for styling makes overrides painful later — prefer classes.
- `>` means direct child only; a space means any descendant.

---

## 2. Box Model

**Concept:** Every element is a box made of content, padding, border, and margin.

**Syntax:**
```css
.box {
    width: 200px;
    padding: 16px;
    border: 2px solid black;
    margin: 10px;
    box-sizing: border-box;
}
```

**Gotchas:**
- By default, `width` only covers content — padding/border add on top (`content-box`).
- `box-sizing: border-box` makes `width` include padding+border — usually what you want, and worth setting globally: `* { box-sizing: border-box; }`.
- Margins between vertical siblings can "collapse" (the larger one wins, they don't add).

---

## 3. Flexbox

**Concept:** One-dimensional layout system — great for rows/columns of items.

**Syntax:**
```css
.container {
    display: flex;
    justify-content: space-between; /* main axis */
    align-items: center;            /* cross axis */
    gap: 1rem;
}
.item {
    flex: 1; /* grow/shrink/basis shorthand */
}
```

**Gotchas:**
- `justify-content` controls the main axis, `align-items` the cross axis — easy to mix up.
- `flex-direction: column` flips which axis is "main," so `justify-content` then controls vertical alignment.
- `flex: 1` is shorthand for `flex-grow: 1; flex-shrink: 1; flex-basis: 0%`.

---

## 4. Grid

**Concept:** Two-dimensional layout system — rows and columns together.

**Syntax:**
```css
.grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1rem;
}
.item {
    grid-column: span 2;
}
```

**Gotchas:**
- `fr` unit divides remaining free space proportionally — not the same as `%`.
- Grid vs Flexbox: use Grid when you need control over both rows AND columns; Flexbox for a single row or column.

---

## 5. Positioning

**Concept:** Controls how an element is placed relative to its normal flow.

**Syntax:**
```css
.relative { position: relative; top: 10px; }
.absolute { position: absolute; top: 0; right: 0; }
.fixed { position: fixed; bottom: 0; }
.sticky { position: sticky; top: 0; }
```

**Gotchas:**
- `absolute` positions relative to the nearest ancestor with `position` set to anything other than `static` — forgetting to set that ancestor is a classic bug.
- `sticky` needs a defined threshold (`top`, `bottom`, etc.) and won't stick if any ancestor has `overflow: hidden`.

---

## 6. Responsive Design

**Concept:** Adjusting layout based on screen size.

**Syntax:**
```css
@media (max-width: 768px) {
    .nav { flex-direction: column; }
}
```

**Gotchas:**
- Mobile-first (`min-width` queries) vs desktop-first (`max-width` queries) — pick one approach and stay consistent.
- Without the viewport meta tag in HTML, media queries won't behave correctly on mobile.

---

## Quick Reference: Common Mistakes
- Forgetting `box-sizing: border-box`, leading to unexpected overflow.
- Using `!important` to patch specificity issues instead of fixing selector structure.
- Mixing units inconsistently (px, %, rem, em) without a clear system.
- Not testing layouts at multiple breakpoints, only resizing the browser slightly.
