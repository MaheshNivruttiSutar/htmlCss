# Section 4 — CSS Flexbox Ultimate Guide

Flexbox is the easiest way to lay out elements in CSS. Before flexbox, developers used hacks like `float` + clearfixes. Flexbox made layouts simple, flexible, and responsive.

This section has **notes in simple language** + **live HTML examples** you can open in the browser.

---

## Table of Contents

| #  | Topic                   | Example File                    |
| -- | ----------------------- | ------------------------------- |
| 1  | Flexbox Basics          | `01-basics.html`                |
| 2  | `justify-content`       | `02-justify-content.html`       |
| 3  | `align-items`           | `03-align-items.html`           |
| 4  | `flex-direction`        | `04-flex-direction.html`        |
| 5  | `align-self`            | `05-align-self.html`            |
| 6  | `flex-grow`             | `06-flex-grow.html`             |
| 7  | `flex-basis`            | `07-flex-basis.html`            |
| 8  | `flex-shrink`           | `08-flex-shrink.html`           |
| 9  | `flex-wrap`             | `09-flex-wrap.html`             |
| 10 | `align-content`         | `10-align-content.html`         |
| 11 | `gap`                   | `11-gap.html`                   |
| 12 | `order`                 | `12-order.html`                 |
| 13 | `flex` shorthand        | `13-flex-shorthand.html`        |
| 14 | 🎁 Real world example    | `14-real-world-example.html`    |

---

## 1. What is Flexbox?

Flexbox has **two parts**:

1. **Flex container** — the parent element (where you write `display: flex`).
2. **Flex items** — the direct children inside that container.

```css
.container {
  display: flex;
}
```

That one line turns the parent into a flex container. All direct children become flex items and line up **horizontally, left to right** by default.

---

## 2. Main Axis vs Cross Axis (most important concept!)

Flexbox works on **two axes**:

- **Main axis** — horizontal by default (left → right).
- **Cross axis** — vertical by default (top → bottom).

Rule of thumb:
- `justify-content` → works on the **main axis**.
- `align-items` → works on the **cross axis**.

If you change direction with `flex-direction: column`, the axes **swap**.

```
 ┌───────────────────────────────────────┐
 │  main axis  →→→→→→→→→→→→→→→→→→→→→→→→  │  (horizontal by default)
 │                                       │
 │  cross axis ↓                         │  (vertical by default)
 │             ↓                         │
 └───────────────────────────────────────┘
```

---

## 3. `justify-content` — spacing along MAIN axis

Assume each item is `width: 20%` (so 3 items = 60% filled, 40% extra space).

| Value           | What it does                                                                   |
| --------------- | ------------------------------------------------------------------------------ |
| `flex-start` ⭐ (default) | Items stick to the **start** (left).                                   |
| `flex-end`      | Items stick to the **end** (right).                                            |
| `center`        | Items are **centered** horizontally. (Easiest way to center in CSS!)           |
| `space-between` | Extra space goes **between** items. First/last touch the edges.                |
| `space-around`  | Each item gets equal space around it. Edge gap = **half** the gap between items. |
| `space-evenly`  | All gaps (including edges) are **exactly equal**.                              |

```css
.container {
  display: flex;
  justify-content: center;
}
```

👉 See `02-justify-content.html` for all 6 values side-by-side.

---

## 4. `align-items` — spacing along CROSS axis

| Value           | What it does                                                              |
| --------------- | ------------------------------------------------------------------------- |
| `stretch` ⭐ (default) | Items stretch to fill container height (if no height is set).       |
| `flex-start`    | Items align to the **top**.                                               |
| `flex-end`      | Items align to the **bottom**.                                            |
| `center`        | Items are **centered** vertically.                                        |

```css
.container {
  display: flex;
  align-items: center;
}
```

👉 See `03-align-items.html`.

💡 **Pro tip — perfect centering in CSS:**
```css
display: flex;
justify-content: center;  /* horizontal */
align-items: center;      /* vertical */
```

---

## 5. `flex-direction` — change the axes

| Value             | Main axis       | Items flow                 |
| ----------------- | --------------- | -------------------------- |
| `row` ⭐ (default)  | horizontal (→) | left to right              |
| `row-reverse`     | horizontal (←)  | right to left              |
| `column`          | vertical (↓)    | top to bottom              |
| `column-reverse`  | vertical (↑)    | bottom to top              |

⚠️ When `flex-direction: column`, then `justify-content` controls **vertical** placement and `align-items` controls **horizontal** placement (the axes swap!).

👉 See `04-flex-direction.html`.

---

## 6. `align-self` — align just ONE item

Usually the container decides alignment via `align-items`. But if you want **one specific item** aligned differently, use `align-self` on that item.

```css
.container { display: flex; align-items: flex-start; }
.item:nth-child(2) { align-self: flex-end; } /* only this one goes bottom */
```

Valid values: `flex-start`, `flex-end`, `center`, `stretch`, `baseline`.

⚠️ There is **no `justify-self`** in flexbox — main-axis placement is always controlled by the container.

👉 See `05-align-self.html`.

---

## 7. Sizing Items — `flex-grow`, `flex-shrink`, `flex-basis`

These are the **most confusing but most powerful** properties. Let's break them down simply.

