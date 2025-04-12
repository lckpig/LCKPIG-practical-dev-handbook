<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/responsive-design/media-queries)
<!-- MULTILANGUAJE MENU END -->

# Media Queries in CSS

Media Queries are a key CSS feature that allows applying styles conditionally based on the characteristics of the device or window where the content is displayed.

## Basic Syntax

```css
@media media-type and (feature: value) {
  /* CSS rules that will be applied when the condition is met */
}
```

## Media Types

- **all**: All devices (default)
- **screen**: Computer screens, tablets, smartphones
- **print**: Printers or print preview
- **speech**: Speech synthesizers

## Common Features

- **width**, **min-width**, **max-width**: Window width
- **height**, **min-height**, **max-height**: Window height
- **orientation**: `portrait` or `landscape`
- **aspect-ratio**: Aspect ratio
- **resolution**: Resolution in dpi (dots per inch)
- **hover**: If the device supports hover
- **prefers-color-scheme**: Dark/light theme preference

## Practical Example

```css
/* Base styles for all devices */
body {
  font-size: 16px;
}

/* Styles for tablets (768px to 1023px) */
@media screen and (min-width: 768px) and (max-width: 1023px) {
  body {
    font-size: 18px;
  }
}

/* Styles for desktop (1024px and up) */
@media screen and (min-width: 1024px) {
  body {
    font-size: 20px;
  }
}

/* Styles for printing */
@media print {
  body {
    font-size: 12pt;
    color: black;
  }
}
```

## Logical Operators

- **and**: Combines conditions (all must be met)
- **,** (comma): Equivalent to OR (any can be met)
- **not**: Negates a media query
- **only**: Prevents older browsers from applying the styles

Media queries are fundamental for implementing responsive design and ensuring that websites look correct on any device. 