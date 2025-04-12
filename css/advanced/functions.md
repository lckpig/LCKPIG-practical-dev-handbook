<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/advanced/functions)
<!-- MULTILANGUAJE MENU END -->

# Functions in CSS

CSS functions allow calculations, transformations, and dynamic operations directly in stylesheets, making CSS more powerful and flexible.

## The calc() Function

The `calc()` function enables dynamic calculations, mixing different units.

```css
.element {
  /* Subtract a fixed margin from a percentage width */
  width: calc(100% - 40px);
  
  /* Combine different units */
  margin-top: calc(2rem + 5vh);
  
  /* More complex operations */
  height: calc((100vh - 10rem) / 2);
}
```

Supported operators: `+`, `-`, `*`, `/`

## Color Functions

### rgb() and rgba()

```css
.element {
  color: rgb(255, 0, 0);            /* Red */
  background-color: rgba(0, 0, 255, 0.5); /* Semi-transparent blue */
}
```

### hsl() and hsla()

```css
.element {
  /* Hue (0-360), Saturation (%), Lightness (%) */
  color: hsl(120, 100%, 50%);            /* Green */
  background-color: hsla(240, 100%, 50%, 0.5); /* Semi-transparent blue */
}
```

### New Notations

```css
/* Modern notation for rgb/rgba */
color: rgb(255 0 0 / 80%);

/* Color functions with alpha channel values */
color: hsl(0deg 100% 50% / 80%);
```

## Selection Functions

### min(), max(), and clamp()

```css
.element {
  /* Uses the smallest value */
  width: min(1000px, 100%);
  
  /* Uses the largest value */
  font-size: max(1rem, 2vw);
  
  /* Constrains a value between a minimum and maximum */
  padding: clamp(1rem, 5vw, 3rem);
}
```

## Gradient Functions

```css
.button {
  /* Linear gradient */
  background: linear-gradient(to right, red, blue);
  
  /* Radial gradient */
  background: radial-gradient(circle, yellow, orange);
  
  /* Conic gradient */
  background: conic-gradient(from 45deg, red, blue, green, yellow);
  
  /* Repeating gradient */
  background: repeating-linear-gradient(45deg, 
                red, red 10px, 
                blue 10px, blue 20px);
}
```

## Filter Functions

```css
.image {
  filter: brightness(150%) contrast(1.2);
  backdrop-filter: blur(5px);
}
```

## The var() Function

```css
.element {
  color: var(--primary-color, blue);
}
```

## Shape Functions

```css
.element {
  clip-path: circle(50%);
  shape-outside: circle(50%);
}
```

## The attr() Function

Accesses HTML attributes:

```css
[data-tooltip]:hover::after {
  content: attr(data-tooltip);
}
```

## Timing Functions

```css
.element {
  transition-timing-function: cubic-bezier(0.42, 0, 0.58, 1);
}
```

CSS functions add programmatic capabilities to stylesheets, allowing the creation of more dynamic, adaptable, and expressive designs without relying on JavaScript or preprocessors. 