# Section 3 — Display & Positioning

Before Flexbox and Grid existed, CSS layouts were built on **two core concepts**:

1. **`display`** — decides *how* an element takes up space
2. **`position`** — decides *where* an element sits

Even today, modern layouts still use these constantly (for navbars, modals, dropdowns, tooltips, overlays, sticky headers, etc). This section covers everything you need to master them.

See also: `vertical-horizontal-layouts.png` — the original exercise image included in the course folder.

---

## Table of Contents

| #  | Topic                                         | Example File                      |
| -- | --------------------------------------------- | --------------------------------- |
| 1  | `display`: block / inline / inline-block      | `01-display.html`                 |
| 2  | `display: none` vs `visibility: hidden`       | `02-hiding-elements.html`         |
| 3  | The Box Model                                 | `03-box-model.html`               |
| 4  | `box-sizing: border-box`                      | `04-box-sizing.html`              |
| 5  | `position: static` / `relative`               | `05-position-relative.html`       |
| 6  | `position: absolute`                          | `06-position-absolute.html`       |
| 7  | `position: fixed`                             | `07-position-fixed.html`          |
| 8  | `position: sticky`                            | `08-position-sticky.html`         |
| 9  | `z-index` & Stacking                          | `09-z-index.html`                 |
| 10 | `float` & `clear`                             | `10-float.html`                   |
| 11 | `overflow`                                    | `11-overflow.html`                |
| 12 | 🎁 Real-world: navbar + modal + dropdown      | `12-real-world.html`              |

---

## 1. The `display` Property

Every HTML element has a default `display` value. There are 3 big ones every developer must know.

### Block elements (`display: block`)
- Take up the **full width** of their parent
- Always start on a **new line**
- Respect `width`, `height`, `margin`, `padding`
- Examples: `<div>`, `<p>`, `<h1>`, `<section>`, `<ul>`, `<li>`

### Inline elements (`display: inline`)
- Take up **only as much width as their content**
- Stay on the **same line** as other inline elements
- ❌ **Ignore `width` and `height`**
- Respect horizontal margin/padding, but vertical margin doesn't push siblings
- Examples: `<span>`, `<a>`, `<strong>`, `<em>`, `<img>`*

### Inline-Block (`display: inline-block`)
- Best of both worlds: stays inline BUT respects `width` and `height`
- Useful when you want things on one line but with custom sizes

### Quick comparison

| Feature                | block | inline | inline-block |
| ---------------------- | :---: | :----: | :----------: |
| Takes full width       |  ✅   |   ❌   |      ❌      |
| New line before/after  |  ✅   |   ❌   |      ❌      |
| Respects `width`/`height` | ✅ |   ❌   |      ✅      |

👉 See `01-display.html`.

---

## 2. Hiding Elements

Three ways to "hide" something — they behave very differently!

| Method                    | Still takes space? | Clickable? | Accessible to screen readers? |
| ------------------------- | :----------------: | :--------: | :---------------------------: |
| `display: none`           |        ❌          |     ❌     |              ❌               |
| `visibility: hidden`      |        ✅          |     ❌     |              ❌               |
| `opacity: 0`              |        ✅          |     ✅     |              ✅               |

💡 **Rule of thumb:**
- `display: none` → element is completely removed from layout
- `visibility: hidden` → element is invisible but still holds its space
- `opacity: 0` → invisible but still interactive (good for fade animations)

👉 See `02-hiding-elements.html`.

---

## 3. The Box Model

Every element in CSS is a box made of 4 layers, from inside out:

```
   ┌──────────────────────────────────────┐
   │            margin                     │
   │   ┌──────────────────────────────┐   │
   │   │         border               │   │
   │   │   ┌──────────────────────┐   │   │
   │   │   │     padding          │   │   │
   │   │   │   ┌──────────────┐   │   │   │
   │   │   │   │   content    │   │   │   │
   │   │   │   └──────────────┘   │   │   │
   │   │   └──────────────────────┘   │   │
   │   └──────────────────────────────┘   │
   └──────────────────────────────────────┘
```

