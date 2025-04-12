<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/modern-layouts/css-grid)
<!-- MULTILANGUAJE MENU END -->

# CSS Grid

CSS Grid Layout is a two-dimensional layout system that allows you to create complex layouts of rows and columns simultaneously. It is especially powerful for full-page layouts and complex components.

## Basic Concepts

- **Grid container**: Element with `display: grid` or `display: inline-grid`
- **Grid items**: Direct children of the grid container
- **Grid lines**: Horizontal and vertical lines that divide the grid
- **Tracks**: Rows and columns of the grid
- **Cells**: Intersections between rows and columns
- **Areas**: Rectangular regions that can occupy multiple cells

## Container Properties

```css
.container {
  display: grid;
  grid-template-columns: 100px 1fr 2fr; /* Three columns */
  grid-template-rows: auto 200px; /* Two rows */
  gap: 10px; /* Space between cells */
  grid-template-areas: 
    "header header header"
    "sidebar content ads"
    "footer footer footer";
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
```

## Item Properties

```css
.item {
  grid-column: 1 / 3; /* From line 1 to line 3 (2 columns) */
  grid-row: 2 / 4; /* From line 2 to line 4 (2 rows) */
  grid-area: header; /* Placement in a named area */
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```

## Special Functions

- `repeat()`: `grid-template-columns: repeat(3, 1fr);`
- `minmax()`: `grid-template-columns: minmax(100px, 1fr);`
- `auto-fill`/`auto-fit`: `grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));`

CSS Grid simplifies the creation of layouts that previously required complex solutions and offers precise control over the placement of elements in two dimensions. 