# Section 5 — CSS Grid Ultimate Notes

CSS Grid is the most powerful layout system in CSS. It lets you build **two-dimensional layouts** (rows AND columns together), unlike Flexbox which is mainly one-dimensional.

This section has **simple notes** + **live HTML examples** you can open in the browser.

---

## 🎯 One Snippet You Must Memorize

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
}
```

That single snippet builds **responsive card layouts** with zero media queries. 90% of modern grid layouts start here.

---

## Table of Contents

| #  | Topic                               | Example File                         |
| -- | ----------------------------------- | ------------------------------------ |
| 1  | Grid Basics                         | `01-basics.html`                     |
| 2  | Columns & Rows                      | `02-columns-rows.html`               |
| 3  | The `fr` Unit                       | `03-fr-unit.html`                    |
| 4  | `repeat()` Function                 | `04-repeat.html`                     |
| 5  | `gap`, `row-gap`, `column-gap`      | `05-gap.html`                        |
| 6  | Grid Lines & `grid-column/row`      | `06-grid-lines.html`                 |
| 7  | `span` Keyword                      | `07-span.html`                       |
| 8  | `grid-template-areas`               | `08-template-areas.html`             |
| 9  | `minmax()`                          | `09-minmax.html`                     |
| 10 | `auto-fit` vs `auto-fill`           | `10-auto-fit-fill.html`              |
| 11 | Implicit Grid (`grid-auto-*`)       | `11-implicit-grid.html`              |
| 12 | Aligning Items (items vs content)   | `12-alignment.html`                  |
| 13 | 🎁 Real-world Page Layout            | `13-real-world.html`                 |

---

## 1. What is CSS Grid?

Grid is a CSS layout system designed for **two-dimensional layouts** — rows and columns together.

**Perfect for:**
- Page layouts (header/sidebar/main/footer)
- Dashboards
- Image galleries
- Card grids
- Anything where rows AND columns matter

**Use Flexbox for:**
- Navbars
- Button groups
- Single-axis centering

Activate Grid with `display: grid` on the parent:

```css
.container {
  display: grid;
}
```

The parent becomes the **grid container**. Its direct children become **grid items**.

---

## 2. Creating Columns and Rows

### Columns

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
}
```

This creates three columns, each 100px wide. Extra items automatically wrap to new rows.

### Rows

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 200px;
}
```

Three columns + two rows. First row 100px, second row 200px.

💡 **You usually only need to define columns** — rows are often auto-created.

---

## 3. The `fr` Unit

`fr` means **fraction of available space**. It's the single most useful grid unit.

```css
grid-template-columns: 1fr 1fr 1fr;   /* 3 equal columns */
grid-template-columns: 1fr 2fr 1fr;   /* middle gets half */
grid-template-columns: 250px 1fr;     /* fixed sidebar + flexible main */
```

**How the math works** — with `1fr 2fr 1fr`:
- Total = 1 + 2 + 1 = 4 parts
- Middle = 2/4 = **50%**
- Sides = 1/4 = **25%** each

---

## 4. `repeat()` — Write Less, Do More

Instead of:
```css
grid-template-columns: 1fr 1fr 1fr 1fr;
```

Write:
```css
grid-template-columns: repeat(4, 1fr);
```

You can mix with other values:
```css
grid-template-columns: 200px repeat(3, 1fr) 100px;
```

`repeat()` becomes incredibly powerful combined with `auto-fit` / `auto-fill` / `minmax()`.

---

## 5. `gap` — Spacing Between Items

```css
.grid {
  display: grid;
  gap: 20px;              /* space between all items */
  row-gap: 20px;          /* only between rows */
  column-gap: 40px;       /* only between columns */
  gap: 20px 40px;         /* row-gap column-gap shorthand */
}
```

✅ Use `gap` instead of item margins — cleaner, no extra edge space.

---

## 6. Grid Lines & Placing Items

Grid is built on invisible **grid lines**. 3 columns = **4 column lines**.

```
line 1     line 2     line 3     line 4
  |         |          |          |
  |  col 1  |  col 2   |  col 3   |
  |         |          |          |