### 7a. `flex-grow` — how to EAT leftover space

Default: `0` (item does not grow).

If you set `flex-grow: 1` on an item, it takes **all remaining free space** in the container.

If multiple items have `flex-grow`, they share the leftover space in proportion.

```css
.item-1 { flex-grow: 2; }  /* gets 2/3 of leftover space */
.item-2 { flex-grow: 1; }  /* gets 1/3 of leftover space */
```

**Math:** `this item's share = flex-grow ÷ total flex-grow of all items`

👉 See `06-flex-grow.html`.

### 7b. `flex-basis` — the item's starting size

Default: `auto` (uses the `width` property).

`flex-basis` is the **base size** of an item *before* `flex-grow`/`flex-shrink` are applied.

Use case: make 3 items equal width even if their original widths differ.

```css
.item {
  flex-grow: 1;
  flex-basis: 0;   /* start all at size 0, then grow equally */
}
```

👉 See `07-flex-basis.html`.

### 7c. `flex-shrink` — how much to SHRINK when overflowing

Default: `1` (item will shrink if needed).

If total item widths > container width, items shrink. Set `flex-shrink: 0` on an item to **stop it from shrinking**.

```css
.item-1 { flex-shrink: 0; }  /* never shrinks */
.item-2 { flex-shrink: 2; }  /* shrinks twice as much as others */
```

👉 See `08-flex-shrink.html`.

### 🎯 Summary of sizing

| Property      | Default  | Think of it as             |
| ------------- | -------- | -------------------------- |
| `flex-grow`   | `0`      | "Should I eat free space?" |
| `flex-shrink` | `1`      | "Should I shrink if needed?" |
| `flex-basis`  | `auto`   | "What's my starting size?" |

---

## 8. `flex-wrap` — letting items go to the next line

Default: `nowrap` — everything tries to fit on one line.

| Value           | Behavior                                            |
| --------------- | --------------------------------------------------- |
| `nowrap` ⭐ (default) | All items on one line, shrink if needed.      |
| `wrap`          | Items wrap to a new line when they don't fit.       |
| `wrap-reverse`  | Items wrap, but new lines appear on the top.        |

```css
.container {
  display: flex;
  flex-wrap: wrap;
}
```

⚠️ If you're building a **2D grid layout**, use **CSS Grid**, not flexbox wrap.

👉 See `09-flex-wrap.html`.

---

## 9. `align-content` — space BETWEEN rows (when wrapping)

This only matters when `flex-wrap: wrap` is used and there are multiple rows.

It's like `justify-content`, but for the **cross axis** / between wrapped lines.

Values: `flex-start`, `flex-end`, `center`, `space-between`, `space-around`, `space-evenly`, `stretch`.

👉 See `10-align-content.html`.

---

## 10. `gap` — space between items (the easy way!)

Instead of margins, just use `gap`:

```css
.container {
  display: flex;
  gap: 10px;              /* gap between all items */
  /* or set individually: */
  row-gap: 10px;
  column-gap: 20px;
}
```

👉 See `11-gap.html`.

---

## 11. `order` — change visual order of items

Every item has a default `order: 0`. Lower orders appear first.

```css
.item-1 { order: 2; }  /* shown last */
.item-2 { order: 1; }  /* shown second */
.item-3 { order: 1; }  /* shown second */
```

⚠️ **Avoid `order` in real projects.** Screen readers follow HTML order, not visual order. Using `order` confuses accessibility.

👉 See `12-order.html`.

---

## 12. `flex` shorthand — set grow/shrink/basis in one line

```css
.item {
  flex: 1 0 10px;
  /* same as: */
  flex-grow: 1;
  flex-shrink: 0;
  flex-basis: 10px;
}
```

Common shorthand patterns:

| Shorthand      | grow | shrink | basis  | Meaning                              |
| -------------- | ---- | ------ | ------ | ------------------------------------ |
| `flex: 1`      | 1    | 1      | `0`    | Grow equally, shrink equally         |
| `flex: 2`      | 2    | 1      | `0`    | Grow 2× as much                      |
| `flex: auto`   | 1    | 1      | `auto` | Use `width`, grow/shrink as needed   |
| `flex: none`   | 0    | 0      | `auto` | Don't grow or shrink. Fixed size.    |
| `flex: 10px`   | 0    | 1      | `10px` | Starts at 10px width                 |

⚠️ Note: When using shorthand, `flex-basis` defaults to `0` (not `auto`).

👉 See `13-flex-shorthand.html`.

---

## 🧠 Quick Cheat Sheet

**On the container:**
- `display: flex`
- `flex-direction` — row/column
- `justify-content` — main axis spacing
- `align-items` — cross axis alignment
- `flex-wrap` — allow wrapping
- `align-content` — wrapped rows spacing
- `gap` — spacing between items

**On the items:**
- `align-self` — override alignment for one item
- `flex-grow` — share free space
- `flex-shrink` — share shrink space
- `flex-basis` — starting size
- `flex` — shorthand for grow/shrink/basis
- `order` — visual order (avoid!)

---

## 🏁 Perfect-Centering Trick (you'll use this forever)

```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
```

That's the simplest way to center **anything** in CSS. 🎉
