# CSS Box Model

## Definition

The **CSS Box Model** explains how CSS treats every HTML element as a box.

Every box has four parts:

1. Content
2. Padding
3. Border
4. Margin

For example:

```html
<div class="box">Hello</div>
```

CSS sees it like this:

```text
┌───────────────────────────────┐
│            Margin             │
│   ┌───────────────────────┐   │
│   │        Border         │   │
│   │   ┌───────────────┐   │   │
│   │   │    Padding    │   │   │
│   │   │   ┌───────┐   │   │   │
│   │   │   │Content│   │   │   │
│   │   │   └───────┘   │   │   │
│   │   └───────────────┘   │   │
│   └───────────────────────┘   │
└───────────────────────────────┘
```

---

# 1. Content

Content is the actual thing inside the element.

```css
.box {
  width: 200px;
  height: 100px;
}
```

```text
┌────────────────────┐
│       Content      │
│                    │
└────────────────────┘
```

For example:

```html
<div>Hello World</div>
```

`Hello World` is the content.

---

# 2. Padding

Padding is the space between the **content and the border**.

```css
.box {
  padding: 20px;
}
```

```text
┌────────────────────────┐
│        Border          │
│   ┌────────────────┐   │
│   │    Padding     │   │
│   │  ┌──────────┐  │   │
│   │  │ Content  │  │   │
│   │  └──────────┘  │   │
│   └────────────────┘   │
└────────────────────────┘
```

### Example

```css
.box {
  padding: 20px;
}
```

This adds `20px` space on all sides:

* Top
* Right
* Bottom
* Left

You can also write:

```css
padding-top: 10px;
padding-right: 20px;
padding-bottom: 10px;
padding-left: 20px;
```

---

# 3. Border

Border is the line around the content and padding.

```css
.box {
  border: 2px solid black;
}
```

Example:

```text
┌──────────────────────┐ ← Border
│                      │
│      Content         │
│                      │
└──────────────────────┘ ← Border
```

---

# 4. Margin

Margin is the space outside the element.

```css
.box {
  margin: 20px;
}
```

```text
        Margin
   ←────────────→

      ┌───────────┐
      │   BOX     │
      └───────────┘

        Margin
```

## Important

> **Padding = space inside the box.**

> **Margin = space outside the box.**

---

# Complete Example

```css
.box {
  width: 200px;
  height: 100px;

  padding: 20px;

  border: 5px solid black;

  margin: 30px;

  background-color: grey;
}
```

The structure is:

```text
Margin: 30px
│
├── Border: 5px
│   │
│   ├── Padding: 20px
│   │   │
│   │   └── Content
│   │
│   └── Border
│
└── Margin
```

---

# How to Calculate the Total Size

By default, CSS uses:

```css
box-sizing: content-box;
```

Suppose:

```css
.box {
  width: 200px;

  padding: 20px;

  border: 5px solid black;

  margin: 30px;
}
```

The actual visible box width is:

```text
Content = 200px

Left padding = 20px
Right padding = 20px

Left border = 5px
Right border = 5px
```

So:

```text
200 + 20 + 20 + 5 + 5
= 250px
```

The margin adds space outside:

```text
250 + 30 + 30
= 310px total occupied horizontal space
```

---

# `box-sizing: border-box`

Developers often use:

```css
* {
  box-sizing: border-box;
}
```

Now:

```css
.box {
  width: 200px;
  padding: 20px;
  border: 5px solid black;
}
```

The total visible width remains:

```text
200px
```

This is because padding and border are included inside the `200px`.

## Comparison

| `content-box`                                     | `border-box`                              |
| ------------------------------------------------- | ----------------------------------------- |
| Width is only for content                         | Width includes content + padding + border |
| Element can become bigger than the declared width | Element stays within the declared width   |
| Default CSS behavior                              | Commonly used by developers               |

---

# Easy Real-Life Example

Imagine a **house**:

```text
Margin
   ↓
Space between houses

Border
   ↓
Wall of the house

Padding
   ↓
Space inside the house

Content
   ↓
Furniture / people inside
```

So remember:

```text
OUTSIDE
   ↓
Margin
   ↓
Border
   ↓
Padding
   ↓
Content
```