```

### `grid-column` — horizontal placement

```css
.item {
  grid-column: 1 / 3;   /* starts at line 1, ends at line 3 → spans col 1 & 2 */
}
```

### `grid-row` — vertical placement

```css
.item {
  grid-row: 1 / 3;   /* spans rows 1 and 2 */
}
```

### The `-1` Trick (memorize this!)

```css
.header {
  grid-column: 1 / -1;   /* spans from first to LAST line = full width */
}
```

`-1` means "the last line". Works no matter how many columns you have.

---

## 7. `span` — Even Easier Placement

Instead of saying where to start AND end, just say "span N tracks":

```css
.item {
  grid-column: span 2;   /* takes 2 columns */
  grid-row: span 2;      /* takes 2 rows */
}
```

Combine for a 2×2 box:
```css
.featured {
  grid-column: span 2;
  grid-row: span 2;
}
```

---

## 8. `grid-template-areas` — Layouts You Can READ

This is the most beautiful Grid feature. You draw your layout using ASCII-like strings:

```css
.page {
  display: grid;
  grid-template-columns: 250px 1fr;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "header  header"
    "sidebar main"
    "footer  footer";
}

.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }
.footer  { grid-area: footer; }
```

Result:
```
| header  | header |
| sidebar | main   |
| footer  | footer |
```

### Empty cells with `.`
```css
grid-template-areas:
  "header  header header"
  ".       main   sidebar"   /* "." = empty cell */
  "footer  footer footer";
```

### ⚠️ Rule: areas must be RECTANGLES

```css
/* ✅ Valid (main is rectangular) */
grid-template-areas:
  "header header"
  "main   sidebar"
  "main   sidebar";

/* ❌ Invalid (main is diagonal) */
grid-template-areas:
  "main   sidebar"
  "sidebar main";
