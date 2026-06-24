# 🌐 HTML & CSS Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · HTML5 & CSS3 quick reference.

---

## HTML Document Skeleton

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <!-- content -->
  <script src="app.js"></script>
</body>
</html>
```

## Common HTML Elements

```html
<h1>–<h6>            <!-- headings -->
<p>paragraph</p>
<a href="url">link</a>
<img src="img.png" alt="desc">
<ul><li>item</li></ul>      <!-- unordered list -->
<ol><li>item</li></ol>      <!-- ordered list -->
<br> <hr>                    <!-- line break, rule -->
<strong>bold</strong> <em>italic</em>
<span>inline</span> <div>block</div>
<button>Click</button>
<input type="text" placeholder="name">
```

## Semantic HTML5

```html
<header>...</header>
<nav>...</nav>
<main>
  <section>...</section>
  <article>...</article>
  <aside>...</aside>
</main>
<footer>...</footer>
<figure><img><figcaption>caption</figcaption></figure>
```

## Forms

```html
<form action="/submit" method="post">
  <label for="email">Email</label>
  <input type="email" id="email" name="email" required>
  <input type="password" name="pwd">
  <textarea name="msg"></textarea>
  <select name="role">
    <option value="user">User</option>
  </select>
  <input type="checkbox" name="agree">
  <input type="radio" name="plan" value="free">
  <button type="submit">Send</button>
</form>
<!-- input types: text, email, number, date, file, color, range, tel, url -->
```

## CSS Selectors

```css
*              { }   /* all */
p              { }   /* element */
.class         { }   /* class */
#id            { }   /* id */
div p          { }   /* descendant */
div > p        { }   /* direct child */
a:hover        { }   /* pseudo-class */
li::before     { }   /* pseudo-element */
input[type=text] { } /* attribute */
.a, .b         { }   /* group */
```

## Box Model

```css
.box {
  width: 200px;
  padding: 10px;          /* inside */
  border: 2px solid #333;
  margin: 20px;           /* outside */
  box-sizing: border-box; /* include padding+border in width */
}
```

## Flexbox (1D layout)

```css
.container {
  display: flex;
  flex-direction: row;          /* row | column */
  justify-content: space-between; /* main axis */
  align-items: center;           /* cross axis */
  gap: 16px;
  flex-wrap: wrap;
}
.item {
  flex: 1;                /* grow to fill */
  flex: 0 0 200px;        /* fixed basis, no grow/shrink */
}
```

## Grid (2D layout)

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);   /* 3 equal cols */
  grid-template-columns: 200px 1fr;        /* fixed + flexible */
  gap: 20px;
}
.span2 { grid-column: span 2; }
```

## Colors, Text & Units

```css
.text {
  color: #333;                  /* hex */
  color: rgb(51, 51, 51);
  color: rgba(0, 0, 0, 0.5);    /* with alpha */
  background: hsl(200, 50%, 50%);
  font-size: 1.2rem;            /* rem, em, px, %, vw, vh */
  font-weight: 600;
  text-align: center;
  line-height: 1.5;
  font-family: 'Inter', sans-serif;
}
```

## Position & Display

```css
position: static | relative | absolute | fixed | sticky;
top: 0; left: 0; z-index: 10;
display: block | inline | inline-block | flex | grid | none;
overflow: hidden | scroll | auto;
```

## Responsive (Media Queries)

```css
/* mobile-first */
.box { width: 100%; }

@media (min-width: 768px) {
  .box { width: 50%; }
}
@media (max-width: 480px) {
  .nav { display: none; }
}
```

## Transitions & Animations

```css
.btn {
  transition: all 0.3s ease;
}
.btn:hover {
  transform: scale(1.05) translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.2);
}

@keyframes fadeIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}
.fade { animation: fadeIn 0.5s ease forwards; }
```

## Variables (Custom Properties)

```css
:root {
  --primary: #7a1220;
  --gap: 16px;
}
.btn {
  background: var(--primary);
  padding: var(--gap);
}
```

---

[🔝 Back to README](../README.md)
