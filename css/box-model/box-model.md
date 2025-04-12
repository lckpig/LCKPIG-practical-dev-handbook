<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/box-model/box-model)
<!-- MULTILANGUAJE MENU END -->

# The Box Model in CSS

The box model is fundamental in CSS and describes how each HTML element is represented as a rectangular box composed of different areas.

## Box Model Components

1. **Content**: The area where text, images, or other media is displayed
2. **Padding**: Transparent space around the content
3. **Border**: Line that surrounds the padding and content
4. **Margin**: Transparent space outside the border

```css
div {
  width: 300px;        /* Content width */
  height: 200px;       /* Content height */
  padding: 20px;       /* Padding on all sides */
  border: 5px solid;   /* Border on all sides */
  margin: 10px;        /* Margin on all sides */
}
```

## Box Model Types

CSS offers two different box models:

1. **Standard Box Model (content-box)**: 
   - Specified width and height apply only to the content
   - Total size = content + padding + border + margin

2. **Alternative Box Model (border-box)**:
   - Specified width and height include content, padding, and border
   - Total size = specified width/height + margin

```css
* {
  box-sizing: border-box; /* Switch to the alternative model */
}
```

## Important Related Properties

- `width` and `height`: Dimensions of the content (or content+padding+border in border-box)
- `min-width`, `max-width`, `min-height`, `max-height`: Dimension limits
- `overflow`: Controls what happens when content overflows its dimensions

Understanding the box model is essential for correctly calculating element dimensions and creating precise layouts. 