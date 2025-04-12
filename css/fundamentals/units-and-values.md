<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/fundamentals/units-and-values)
<!-- MULTILANGUAJE MENU END -->

# Units and Values in CSS

CSS uses different types of units and values to define sizes, colors, and other visual properties. Understanding these units is essential for creating precise and adaptable designs.

## Length Units

### Absolute Units
- `px` (pixels): The most common unit, a point on the screen
- `pt` (points): Primarily used for printing (1pt = 1/72 inch)
- `cm`, `mm`, `in` (centimeters, millimeters, inches): Physical units

### Relative Units
- `%` (percentage): Relative to the parent element
- `em`: Relative to the element's font size
- `rem`: Relative to the root element's font size (html)
- `vw`, `vh`: 1% of viewport width or height
- `vmin`, `vmax`: 1% of viewport's minimum or maximum dimension

## Color Values

- **Names**: `red`, `blue`, `transparent`...
- **Hexadecimal**: `#FF0000` (red), `#00FF00` (green)
- **RGB/RGBA**: `rgb(255, 0, 0)`, `rgba(255, 0, 0, 0.5)`
- **HSL/HSLA**: `hsl(0, 100%, 50%)`, `hsla(0, 100%, 50%, 0.5)`

## Other Common Values

- **Numbers**: `1`, `2.5`, `-10` (without units)
- **Time**: `1s` (seconds), `500ms` (milliseconds)
- **Functions**: `calc()`, `min()`, `max()`, `clamp()`
- **Identifiers**: `inherit`, `initial`, `unset`

## When to Use Different Units

- Use `rem` for text to allow accessible scaling
- Use `%` and viewport units for responsive layouts
- Use `em` for spacing related to text size

Choosing the right units for each property helps create flexible and maintainable designs that adapt to different devices and user preferences. 