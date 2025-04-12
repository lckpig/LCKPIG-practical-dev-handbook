<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/transformations/2d-3d-transformations)
<!-- MULTILANGUAJE MENU END -->

# 2D and 3D Transformations in CSS

CSS transforms allow elements to be visually manipulated in two-dimensional or three-dimensional space without affecting the document flow.

## 2D Transformations

### Basic Transform Functions

```css
.element {
  /* Basic transforms */
  transform: translateX(20px);      /* Move right 20px */
  transform: translateY(20px);      /* Move down 20px */
  transform: translate(20px, 20px); /* Move right and down */
  
  transform: scaleX(1.5);           /* Scale width by 150% */
  transform: scaleY(0.5);           /* Scale height by 50% */
  transform: scale(1.5, 0.5);       /* Scale width and height */
  
  transform: rotate(45deg);         /* Rotate 45 degrees clockwise */
  
  transform: skewX(10deg);          /* Skew along X axis */
  transform: skewY(10deg);          /* Skew along Y axis */
  transform: skew(10deg, 10deg);    /* Skew along both axes */
}
```

### Combining Transforms

```css
.element {
  /* Compound transformations */
  transform: translate(100px, 50px) rotate(45deg) scale(1.5);
}
```

### Transform Origin

The point around which transformations are applied.

```css
.element {
  transform: rotate(45deg);
  transform-origin: center;    /* Default */
  transform-origin: top left;  /* Rotate around top-left corner */
  transform-origin: 100px 50px; /* Custom point */
}
```

## 3D Transformations

### Setting Up 3D Space

```css
.container {
  /* Create a 3D space for children */
  perspective: 1000px;
  perspective-origin: center;
}

.element {
  /* Preserve 3D for nested children */
  transform-style: preserve-3d;
}
```

### 3D Transform Functions

```css
.element {
  /* 3D translations */
  transform: translateZ(50px);                /* Move along Z axis */
  transform: translate3d(10px, 20px, 50px);   /* X, Y, and Z translation */
  
  /* 3D rotations */
  transform: rotateX(45deg);                  /* Rotate around X axis */
  transform: rotateY(45deg);                  /* Rotate around Y axis */
  transform: rotateZ(45deg);                  /* Same as 2D rotate() */
  transform: rotate3d(1, 1, 1, 45deg);        /* Rotate around a vector */
  
  /* 3D scaling */
  transform: scaleZ(1.5);                     /* Scale along Z axis */
  transform: scale3d(1.5, 1.5, 1.5);          /* Scale in all dimensions */
}
```

### Backface Visibility

Controls whether the back of an element is visible when rotated.

```css
.element {
  backface-visibility: visible; /* Default */
  backface-visibility: hidden;  /* Hide when facing away */
}
```

## Practical Examples

### 3D Card Flip

```css
.card-container {
  perspective: 1000px;
  width: 200px;
  height: 300px;
}

.card {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 0.6s;
}

.card:hover {
  transform: rotateY(180deg);
}

.front, .back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
}

.back {
  transform: rotateY(180deg);
}
```

### Image Gallery with Perspective

```css
.gallery {
  perspective: 1000px;
  display: flex;
  justify-content: center;
}

.image {
  width: 150px;
  height: 200px;
  margin: 0 10px;
  transition: transform 0.5s;
}

.image:hover {
  transform: rotateY(30deg) translateZ(50px);
}
```

## Browser Support and Performance

- Most modern browsers support 2D and 3D transforms
- Hardware acceleration is automatically applied for most transforms
- Complex 3D transformations can impact performance on mobile devices
- Use `will-change: transform` for better performance on animations

CSS transforms provide a powerful way to create visually engaging layouts and interactions while maintaining good performance and browser compatibility. 