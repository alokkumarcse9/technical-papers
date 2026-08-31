# What is `display` in CSS?

## Simple Definition

`display` tells the browser **how an element should appear and behave on the page**.

It also helps control **how child elements are arranged**.

For example:

```css
.container {
  display: flex;
}
```

This means:

> Make `.container` a Flexbox container and arrange its children using Flexbox.

---

# Common `display` Values

The important values are:

```css
display: block;
display: inline;
display: inline-block;
display: flex;
display: grid;
display: none;
```

---

# 1. `display: block`

## Simple Definition

A block element starts on a **new line** and usually takes the **full available width**.

Example:

```html
<div>Box 1</div>
<div>Box 2</div>
<div>Box 3</div>
```

Result:

```text
┌─────────────────────┐
│ Box 1               │
└─────────────────────┘

┌─────────────────────┐
│ Box 2               │
└─────────────────────┘

┌─────────────────────┐
│ Box 3               │
└─────────────────────┘
```

`div` is normally a block element.

You can explicitly write:

```css
.box {
  display: block;
}
```

### Easy Rule

> **Block = New line + usually full width**

---

# 2. `display: inline`

## Simple Definition

Inline elements stay **on the same line**.

Example:

```html
<span>One</span>
<span>Two</span>
<span>Three</span>
```

Result:

```text
One Two Three
```

Example:

```css
span {
  display: inline;
}
```

⚠️ With inline elements, `width` and `height` generally do not work like normal block elements.

### Easy Rule

> **Inline = Elements stay on the same line**

---

# 3. `display: inline-block`

## Simple Definition

`inline-block` allows elements to stay **side by side** like inline elements.

But you can also set their `width` and `height` like block elements.

Example:

```css
.box {
  display: inline-block;

  width: 100px;
  height: 100px;
}
```

Boxes can stay side by side:

```text
┌───────┐ ┌───────┐ ┌───────┐
│ Box 1 │ │ Box 2 │ │ Box 3 │
└───────┘ └───────┘ └───────┘
```

And you can use:

```css
width
height
margin
padding
```

### Easy Rule

> **Inline-block = Side by side + can use width and height**

---

# 4. `display: flex`

## Simple Definition

`display: flex` is used to arrange **child elements easily**.

```css
.container {
  display: flex;
}
```

The `.container` becomes a **Flexbox container**.

Its children become **flex items**.

Example:

```html
<div class="container">
  <div>Box 1</div>
  <div>Box 2</div>
  <div>Box 3</div>
</div>
```

```css
.container {
  display: flex;
}
```

Result:

```text
┌──────────────────────────────┐
│ [Box 1] [Box 2] [Box 3]      │
└──────────────────────────────┘
```

By default:

```css
flex-direction: row;
```

So children go **horizontally**.

### Easy Rule

> **Flex = Easy way to arrange children**

---

## With `flex-direction: column`

```css
.container {
  display: flex;
  flex-direction: column;
}
```

Result:

```text
┌──────────────┐
│ Box 1        │
│ Box 2        │
│ Box 3        │
└──────────────┘
```

Now the children go **vertically**.

### Easy Rule

> **row = horizontal**

> **column = vertical**

---

## `display: flex` Gives You Control Over Children

```css
.container {
  display: flex;

  justify-content: center;
  align-items: center;
}
```

This controls how the **children are arranged**.

Important mindset:

```text
Parent
display: flex
     ↓
Controls
     ↓
Children
```

### Easy Rule

> **The parent uses Flexbox to control its children.**

---

# 5. `display: grid`

## Simple Definition

`display: grid` is used to arrange elements in **rows and columns**.

```css
.container {
  display: grid;

  grid-template-columns: 1fr 1fr 1fr;
}
```

Result:

```text
┌──────┬──────┬──────┐
│ Box1 │ Box2 │ Box3 │
├──────┼──────┼──────┤
│ Box4 │ Box5 │ Box6 │
└──────┴──────┴──────┘
```

Use Grid when you want to control:

* Rows
* Columns

### Easy Rule

> **Grid = Rows + Columns**

---

# 6. `display: none`

## Simple Definition

`display: none` completely hides an element.

```css
.box {
  display: none;
}
```

The element disappears from the page.

```text
HTML:

Box 1
Box 2
Box 3
```

If:

```css
.box2 {
  display: none;
}
```

Result:

```text
Box 1
Box 3
```

`Box 2` disappears and does not take any space.

### Easy Rule

> **display: none = Hide the element completely**

---

# Most Important Difference

## `display` vs `position`

These are different concepts.

---

# `display`

## Simple Definition

`display` controls **how an element behaves in the layout**.

It can also control how its children are arranged.

Examples:

```css
display: flex;
display: grid;
display: block;
```

### Easy Rule

> **display = How elements are arranged**

---

# `position`

## Simple Definition

`position` controls **where an element is placed on the page**.

Examples:

```css
position: relative;
position: absolute;
position: fixed;
```

### Easy Rule

> **position = Where an element is placed**

---

# Simple Example

```html
<div class="container">
  <div class="box">Box</div>
</div>
```

You can write:

```css
.container {
  display: flex;
}
```

This controls how `.box` is arranged inside `.container`.

Or:

```css
.container {
  position: relative;
}

.box {
  position: absolute;
  top: 0;
  right: 0;
}
```

This controls the exact position of `.box`.

---

# Easy Rule to Remember 

| If you want to...                               | Use                     |
| ----------------------------------------------- | ----------------------- |
| Arrange children                                | `display: flex`         |
| Create rows and columns                         | `display: grid`         |
| Normal new-line layout                          | `display: block`        |
| Keep elements on the same line                  | `display: inline`       |
| Keep elements side by side and use width/height | `display: inline-block` |
| Hide an element                                 | `display: none`         |
| Put an element at a specific location           | `position`              |

---

# Golden Definition 

> **`display` tells the browser how an HTML element should behave and appear in the page layout. It can also control how its child elements are arranged.**
