# Accessibility (a11y) Notes

## 1. Why It Matters

**Concept:** Accessibility means your site works for people using screen readers, keyboard-only navigation, low vision, or other assistive tech — it's also a strong signal of code quality to reviewers/recruiters.

**Gotchas:**
- Accessibility is not just for a small niche of users — think of it as a quality baseline, similar to responsive design.
- Retrofitting accessibility later is harder than building it in from the start (e.g., semantic HTML from day one).

---

## 2. Semantic HTML First

**Concept:** Screen readers rely heavily on correct HTML elements, not just visual appearance, to understand a page.

**Syntax:**
```html
<!-- Good -->
<button onclick="submitForm()">Submit</button>

<!-- Bad — visually a button, but a screen reader treats it as plain text -->
<div onclick="submitForm()">Submit</div>
```

**Gotchas:**
- A `<div>` styled to look like a button has no keyboard focus, no `Enter`/`Space` activation, and no announcement as a button to screen readers — always prefer real `<button>`, `<a>`, `<input>` elements.
- Use heading tags (`<h1>`–`<h6>`) in logical order — don't skip levels just for font size (use CSS for that instead).

---

## 3. Alt Text for Images

**Concept:** `alt` describes an image for people who can't see it.

**Syntax:**
```html
<img src="chart.png" alt="Bar chart showing sales increasing from January to June">

<!-- Decorative image — no meaningful content -->
<img src="divider.png" alt="">
```

**Gotchas:**
- Empty `alt=""` is correct for purely decorative images — screen readers skip it. Omitting `alt` entirely is different and worse (some readers announce the filename instead).
- Avoid redundant phrases like "image of..." — the screen reader already announces it's an image.

---

## 4. Labels & Form Accessibility

**Concept:** Every input needs an accessible label, not just a visual one.

**Syntax:**
```html
<label for="email">Email</label>
<input type="email" id="email" name="email">

<!-- Or wrap it -->
<label>
    Email
    <input type="email" name="email">
</label>
```

**Gotchas:**
- Using placeholder text *instead of* a `<label>` is a common but real accessibility failure — placeholders disappear once typing starts and aren't reliably announced.
- Error messages should be programmatically associated with their field (`aria-describedby`), not just shown visually nearby.

---

## 5. Keyboard Navigation

**Concept:** Every interactive element should be reachable and usable via keyboard alone (Tab, Enter, Space, arrow keys).

**Test:** Try navigating your own project using only the Tab key — no mouse.

**Gotchas:**
- `tabindex="-1"` removes an element from keyboard tab order — use sparingly and intentionally.
- Custom dropdowns/modals built with `<div>`s often trap or lose keyboard focus if not handled carefully — this is one of the most common real-world a11y bugs.

---

## 6. Color Contrast

**Concept:** Text needs enough contrast against its background to be readable for users with low vision or color blindness.

**Guideline:** WCAG AA requires at least 4.5:1 contrast ratio for normal text, 3:1 for large text.

**Gotchas:**
- Light gray text on a white background (a common "modern minimal" look) often fails contrast checks — worth checking with a contrast checker tool.
- Don't rely on color alone to convey meaning (e.g., red text = error) — pair it with an icon or text label too.

---

## 7. ARIA (use sparingly)

**Concept:** ARIA attributes fill gaps when semantic HTML alone isn't enough — but the first rule of ARIA is "don't use ARIA if a native HTML element already does the job."

**Syntax:**
```html
<div role="alert">Form submitted successfully!</div>
<button aria-expanded="false" aria-controls="menu">Menu</button>
```

**Gotchas:**
- Adding ARIA roles doesn't add behavior (keyboard handling, focus) — you still have to implement that in JS yourself.
- Wrong or redundant ARIA (e.g., `role="button"` on an actual `<button>`) can confuse screen readers more than help.

---

## Quick Reference: Common Mistakes
- Styling a `<div>` to act like a button/link instead of using the real element.
- Using placeholder text as the only label for a form field.
- Images missing `alt` attributes entirely.
- Custom UI components (dropdowns, modals, tabs) that can't be operated by keyboard.
- Low-contrast text that looks fine to the designer but fails for many real users.