- **content** — the actual text/image
- **padding** — space INSIDE the border (between content and border)
- **border** — the outline
- **margin** — space OUTSIDE the border (between this element and neighbors)

👉 See `03-box-model.html`.

---

## 4. `box-sizing: border-box` — the trick pros add to every project

### The problem
By default, `width` only sets the **content width**. Padding and border are ADDED on top.

```css
.box {
  width: 200px;
  padding: 20px;
  border: 5px solid red;
}
/* Actual rendered width = 200 + 20*2 + 5*2 = 250px ❌ */
```

### The fix
`box-sizing: border-box` makes `width` include padding + border.

```css
.box {
  box-sizing: border-box;
  width: 200px;
  padding: 20px;
  border: 5px solid red;
}
/* Actual rendered width = 200px ✅ */
```

### The global reset every pro uses
```css
* {
  box-sizing: border-box;
}
```

Just set this once at the top of your CSS and your life gets 10× easier.

👉 See `04-box-sizing.html`.

---

## 5. `position` — The 5 Values

| Value       | Behavior                                                               |
| ----------- | ---------------------------------------------------------------------- |
| `static` ⭐ (default) | Normal flow. `top/left/etc` are ignored.                        |
| `relative`  | Normal flow, but now `top/left/right/bottom` SHIFT the element.        |
| `absolute`  | Removed from flow. Positioned relative to nearest **positioned** ancestor. |
| `fixed`     | Removed from flow. Positioned relative to the **viewport** (screen).   |
| `sticky`    | Hybrid — normal until you scroll past it, then it sticks.              |

### 5a. `position: static` (default)
The element is in normal flow. `top`, `left`, `right`, `bottom` **do nothing**.

### 5b. `position: relative`
The element stays in normal flow (still takes up space where it was), BUT you can now shift it visually using `top`/`left`/`right`/`bottom`.

```css
.box {
  position: relative;
  top: 20px;       /* moves DOWN 20px from where it would be */
  left: 50px;      /* moves RIGHT 50px from where it would be */
}
```

**Most important use:** making a parent a "positioning context" for absolute children.

👉 See `05-position-relative.html`.

### 5c. `position: absolute` — the #1 source of bugs!

This is powerful but tricky:

