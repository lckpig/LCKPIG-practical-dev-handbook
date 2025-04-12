<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/box-model/positioning)
<!-- MULTILANGUAJE MENU END -->

# Positioning in CSS

Positioning in CSS allows you to control the exact location of elements on the page, taking them out of the normal document flow when necessary.

## The position Property

CSS offers five values for the `position` property:

1. **Static**: Normal positioning (default)
2. **Relative**: Positioned relative to its normal position
3. **Absolute**: Positioned relative to the nearest positioned ancestor
4. **Fixed**: Positioned relative to the browser window
5. **Sticky**: Hybrid between relative and fixed

## Offset Properties

Once a non-static positioning is established, you can use:

- `top`: Distance from the top reference edge
- `right`: Distance from the right reference edge
- `bottom`: Distance from the bottom reference edge
- `left`: Distance from the left reference edge
- `z-index`: Control of vertical stacking (which element appears on top)

```css
.relative {
  position: relative;
  top: 20px;
  left: 30px;
}

.absolute {
  position: absolute;
  top: 0;
  right: 0;
}
```

## Common Use Cases

- **Relative**: Slightly adjust position without affecting other elements
- **Absolute**: Create overlapping elements, dropdown menus
- **Fixed**: Navigation bars that remain visible when scrolling
- **Sticky**: Section headers that stick when scrolling

Positioning is a powerful tool, but should be used carefully to avoid accessibility and responsive design issues. 