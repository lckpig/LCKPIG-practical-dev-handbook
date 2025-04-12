<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/optimization/critical-css)
<!-- MULTILANGUAJE MENU END -->

# Critical CSS

Critical CSS is a performance optimization technique that identifies and delivers the essential CSS needed to render the visible portion of a web page (above-the-fold) first, allowing for faster initial content display.

## What is Critical CSS?

It's the minimum set of CSS needed to properly display the portion of the page that users see without scrolling (above-the-fold). By delivering this critical CSS with priority, it eliminates the render-blocking caused by large stylesheets.

## Benefits

- **Faster rendering**: Visible content appears sooner
- **Improved web vital metrics**: Reduces LCP (Largest Contentful Paint) and FCP (First Contentful Paint)
- **Better user experience**: Reduced perception of loading
- **SEO improvements**: Performance metrics are ranking factors

## Implementing Critical CSS

### 1. Extracting Critical CSS

Several automated tools exist:

- **Critical**: Node.js tool for extracting critical CSS
- **criticalCSS**: Another Node.js-based option
- **Penthouse**: Critical CSS generator
- **CriticalPath**: Plugin for build tasks

### 2. Inline Critical CSS

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    /* Inlined critical CSS */
    header { background-color: #fff; height: 60px; }
    .hero { padding: 20px; font-size: 2rem; }
    /* ... more critical styles ... */
  </style>
  
  <!-- Load the rest of CSS non-blocking -->
  <link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="styles.css"></noscript>
</head>
<body>
  <!-- Page content -->
</body>
</html>
```

### 3. Asynchronous Loading of Non-Critical CSS

```html
<script>
  // Function to load CSS asynchronously
  function loadCSS(src) {
    const link = document.createElement('link');
    link.rel = 'stylesheet';
    link.href = src;
    document.head.appendChild(link);
  }
  
  // Load full CSS asynchronously
  if (window.requestIdleCallback) {
    requestIdleCallback(() => {
      loadCSS('styles.css');
    });
  } else {
    setTimeout(() => {
      loadCSS('styles.css');
    }, 1);
  }
</script>
```

## Alternative Approaches

### Component-Based System

With frameworks like React, Vue, or Angular, you can load component-specific CSS, delivering only the styles needed:

```jsx
// React component with specific CSS
import './Button.css';

function Button() {
  return <button className="btn">Click Me</button>;
}
```

### CSS-in-JS with Critical Extraction

Some CSS-in-JS libraries allow automatic critical CSS extraction:

```jsx
// Using styled-components
import styled from 'styled-components';

const Button = styled.button`
  background: blue;
  color: white;
  padding: 10px 15px;
`;
```

## Important Considerations

- **Maintenance**: Critical CSS must be updated when above-the-fold content changes
- **Multiple templates**: Each page type may need its own critical CSS
- **Browser cache**: Consider how it affects returning visitors
- **Viewport variables**: Above-the-fold content varies by device

## Automation in Development Process

For large projects, manual Critical CSS generation can be inefficient. Integrate extraction into your build process:

- **Webpack**: critical-css-webpack-plugin
- **Gulp**: gulp-critical-css
- **Netlify/Vercel**: Integrated solutions for automated generation

Critical CSS is an advanced optimization that can significantly improve the initial loading experience, but must be implemented carefully and kept up-to-date with design changes. 