1. Element is **removed from normal flow** (neighbors act like it doesn't exist)
2. It's positioned relative to the **nearest ancestor that has `position` set to anything except `static`**
3. If no ancestor is positioned, it positions relative to the `<html>` element

### 💡 The classic pattern
```css
.parent {
  position: relative;   /* this makes it the "anchor" */
}
.child {
  position: absolute;
  top: 0;
  right: 0;             /* child pinned to top-right of parent */
}
```

**Common uses:**
- Badge on a card
- Close button on a modal
- Dropdown menu under a button
- Tooltip on an icon

👉 See `06-position-absolute.html`.

### 5d. `position: fixed`
Always positioned relative to the **viewport** (the visible screen). Stays in place when you scroll.

**Common uses:**
- Sticky headers that never move
- Floating "back to top" buttons
- Chat widgets in the corner
- Modal overlays

```css
.back-to-top {
  position: fixed;
  bottom: 20px;
  right: 20px;
}
```

👉 See `07-position-fixed.html`.

### 5e. `position: sticky` — the new hero
Behaves like `relative` until you scroll past a threshold, then acts like `fixed`.

```css
.section-header {
  position: sticky;
  top: 0;   /* sticks to top when scrolling past */
}
```

**Common uses:**
- Section headers in a long article
- Table headers that stay visible while scrolling
- Sidebars that follow as you scroll

👉 See `08-position-sticky.html`.

---

## 6. `z-index` — Who Shows on Top?

When elements overlap (using `position`, `transform`, etc), `z-index` decides who's on top.

```css
.lower { z-index: 1; }
.higher { z-index: 10; }   /* appears ON TOP */
```

### ⚠️ Rules of z-index
1. **`z-index` only works on positioned elements** (anything except `position: static`).
2. Higher number = on top.
3. **Stacking contexts** — a `z-index: 9999` inside one stacking context can still appear BELOW a `z-index: 1` in a different stacking context!

### What creates a new stacking context?
- `position: relative/absolute/fixed/sticky` + a `z-index` value
- `opacity < 1`
- `transform`, `filter`, `will-change`
- And many more...

This is why your `z-index: 9999` sometimes doesn't work. The fix: make the correct ancestor a stacking context.

👉 See `09-z-index.html`.

---

## 7. `float` and `clear` (legacy but still useful)

Before Flexbox, `float` was used for almost everything. Today it has one main job: **wrapping text around images**.

```css
img {
  float: left;
  margin-right: 10px;
}
/* Text flows around the image */
```

### The clearfix problem
Floated elements are removed from normal flow — their parent collapses to zero height.

**Modern fix:**
```css
.parent {
  display: flow-root;  /* clean, no hacks */
}
```

**Classic clearfix hack (good to know):**
```css
.parent::after {
  content: "";
  display: block;
  clear: both;
}
```

👉 See `10-float.html`.

💡 **In 2026:** Use floats only for text-wrapping. For layouts, use Flexbox/Grid (see Sections 4 & 5).

---

## 8. `overflow` — What Happens When Content Is Too Big?

```css
.box {
  overflow: visible;   /* default — content spills out */
  overflow: hidden;    /* clipped, no scrollbar */
  overflow: scroll;    /* always shows scrollbars */
  overflow: auto;      /* scrollbars only if needed ⭐ (best default) */
}
```

You can control axes separately:
```css
overflow-x: hidden;
overflow-y: auto;
```

👉 See `11-overflow.html`.

---

## 🧠 Cheat Sheet

| Need to...                                 | Use                                                      |
| ------------------------------------------ | -------------------------------------------------------- |
| Put a badge on the corner of a card        | Parent `position: relative` + child `position: absolute` |
| Navbar that stays at top when scrolling    | `position: fixed; top: 0` OR `position: sticky; top: 0`  |
| Back-to-top button                         | `position: fixed; bottom: 20px; right: 20px`             |
| Modal overlay                              | `position: fixed; inset: 0` with high `z-index`          |
| Dropdown under a button                    | Button `position: relative` + menu `position: absolute`  |
| Text wrapping around image                 | `img { float: left; margin-right: 10px; }`               |
| Scrollable content area                    | `overflow: auto` with a fixed `height` or `max-height`   |
| Make width include padding & border        | `* { box-sizing: border-box; }`                          |
| Hide element completely (no space)         | `display: none`                                          |
| Hide element but keep space                | `visibility: hidden`                                     |
| Hide but keep interactive (fade in/out)    | `opacity: 0`                                             |

---

## ⚠️ Common Mistakes

1. **`z-index` not working** — you forgot to set `position` on the element.
2. **`position: absolute` behaves weirdly** — you forgot to put `position: relative` on the parent.
3. **Padding makes my element too big** — you didn't use `box-sizing: border-box`.
4. **`display: none` vs `visibility: hidden`** — pick the right one! Do you want the space or not?
5. **Using `float` for layouts in 2026** — stop. Use Flexbox or Grid.
6. **Forgetting `overflow: hidden` on a rounded-corner parent** — child images may poke outside the border-radius.

---

## 🎓 Learning Order

Open the HTML files in this order:

1. `01-display.html` — foundations
2. `02-hiding-elements.html`
3. `03-box-model.html`
4. `04-box-sizing.html` — the pro trick
5. `05-position-relative.html`
6. `06-position-absolute.html` — the most important one ⭐
7. `07-position-fixed.html`
8. `08-position-sticky.html`
9. `09-z-index.html`
10. `10-float.html`
11. `11-overflow.html`
12. `12-real-world.html` — 🎁 everything combined in a real UI

Once you're comfortable with everything here, move on to Section 4 (Flexbox) and Section 5 (Grid) for modern layout!
