<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/box-model/display-and-visibility)
<!-- MULTILANGUAJE MENU END -->

# Display and Visibility in CSS

The `display` and `visibility` properties control how elements are rendered on the page and are fundamental to web design.

## The display Property

The `display` property determines the type of rendering box used by an element.

### Common display Values

- **block**: Occupies all available width, creates line breaks
- **inline**: Adjusts to content, does not accept dimensions
- **inline-block**: Hybrid that allows dimensions without forcing line breaks
- **none**: Completely removes the element from the flow (occupies no space)
- **flex**: Creates a flexible container for one-dimensional layouts
- **grid**: Creates a grid container for two-dimensional layouts
- **table**, **table-row**, **table-cell**: Emulates table behavior

```css
.block { display: block; }
.inline { display: inline; }
.inline-block { display: inline-block; }
.hidden { display: none; }
```

## The visibility Property

The `visibility` property controls whether an element is visible, but unlike `display: none`, it maintains the space occupied by the element.

- **visible**: The element is visible (default)
- **hidden**: The element is invisible but maintains its space
- **collapse**: Special for table rows/columns (they collapse)

```css
.invisible { visibility: hidden; }
```

## Key Differences

| Property             | Visual Effect | Space in Document | Events             | Accessibility                   |
| -------------------- | ------------- | ----------------- | ------------------ | ------------------------------- |
| `display: none`      | Not visible   | Occupies no space | Receives no events | Removed from accessibility tree |
| `visibility: hidden` | Not visible   | Maintains space   | Receives no events | Present in accessibility tree   |

These properties are essential for controlling the presentation and behavior of elements in different contexts and devices. 