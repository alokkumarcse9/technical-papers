# CSS Positioning

## Definition

The CSS `position` property tells the browser **where and how an element should be placed on the webpage**.

The main values are:

```css
position: static;
position: relative;
position: absolute;
position: fixed;
position: sticky;
```

Let's understand them one by one.

---

# 1. `position: static`

## Definition

`static` is the **default position** of HTML elements.

```css
.box {
  position: static;
}
```

Elements follow the normal HTML flow.

Example:

```html
<div>Box 1</div>
<div>Box 2</div>
<div>Box 3</div>
```

Result:

```text
Box 1
Box 2
Box 3
```

Normally:

```css
top
right
bottom
left
```

do not work to move a `static` element.

### Easy Rule

> **Static = Normal position**

---

# 2. `position: relative` 

## Definition

`relative` means:

> **The element stays in its normal place, but you can move it from its original position.**

Example:

```css
.box {
  position: relative;

  top: 20px;
  left: 30px;
}
```

This means:

```text
Original position
      ↓ 20px
         → 30px
```

The important thing is:

> **The original space of the element still remains reserved.**

### Easy Rule

> **Relative = Move an element from its original position**

---

## Another Important Use of `relative`

`relative` is also commonly used with `absolute`.

```css
.container {
  position: relative;
}

.box {
  position: absolute;
}
```

Here, `.container` becomes the **reference area** for `.box`.

Think:

```text
Container
position: relative
        ↓
Creates a reference area
        ↓
Box
position: absolute
```

### Easy Rule

> **Parent: relative → Child: absolute**

---

# 3. `position: absolute`

## Simple Definition

`absolute` means:

> **The element is removed from the normal flow and placed at an exact position.**

Example:

```css
.box {
  position: absolute;

  top: 0;
  right: 0;
}
```

The box moves to the top-right of its **positioning reference**.

Usually:

```css
.parent {
  position: relative;
}

.child {
  position: absolute;
}
```

Example:

```html
<div class="parent">

  <div class="child">
    Box
  </div>

</div>
```

```css
.parent {
  position: relative;

  width: 400px;
  height: 400px;

  background-color: grey;
}

.child {
  position: absolute;

  top: 0;
  right: 0;

  width: 150px;
  height: 150px;

  background-color: black;
}
```

Result:

```text
Parent
┌──────────────────────┐
│               ┌─────┐│
│               │ Box ││
│               └─────┘│
│                      │
└──────────────────────┘
```

### Important

The `absolute` element does not keep its original space in the normal layout.

### Easy Rule

> **Absolute = Exact position inside a reference area**

---

# 4. `position: fixed`

## Simple Definition

`fixed` means:

> **The element is fixed relative to the screen.**

Example:

```css
.box {
  position: fixed;

  bottom: 0;
  right: 0;
}
```

Result:

```text
SCREEN
┌──────────────────────────┐
│                          │
│                          │
│                    ┌────┐│
│                    │BOX ││
└────────────────────┴────┘
```

Even if you scroll the page, the box stays in the same place on the screen.

Common examples:

* Chat button 
* Back-to-top button
* Fixed navigation bar

### Easy Rule

> **Fixed = Stays in the same place on the screen**

---

# 5. `position: sticky`

## Simple Definition

`sticky` is like a combination of:

```text
relative + fixed
```

Example:

```css
.header {
  position: sticky;
  top: 0;
}
```

At first, the element behaves normally.

When you scroll to its position:

> **It sticks to the top.**

Common example:

```text
Scroll ↓

HEADER
────────────
Content
Content
Content
```

After scrolling:

```text
HEADER ← stays at top
────────────
Content
Content
Content
```

### Easy Rule

> **Sticky = Normal at first, then sticks while scrolling**

---

# Positioning Properties

These properties are used with positioning:

```css
top
right
bottom
left
```

They help you decide where an element should be placed.

---

## `top`

```css
top: 20px;
```

The element is placed `20px` from the top.

---

## `right`

```css
right: 20px;
```

The element is placed `20px` from the right.

---

## `bottom`

```css
bottom: 20px;
```

The element is placed `20px` from the bottom.

---

## `left`

```css
left: 20px;
```

The element is placed `20px` from the left.

---

# Four Corners

## Top-left

```css
position: absolute;
top: 0;
left: 0;
```

## Top-right

```css
position: absolute;
top: 0;
right: 0;
```

## Bottom-left

```css
position: absolute;
bottom: 0;
left: 0;
```

## Bottom-right

```css
position: absolute;
bottom: 0;
right: 0;
```

---

# Most Important Concept

## Which Position Should I Use?

### Normal layout

```text
Box 1
Box 2
Box 3
```

Use:

```css
position: static;
```

Or simply don't specify `position`.

### Easy Rule

> **Normal layout = static**

---

### Move something slightly from its original place

Use:

```css
position: relative;
```

### Easy Rule

> **Move from original place = relative**

---

### Place something at an exact position inside a container

Use:

```css
.parent {
  position: relative;
}

.child {
  position: absolute;
}
```

### Easy Rule

> **Exact position inside parent = relative + absolute**

---

### Place something relative to the screen

Use:

```css
position: fixed;
```

### Easy Rule

> **Stay in one place on screen = fixed**

---

### Make something stick while scrolling

Use:

```css
position: sticky;
```

### Easy Rule

> **Stick while scrolling = sticky**

---

# Quick Comparison

| Position   | Reference                   | Normal Flow?  | Common Use                        |
| ---------- | --------------------------- | ------------- | --------------------------------- |
| `static`   | Normal document flow        | Yes           | Default position                  |
| `relative` | Its original position       | Yes           | Small movement / parent reference |
| `absolute` | Positioned parent           | No            | Exact position                    |
| `fixed`    | Viewport / screen           | No            | Fixed buttons / navbar            |
| `sticky`   | Scroll position / container | Initially Yes | Sticky headers                    |

---

# Best Mental Model 

When you see a layout, ask yourself:

### Question 1:

> **Do I need normal arrangement of elements?**

Use:

```css
display: flex;
```

or:

```css
display: grid;
```

---

### Question 2:

> **Do I need an element at an exact location?**

Use:

```css
position: absolute;
```

---

### Question 3:

> **Exact location relative to what?**

## Inside a container

```css
.parent {
  position: relative;
}

.child {
  position: absolute;
}
```

## Relative to the screen

```css
.box {
  position: fixed;
}
```

---

# Golden Rule 

> **Flexbox and Grid are used to arrange elements.**

> **Positioning is used to place elements in specific locations.**
