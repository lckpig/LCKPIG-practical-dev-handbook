<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/transformations/filters-and-visual-effects)
<!-- MULTILANGUAJE MENU END -->

# Filters and Visual Effects in CSS

CSS filters and visual effects provide powerful tools for modifying the appearance of elements, applying visual processing, and creating engaging graphical effects directly in the browser.

## CSS Filters

The `filter` property applies visual effects to elements.

### Basic Filter Functions

```css
.element {
  /* Individual filters */
  filter: blur(5px);         /* Applies a Gaussian blur */
  filter: brightness(150%);  /* Adjusts brightness (0-∞, 100% is normal) */
  filter: contrast(200%);    /* Adjusts contrast (0-∞, 100% is normal) */
  filter: grayscale(80%);    /* Converts to grayscale (0-100%) */
  filter: hue-rotate(90deg); /* Shifts colors around the color wheel */
  filter: invert(75%);       /* Inverts colors (0-100%) */
  filter: opacity(50%);      /* Adjusts transparency (0-100%) */
  filter: saturate(200%);    /* Adjusts saturation (0-∞, 100% is normal) */
  filter: sepia(90%);        /* Converts to sepia (0-100%) */
  filter: drop-shadow(5px 5px 5px gray); /* Adds a drop shadow */
}
```

### Combining Multiple Filters

```css
.multi-filtered {
  /* Apply multiple filters in sequence */
  filter: contrast(175%) brightness(103%) sepia(30%);
}
```

### Use Cases for Filters

```css
/* Darken images for text overlay */
.text-overlay-image {
  filter: brightness(60%);
}

/* Create a "disabled" effect */
.disabled-element {
  filter: grayscale(100%) opacity(70%);
}

/* Vintage photo effect */
.vintage-photo {
  filter: sepia(80%) contrast(110%) brightness(110%) saturate(130%);
}
```

## Backdrop Filters

The `backdrop-filter` property applies filters to the area behind an element.

```css
.frosted-glass {
  background-color: rgba(255, 255, 255, 0.3);
  backdrop-filter: blur(10px);
}

.dimmed-background {
  background-color: rgba(0, 0, 0, 0.2);
  backdrop-filter: brightness(70%);
}
```

## Blend Modes

### Background Blend Mode

Controls how an element's background images and colors blend together.

```css
.blend-example {
  background-image: url('texture.jpg');
  background-color: #3498db;
  background-blend-mode: multiply;
}
```

Available blend modes: `normal`, `multiply`, `screen`, `overlay`, `darken`, `lighten`, `color-dodge`, `color-burn`, `hard-light`, `soft-light`, `difference`, `exclusion`, `hue`, `saturation`, `color`, `luminosity`

### Mix Blend Mode

Controls how an element's content blends with the content of its parent and background.

```css
.mixed-content {
  mix-blend-mode: difference;
}
```

## Masks

CSS masks allow for clipping and transparency effects.

```css
.masked-element {
  /* Using an image as a mask */
  mask-image: url('mask.svg');
  mask-size: cover;
  
  /* Using a gradient as a mask */
  mask-image: linear-gradient(to right, transparent, black);
}
```

## Clipping Paths

The `clip-path` property creates a clipping region that defines what part of an element is visible.

```css
.clipped {
  /* Basic geometric shapes */
  clip-path: circle(50% at center);
  clip-path: ellipse(25% 40% at 50% 50%);
  clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
  clip-path: path('M 0,0 L 100,0 L 50,100 Z'); /* SVG path */
}
```

## Performance Considerations

- Filters and blend modes can be computationally expensive
- Use `will-change: filter` for animations that change filters
- Consider using less intensive effects for mobile devices
- Test performance in multiple browsers

```css
.optimized-filter-animation {
  will-change: filter;
}
```

CSS filters and visual effects provide a wide range of creative possibilities directly in CSS, eliminating the need for image editing software or JavaScript libraries for many common visual effects. 