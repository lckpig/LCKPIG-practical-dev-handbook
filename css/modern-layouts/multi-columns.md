<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/modern-layouts/multi-columns)
<!-- MULTILANGUAJE MENU END -->

# Multi-columns in CSS

The CSS Multi-column module allows you to create multiple column layouts similar to newspapers or magazines, where content automatically flows from one column to another.

## Main Properties

```css
.container {
  /* Set number of columns */
  column-count: 3;
  
  /* Or set column width and let the browser calculate the number */
  column-width: 200px;
  
  /* Shorthand for both properties */
  columns: 3 200px;
  
  /* Space between columns */
  column-gap: 2em;
  
  /* Dividing line between columns */
  column-rule: 1px solid #ddd;
  
  /* Width stabilization */
  column-fill: auto | balance;
}
```

## Control of Elements Within Columns

```css
.title {
  /* Make an element span across all columns */
  column-span: all;
}

.no-break {
  /* Prevent an element from breaking between columns */
  break-inside: avoid;
}
```

## Common Use Cases

- Long texts to improve readability
- Lists of items (such as product cards)
- Forms with multiple fields
- Image galleries

## Considerations

- Content flows from top to bottom and then to the next column
- Works best with homogeneous content
- Can be difficult to control the exact balance between columns
- Combines well with media queries for responsive designs

Multi-columns are ideal for improving the readability of long texts on wide screens and creating magazine or newspaper-style layouts. 