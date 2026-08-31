# Common CSS Structural Classes — Short Notes

## Definition

**CSS structural classes** are class names used to organize and structure a webpage.

They help us understand the purpose of each part of the page.

---

## 1. `.container`

### Simple Definition

`.container` is used to control the **width of content** and keep it **centered on the page**.

```css
.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
}
```

### Easy Rule

> **Container = Controls width and centers content**

---

## 2. `.wrapper`

### Simple Definition

`.wrapper` is used to **group elements together**.

It wraps other elements inside it.

```text
wrapper
└── content
```

### Easy Rule

> **Wrapper = Groups elements together**

---

## 3. `.layout`

### Simple Definition

`.layout` is used to control the **overall arrangement of elements**.

It usually uses Flexbox or Grid.

```css
.layout {
  display: grid;
}
```

### Easy Rule

> **Layout = Arranges the main parts of a page**

---

## 4. `.header`

### Simple Definition

`.header` represents the **top part of a webpage**.

It usually contains:

* Logo
* Navigation
* Search

### Easy Rule

> **Header = Top section of the webpage**

---

## 5. `.main`

### Simple Definition

`.main` contains the **main and important content** of the webpage.

```html
<main class="main">
  Main content
</main>
```

### Easy Rule

> **Main = Main content of the webpage**

---

## 6. `.section`

### Simple Definition

`.section` represents a **large part of a webpage**.

Examples:

* Hero section
* About section
* Features section
* Contact section

### Easy Rule

> **Section = One major part of a webpage**

---

## 7. `.content`

### Simple Definition

`.content` represents the **main content area**.

```text
Main
├── Content
└── Sidebar
```

### Easy Rule

> **Content = Main information area**

---

## 8. `.sidebar`

### Simple Definition

`.sidebar` contains **extra or secondary content** beside the main content.

Examples:

* Related posts
* Filters
* Advertisements

### Easy Rule

> **Sidebar = Extra content beside the main content**

---

## 9. `.footer`

### Simple Definition

`.footer` represents the **bottom part of a webpage**.

It usually contains:

* Copyright
* Links
* Contact information

### Easy Rule

> **Footer = Bottom section of the webpage**

---

## 10. `.grid`

### Simple Definition

`.grid` is a reusable class used to create a **CSS Grid container**.

```css
.grid {
  display: grid;
}
```

### Easy Rule

> **Grid = Arrange elements in rows and columns**

---

## 11. `.flex`

### Simple Definition

`.flex` is a reusable class used to create a **Flexbox container**.

```css
.flex {
  display: flex;
}
```

### Easy Rule

> **Flex = Arrange child elements easily**

---

## 12. `.row`

### Simple Definition

`.row` arranges elements **horizontally**, from left to right.

```text
A | B | C
```

```css
.row {
  display: flex;
  flex-direction: row;
}
```

### Easy Rule

> **Row = Horizontal direction**

---

## 13. `.column`

### Simple Definition

`.column` arranges elements **vertically**, from top to bottom.

```text
A
B
C
```

```css
.column {
  display: flex;
  flex-direction: column;
}
```

### Easy Rule

> **Column = Vertical direction**

---

## 14. `.card`

### Simple Definition

`.card` is a reusable **UI box or component** used to show related information.

Examples:

* Product card
* Profile card
* Article card

### Easy Rule

> **Card = A reusable box for related content**

---

## 15. `.inner`

### Simple Definition

`.inner` represents the **content inside another component or section**.

Example:

```html
<section class="hero">
  <div class="hero__inner">
    Content
  </div>
</section>
```

### Easy Rule

> **Inner = Content inside another element**

---

# Quick Summary

```text
.page
│
├── .header
│
├── .main
│   ├── .container
│   │   ├── .section
│   │   ├── .content
│   │   └── .sidebar
│
└── .footer
```

---

# Naming Rule

Avoid unclear class names like:

```css
.box1
.box2
.container1
.container2
```

Prefer meaningful names like:

```css
.header
.sidebar
.product-card
.profile-card
.main-content
```

## Simple Rule

> **Give a class a name based on the purpose of the element.**

---

# Golden Definition

> **CSS structural classes are meaningful class names used to organize, structure, and arrange different parts of a webpage.**
