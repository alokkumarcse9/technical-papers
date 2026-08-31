# CSS Technical Paper — Simple English Notes

---

# 1. CSS Box Model

## Definition

The **CSS Box Model** explains how every HTML element is treated as a rectangular box.

Every element has four main parts:

```text
Margin
 └── Border
      └── Padding
           └── Content
```

## Parts

* **Content** → The actual text, image, or other content inside the element.
* **Padding** → Space inside the element, around the content.
* **Border** → A line around the content and padding.
* **Margin** → Space outside the element.

Example:

```css
box-sizing: border-box;
```

With `border-box`, padding and border are included inside the specified width and height.

### Easy Rule

> **Content = Inside**
> **Padding = Space inside**
> **Border = Line around the element**
> **Margin = Space outside**

---

# 2. Inline vs Block Elements

## Block Elements

### Definition

Block elements usually:

* Start on a new line.
* Take the available width.

Examples:

```html
<div>
<p>
<h1>
<section>
<main>
<header>
<footer>
```

### Easy Rule

> **Block = New line**

---

## Inline Elements

### Definition

Inline elements stay on the same line and usually take only the space they need.

Examples:

```html
<span>
<a>
<strong>
<em>
```

Example:

```html
<p>
  Hello <span>World</span>
</p>
```

Here, `Hello` and `World` can stay on the same line.

### Easy Rule

> **Inline = Same line**

---

# 3. Positioning: Relative and Absolute

## `position: relative`

### Definition

The element stays in the normal document flow, but it can be moved from its original position.

It can also become a reference for absolute child elements.

```css
.parent {
  position: relative;
}
```

### Easy Rule

> **Relative = Stays in normal flow and can be a reference for children**

---

## `position: absolute`

### Definition

The element is removed from the normal document flow.

It can be placed at an exact position relative to its nearest positioned parent.

```css
.child {
  position: absolute;
  top: 0;
  right: 0;
}
```

## Common Pattern

```text
Parent → position: relative
Child  → position: absolute
```

### Easy Rule

> **Parent = relative**
> **Child = absolute**

---

# 4. Common CSS Structural Classes

## Definition

Structural classes describe the **purpose or layout role** of an element.

| Class        | Simple Meaning                        |
| ------------ | ------------------------------------- |
| `.container` | Controls width and centers content    |
| `.wrapper`   | Groups elements together              |
| `.layout`    | Controls the overall layout           |
| `.header`    | Top section                           |
| `.main`      | Main page content                     |
| `.section`   | Major part of a page                  |
| `.content`   | Main content area                     |
| `.sidebar`   | Extra content beside the main content |
| `.footer`    | Bottom section                        |
| `.grid`      | Grid layout container                 |
| `.flex`      | Flexbox layout container              |
| `.row`       | Horizontal layout                     |
| `.column`    | Vertical layout                       |
| `.card`      | Reusable UI component                 |

## Naming Rule

Give class names based on their purpose.

Prefer:

```css
.sidebar
.product-card
.main-content
```

Avoid unclear names:

```css
.box1
.box2
.container1
```

### Golden Rule

> **Name a class based on what the element does or represents.**

---

# 5. Common CSS Styling Classes

## Definition

Styling classes describe **how an element looks or behaves visually**.

Examples:

```css
.text-center
.text-bold
.text-large
.hidden
.visible
.active
.disabled
.primary
.secondary
```

Example:

```html
<button class="button primary">
  Submit
</button>
```

## Difference

> **Structural classes = What the element is or does**

> **Styling classes = How the element looks**

---

# 6. CSS Specificity

## Definition

CSS specificity decides **which CSS rule will be applied** when multiple rules target the same element.

## Priority

```text
Inline styles
↓
ID selector
↓
Class / Attribute / Pseudo-class
↓
Element selector
```

Example:

```css
p {
  color: blue;
}

.text {
  color: red;
}

#title {
  color: green;
}
```

```html
<p id="title" class="text">
  Hello
</p>
```

Result:

```text
green
```