```

### Responsive magic

Just redefine the areas in a media query — no HTML changes:

```css
@media (max-width: 600px) {
  .page {
    grid-template-columns: 1fr;
    grid-template-areas:
      "header"
      "main"
      "sidebar"
      "footer";
  }
}
```

---

## 9. `minmax()` — Min + Max Sizing

```css
grid-template-columns: minmax(200px, 1fr) 2fr;
```

The first column:
- Can't be smaller than 200px
- Can grow up to 1fr

Prevents columns from squishing too small on narrow screens.

### Bonus: `minmax(0, 1fr)` fixes overflow!

Long text or images sometimes force grid columns to grow beyond the container. Fix:

```css
grid-template-columns: minmax(0, 1fr) minmax(0, 1fr);
```

Or on the child: `min-width: 0;`

---

## 10. `auto-fit` vs `auto-fill` — Responsive Magic

Both create "as many columns as can fit". The difference is what happens with **empty space**.

### `auto-fit` (most common)
```css
grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
```
When there aren't enough items to fill the row, existing items **stretch** to fill the gap.

### `auto-fill`
```css
grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
```
Keeps **empty tracks** so items don't stretch — they stay at their minimum size.

**Rule of thumb:** use `auto-fit` for card grids. Use `auto-fill` if you want predictable item sizes.

---

## 11. Implicit Grid (`grid-auto-rows`, `grid-auto-flow`)

When Grid creates rows/columns automatically (because you didn't define them), that's the **implicit grid**.

### `grid-auto-rows` — set size of auto-created rows

```css
.cards {
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 200px;   /* every auto-created row is 200px tall */
}
```

Flexible:
```css
grid-auto-rows: minmax(150px, auto);
```

### `grid-auto-flow` — how items fill

```css
grid-auto-flow: row;      /* default — left to right, top to bottom */
grid-auto-flow: column;   /* top to bottom, left to right */
grid-auto-flow: dense;    /* fills gaps with later items if they fit */
```

⚠️ `dense` can reorder items visually, which hurts accessibility. Use with care.

---

## 12. Alignment — items vs content

Grid has two families of alignment properties, and confusing them is the #1 Grid mistake.

### 🎯 Easy memory rule
- **`items`** = align each item INSIDE its own cell
- **`content`** = align the whole grid INSIDE the container

### Aligning items inside their cells

| Property        | What it does                                       |
| --------------- | -------------------------------------------------- |
| `justify-items` | Items → horizontally inside cell (on container)    |
| `align-items`   | Items → vertically inside cell (on container)      |
| `place-items`   | Shorthand for both                                 |
| `justify-self`  | One item → horizontally (on item itself)           |
| `align-self`    | One item → vertically (on item itself)             |
| `place-self`    | Shorthand for both (on item itself)                |

Values: `start`, `end`, `center`, `stretch` (default).

### Aligning the whole grid in the container

Only matters if the total grid size is smaller than the container.

| Property          | What it does                                  |
| ----------------- | --------------------------------------------- |
| `justify-content` | The grid → horizontally in the container      |
| `align-content`   | The grid → vertically in the container        |
| `place-content`   | Shorthand for both                            |

Values: `start`, `end`, `center`, `stretch`, `space-between`, `space-around`, `space-evenly`.

### 💎 The easiest centering trick in CSS

```css
.center-screen {
  min-height: 100vh;
  display: grid;
  place-items: center;
}
```

Perfect for login pages, loading screens, error pages, modals.

---

## 13. Grid vs Flexbox — When to Use Which

| Question                      | Flexbox                              | Grid                                   |
| ----------------------------- | ------------------------------------ | -------------------------------------- |
| Main direction                | 1-dimensional (row OR column)        | 2-dimensional (rows AND columns)       |
| Best for                      | Navbars, buttons, centering          | Page layouts, dashboards, galleries    |
| Control rows AND columns?     | Limited                              | Yes — that's the point                 |
| Sizing model                  | Items flex based on space            | Tracks + items placed into a grid      |

**Simple rule:** One line of stuff → Flexbox. Grid of stuff → Grid.

---

## 14. Common Mistakes

1. **Using Grid when Flexbox is simpler.** Navbars? Use Flexbox.
2. **Forgetting `gap`.** Don't add margin to items — use `gap`.
3. **Confusing `align-items` vs `align-content`.** Items align INSIDE cells. Content aligns the WHOLE grid.
4. **Using `auto-fill` when you meant `auto-fit`.** For card grids, use `auto-fit`.
5. **Overflow with `1fr`.** Use `minmax(0, 1fr)` when content might overflow.
6. **Hardcoding line numbers.** Prefer `1 / -1` over `1 / 4` so it survives layout changes.
7. **Reordering items visually.** Screen readers follow HTML order, not visual.

---

## 🧠 Cheat Sheet

| Pattern                   | Snippet                                                        |
| ------------------------- | -------------------------------------------------------------- |
| **Equal columns**         | `grid-template-columns: repeat(3, 1fr);`                       |
| **Sidebar layout**        | `grid-template-columns: 250px 1fr;`                            |
| **Responsive cards** ⭐   | `repeat(auto-fit, minmax(250px, 1fr))`                         |
| **Perfect centering**     | `display: grid; place-items: center;`                          |
| **Full-width header**     | `grid-column: 1 / -1;`                                         |
| **Span 2 columns**        | `grid-column: span 2;`                                         |
| **Span 2 rows**           | `grid-row: span 2;`                                            |
| **Fix overflow**          | `minmax(0, 1fr)`                                               |

---

## 🎓 Learning Order

Open the HTML files in this order:

1. `01-basics.html` — just `display: grid`
2. `02-columns-rows.html` — build the grid structure
3. `03-fr-unit.html` — flexible sizing
4. `04-repeat.html` — shortcuts
5. `05-gap.html` — spacing
6. `06-grid-lines.html` — placing items
7. `07-span.html` — easier placement
8. `08-template-areas.html` — visual layouts ⭐
9. `09-minmax.html` — flexible limits
10. `10-auto-fit-fill.html` — responsive magic ⭐
11. `11-implicit-grid.html` — auto-created tracks
12. `12-alignment.html` — items vs content
13. `13-real-world.html` — 🎁 everything together

Happy gridding! 🎉
