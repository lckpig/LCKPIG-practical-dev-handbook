<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/responsive-design/responsive-images)
<!-- MULTILANGUAJE MENU END -->

# Responsive Images in CSS

Responsive images are an essential part of modern web design, allowing images to adapt to different screen sizes, improving both performance and user experience.

## Basic Techniques with CSS

### Simple Fluid Method

```css
img {
  max-width: 100%;
  height: auto;
}
```

This simple method ensures that images never exceed the width of their container while maintaining their aspect ratio.

## Modern HTML for Responsive Images

### srcset Attribute

```html
<img 
  src="base-image.jpg" 
  srcset="small-image.jpg 480w,
          medium-image.jpg 800w,
          large-image.jpg 1200w"
  sizes="(max-width: 600px) 480px,
         (max-width: 1000px) 800px,
         1200px"
  alt="Image description">
```

### Picture Element

```html
<picture>
  <source media="(max-width: 600px)" srcset="mobile-image.jpg">
  <source media="(max-width: 1000px)" srcset="tablet-image.jpg">
  <img src="desktop-image.jpg" alt="Image description">
</picture>
```

## Advanced Techniques with CSS

### Responsive Background Images

```css
.hero-banner {
  background-image: url('large-image.jpg');
  background-size: cover;
  background-position: center;
}

@media (max-width: 768px) {
  .hero-banner {
    background-image: url('small-image.jpg');
  }
}
```

### Constant Aspect Ratio

```css
.aspect-ratio-container {
  position: relative;
  width: 100%;
  padding-bottom: 56.25%; /* For 16:9 ratio */
}

.aspect-ratio-container img {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

## Performance Considerations

- **File size**: Optimize images to reduce weight
- **Appropriate format**: Use WebP, AVIF for better compression
- **Lazy loading**: Implement `loading="lazy"` for off-screen images
- **Retina images**: Consider high-pixel-density screens

Responsive images not only improve the visual experience across different devices but also significantly contribute to site performance by loading only the necessary image size. 