This happens because an **ID selector has higher specificity** than a class selector or element selector.

### Easy Rule

> **Higher specificity usually wins.**

---

# 7. CSS Responsive Queries

## Definition

Media queries allow CSS to change based on the screen size or other screen conditions.

Example:

```css
@media (max-width: 768px) {
  .container {
    grid-template-columns: 1fr;
  }
}
```

Meaning:

> When the screen width is `768px` or smaller, use one column.

## Common Breakpoints

```css
/* Tablet */
@media (max-width: 768px) {}

/* Mobile */
@media (max-width: 480px) {}
```

Responsive design helps a website work on:

```text
Desktop
Tablet
Mobile
```

### Easy Rule

> **Media queries = Change the layout for different screen sizes**

---

# 8. Flexbox

## Definition

Flexbox is mainly used to arrange items in **one direction**.

```css
.container {
  display: flex;
}
```

By default, items go in a row:

```text
A → B → C
```

For column direction:

```css
flex-direction: column;
```

Result:

```text
A
B
C
```

## Important Properties

```css
display: flex;
flex-direction;
justify-content;
align-items;
flex-wrap;
gap;
```

## Mental Model

```text
Main Axis
Cross Axis
```

Use Flexbox when you mainly think:

> Arrange items in a row or column.

### Easy Rule

> **Flexbox = One-dimensional layout**

---

# 9. CSS Grid

## Definition

CSS Grid is mainly used for layouts with **rows and columns**.

```css
.container {
  display: grid;
}
```

Example:

```css
grid-template-columns: 1fr 1fr;
grid-template-rows: 200px 200px;
```

## Important Properties

```css
display: grid;
grid-template-columns;
grid-template-rows;
gap;
grid-column;
grid-row;
```

## `fr`

### Definition

`fr` means **fraction of the available space**.

Example:

```css
grid-template-columns: 1fr 2fr;
```

Result:

```text
|----|--------|
  1fr    2fr
```

Use Grid when you think:

> Rows and columns.

### Easy Rule

> **Grid = Two-dimensional layout**

---

# 10. Flexbox vs Grid

| Flexbox             | Grid                  |
| ------------------- | --------------------- |
| One-dimensional     | Two-dimensional       |
| Row or column       | Rows and columns      |
| Good for components | Good for page layouts |
| Easy alignment      | More layout control   |

## Example

Use Flexbox:

```text
Logo | Navigation | Button
```

Use Grid:

```text
Header
----------------
Content | Sidebar
----------------
Footer
```

### Easy Rule

> **Flexbox = Row or column**

> **Grid = Rows and columns**

---

# 11. Common Header Meta Tags

## Character Encoding

```html
<meta charset="UTF-8">
```

### Definition

This helps the browser display common characters and languages correctly.

---

## Responsive Viewport

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### Definition

This helps the webpage work properly on mobile devices.

---

## Page Title

```html
<title>My Website</title>
```

### Definition

The page title is usually displayed in the browser tab.

---

## Description

```html
<meta
  name="description"
  content="Description of the webpage"
>
```

### Definition

This gives search engines and previews a short description of the webpage.

---

# 12. `display` Property

## Definition

The `display` property controls **how an element behaves in the page layout**.

Common values:

```css
display: block;
display: inline;
display: inline-block;
display: flex;
display: grid;
display: none;
```

## Summary

| Value          | Simple Meaning                             |
| -------------- | ------------------------------------------ |
| `block`        | Starts on a new line                       |
| `inline`       | Stays on the same line                     |
| `inline-block` | Stays inline but supports width and height |
| `flex`         | Uses Flexbox                               |
| `grid`         | Uses Grid                                  |
| `none`         | Hides the element                          |

### Easy Rule

> **display = How an element behaves in the layout**

---

# 13. Width and Height Units

## `px`

### Simple Definition

`px` is used for a fixed size.

```css
width: 200px;
```

---

## `%`

### Simple Definition

`%` is relative to the appropriate parent or containing area.

```css
width: 50%;
```

