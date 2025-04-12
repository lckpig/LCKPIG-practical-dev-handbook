<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/advanced/css-variables)
<!-- MULTILANGUAJE MENU END -->

# CSS Variables (Custom Properties)

CSS variables, also known as custom properties, allow you to store specific values to reuse throughout a document, adding dynamism and simplifying stylesheet maintenance.

## Basic Syntax

```css
/* Variable declaration (usually in the :root element) */
:root {
  --primary-color: #3498db;
  --secondary-color: #2ecc71;
  --base-spacing: 1rem;
  --main-font: 'Roboto', sans-serif;
}

/* Using variables */
.button {
  background-color: var(--primary-color);
  padding: var(--base-spacing);
  font-family: var(--main-font);
}
```

## Key Features

### Scope

CSS variables respect the DOM scope where they are defined:

```css
:root {
  --color: blue; /* Global variable */
}

.component {
  --color: red; /* Local variable for .component and its descendants */
  color: var(--color); /* Red */
}

.another-element {
  color: var(--color); /* Blue, uses the global variable */
}
```

### Fallback Values

```css
.element {
  /* If --width is not defined, 100px will be used */
  width: var(--width, 100px);
}
```

### Nested Variables

```css
:root {
  --base-hue: 200;
  --primary-color: hsl(var(--base-hue), 80%, 50%);
}
```

## Manipulation with JavaScript

CSS variables can be read and modified with JavaScript, allowing dynamic styles:

```javascript
// Reading a CSS variable
const primaryColor = getComputedStyle(document.documentElement)
  .getPropertyValue('--primary-color');

// Modifying a CSS variable
document.documentElement.style.setProperty('--primary-color', '#ff0000');
```

## Common Use Cases

- **Color themes**: Facilitating theme changes (light/dark mode)
- **Design systems**: Maintaining consistency across components
- **Responsive design**: Changing values based on screen size
- **Animations**: Animating properties by changing variables
- **Component states**: Modifying variables based on UI states

## Advantages Over Preprocessors

- **Dynamism**: CSS variables are reactive, not just at compilation time
- **Interactivity**: Can be modified with JavaScript
- **DOM Scope**: Respect cascade and element scope
- **Native standard**: No compilation required

## Limitations

- **Browser support**: Not available in IE11 and very old browsers
- **Limited functionality**: Don't allow complex operations without `calc()` functions
- **No native conditionals**: Don't have built-in control structures

CSS variables are a powerful tool for creating more dynamic, maintainable, and systematic styles in modern web applications. 