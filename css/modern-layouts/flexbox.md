<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/modern-layouts/flexbox)
<!-- MULTILANGUAJE MENU END -->

# Flexbox in CSS

Flexbox (Flexible Box Layout) is a one-dimensional layout model that allows you to distribute space between elements in an interface and improve alignment capabilities.

## Basic Concepts

- **Flex container**: The parent element with `display: flex` or `display: inline-flex`
- **Flex items**: The direct children of the flex container
- **Main axis**: Principal direction (horizontal or vertical)
- **Cross axis**: Perpendicular to the main axis

## Container Properties

```css
.container {
  display: flex;
  flex-direction: row | row-reverse | column | column-reverse;
  flex-wrap: nowrap | wrap | wrap-reverse;
  justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
  align-items: flex-start | flex-end | center | stretch | baseline;
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
  gap: 10px; /* Space between items */
}
```

## Item Properties

```css
.item {
  order: 0; /* Order of appearance */
  flex-grow: 0; /* Growth factor */
  flex-shrink: 1; /* Shrink factor */
  flex-basis: auto; /* Initial size */
  flex: 0 1 auto; /* Shorthand: grow shrink basis */
  align-self: auto | flex-start | flex-end | center | stretch | baseline;
}
```

## Common Use Cases

- Vertical and horizontal centering
- Responsive navigation
- Card layouts
- Balanced distribution of elements
- Reordering elements at different screen sizes

Flexbox is ideal for one-dimensional components (rows or columns) and offers elegant solutions for many design problems that previously required complicated hacks. 