---

## `vw`

### Simple Definition

`vw` is relative to the viewport width.

```css
width: 50vw;
```

---

## `vh`

### Simple Definition

`vh` is relative to the viewport height.

```css
height: 100vh;
```

---

## `rem`

### Simple Definition

`rem` is relative to the root font size.

```css
font-size: 2rem;
```

---

## `fr`

### Simple Definition

`fr` is mainly used in CSS Grid to divide available space.

```css
grid-template-columns: 1fr 2fr;
```

---

# 14. Margin vs Padding

## Margin

### Simple Definition

Margin is the space **outside an element**.

```css
margin: 20px;
```

---

## Padding

### Simple Definition

Padding is the space **inside an element**.

```css
padding: 20px;
```

Remember:

```text
Margin → Outside
Padding → Inside
```

### Easy Rule

> **Margin = Outside space**

> **Padding = Inside space**

---

# 15. Normal Document Flow

## Simple Definition

By default, HTML elements follow the normal document flow.

Example:

```html
<div>A</div>
<div>B</div>
<div>C</div>
```

Usually:

```text
A
B
C
```

Some properties can change this behavior:

```css
position: absolute;
position: fixed;
float;
display: flex;
display: grid;
```

### Easy Rule

> **Normal document flow = Elements appear in their normal HTML order**

---

# 16. CSS `position` Values

The main position values are:

```css
position: static;
position: relative;
position: absolute;
position: fixed;
position: sticky;
```

---

## `static`

### Simple Definition

This is the default position.

```css
position: static;
```

### Easy Rule

> **Static = Normal position**

---

## `relative`

### Simple Definition

The element stays in the normal flow but can be moved from its original position.

```css
position: relative;
```

### Easy Rule

> **Relative = Move from the original position**

---

## `absolute`

### Simple Definition

The element is removed from the normal flow and placed relative to a positioned parent.

```css
position: absolute;
```

### Easy Rule

> **Absolute = Exact position**

---

## `fixed`

### Simple Definition

The element is positioned relative to the screen and stays there while scrolling.

```css
position: fixed;
```

### Easy Rule

> **Fixed = Stays in the same place on the screen**

---

## `sticky`

### Simple Definition

The element behaves normally at first and then sticks when a scroll position is reached.

```css
position: sticky;
top: 0;
```

### Easy Rule

> **Sticky = Normal first, then sticks while scrolling**

---

# 17. `justify-content` vs `align-items`

For Flexbox:

```css
display: flex;
```

## `justify-content`

### Simple Definition

`justify-content` aligns items along the **main axis**.

## `align-items`

### Simple Definition

`align-items` aligns items along the **cross axis**.

Example:

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

This centers the items.

### Easy Rule

> **justify-content = Main axis**

> **align-items = Cross axis**

---

# 18. `box-sizing`

Recommended:

```css
* {
  box-sizing: border-box;
}
```

## Simple Definition

`box-sizing: border-box` makes width and height calculations easier.

Example:

```css
width: 200px;
padding: 20px;
border: 5px solid black;
```

With:

```css
box-sizing: border-box;
```

The total width remains:

```text
200px
```

### Easy Rule

> **border-box = Padding and border stay inside the declared width**

---

# 19. `margin: 0 auto`

## Simple Definition

`margin: 0 auto` is commonly used to center a block or container horizontally.

```css
.container {
  max-width: 1200px;
  margin: 0 auto;
}
```

Meaning:

```text
top-bottom → 0
left-right → automatic
```

### Easy Rule

> **`margin: 0 auto` = Center the container horizontally**

---

# 20. Basic Professional CSS Reset

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```

## Definition

This removes default spacing and makes element sizing easier and more predictable.

### Easy Rule

> **CSS Reset = Remove browser default spacing**

---

# Golden Rule

> **HTML gives structure.**

> **CSS gives layout and styling.**

> **Flexbox and Grid arrange elements.**

> **Positioning places elements in specific locations.**

> **Media queries make the layout responsive.**
