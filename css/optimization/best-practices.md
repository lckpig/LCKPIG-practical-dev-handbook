<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/optimization/best-practices)
<!-- MULTILANGUAJE MENU END -->

# CSS Best Practices

Following CSS best practices is essential for maintaining efficient, maintainable, and performant stylesheets. These recommendations help avoid common problems and optimize CSS code.

## Organization and Structure

### Logical File Separation

```
styles/
  ├── base/
  │   ├── reset.css
  │   ├── typography.css
  │   └── colors.css
  ├── components/
  │   ├── buttons.css
  │   ├── forms.css
  │   └── cards.css
  ├── layout/
  │   ├── header.css
  │   ├── footer.css
  │   └── grid.css
  └── main.css
```

### Descriptive Comments

```css
/* ==========================================================================
   Section: Main Header
   ========================================================================== */

/* Main Navigation */
.main-nav {
  /* ... */
}
```

## Efficient Selectors

### Minimize Specificity

```css
/* Avoid */
body .wrapper .content article.post h2 { ... }

/* Prefer */
.post-title { ... }
```

### Avoid Deep Nested Selectors

```css
/* Avoid */
.header .nav .dropdown .item .link { ... }

/* Prefer */
.nav-link { ... }
```

### Use Classes Instead of ID Selectors

ID selectors have high specificity and are difficult to override.

```css
/* Avoid */
#header { ... }

/* Prefer */
.header { ... }
```

## Properties and Values

### Shorthand When Appropriate

```css
/* Verbose */
margin-top: 10px;
margin-right: 20px;
margin-bottom: 10px;
margin-left: 20px;

/* Concise */
margin: 10px 20px;
```

### Avoid !important

```css
/* Avoid */
.element {
  color: red !important;
}

/* Prefer better selectors or higher specificity when necessary */
```

### Consistent Units

Maintain consistency in the units of measurement used:

- `rem` for typography and general spacing
- `em` for text-related spacing
- `%` for relative widths
- `px` only for borders and very small sizes

## Performance

### Avoid Costly Universal Selectors

```css
/* Avoid as main selector */
* { ... }
[class^="col-"] { ... }

/* Limit the scope */
.container * { ... }
```

### Limit Use of Costly Properties

```css
/* Properties that can affect performance */
box-shadow
text-shadow
opacity
transform
filter
position: fixed
```

### Simplify Selectors

```css
/* Avoid */
button:not([disabled]):not(.btn--secondary) { ... }

/* Prefer */
.btn--primary { ... }
```

## Maintainability

### Follow a Naming Convention

Examples: BEM, SMACSS, OOCSS

```css
/* BEM example */
.block__element--modifier { ... }
```

### Maintain Consistency

- Keep a consistent order for properties
- Use the same format (indentation, spaces)
- Apply the same conventions throughout the project

### Avoid Highly Specific CSS for Components

To facilitate component reuse in different contexts.

## Compatibility

### Use Prefixes Only When Necessary

```css
/* Use automated tools like Autoprefixer */
display: flex;
```

### Test in Multiple Browsers

Verify CSS in major browsers and devices.

Following these best practices will help create cleaner, more efficient, and maintainable CSS code, facilitating teamwork and improving the overall performance of websites. 