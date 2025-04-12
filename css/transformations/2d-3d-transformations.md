<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/transformations/2d-3d-transformations)
<!-- MULTILANGUAJE MENU END -->

# Transformaciones 2D y 3D en CSS

Las transformaciones CSS permiten modificar la posición, tamaño y forma de los elementos HTML en espacios bidimensionales y tridimensionales, creando efectos visuales avanzados.

## Transformaciones 2D

Las transformaciones 2D manipulan elementos en el plano X-Y.

```css
.elemento {
  /* Transformación individual */
  transform: translateX(20px);
  
  /* Múltiples transformaciones */
  transform: rotate(45deg) scale(1.5);
}
```

### Funciones de transformación 2D

- **translate(x, y)**: Mueve el elemento horizontal y verticalmente
  - `translateX(x)`, `translateY(y)`
- **scale(x, y)**: Cambia el tamaño del elemento
  - `scaleX(n)`, `scaleY(n)`
- **rotate(ángulo)**: Gira el elemento (deg, grad, rad, turn)
- **skew(x, y)**: Inclina el elemento
  - `skewX(ángulo)`, `skewY(ángulo)`
- **matrix()**: Transformación mediante una matriz 2D

## Transformaciones 3D

Las transformaciones 3D extienden las posibilidades al espacio tridimensional (X, Y, Z).

```css
.escena {
  /* Establecer perspectiva para efectos 3D */
  perspective: 800px;
}

.elemento-3d {
  transform: rotateY(45deg) translateZ(50px);
  transform-style: preserve-3d; /* Mantener hijos en espacio 3D */
}
```

### Funciones de transformación 3D

- **translate3d(x, y, z)**: Mueve en las tres dimensiones
  - `translateZ(z)`
- **scale3d(x, y, z)**: Escala en las tres dimensiones
  - `scaleZ(z)`
- **rotate3d(x, y, z, ángulo)**: Rotación 3D
  - `rotateX(ángulo)`, `rotateY(ángulo)`, `rotateZ(ángulo)`
- **perspective(n)**: Establece perspectiva directamente en el elemento
- **matrix3d()**: Transformación mediante una matriz 3D

### Propiedades adicionales para 3D

- **perspective**: Define la distancia entre el usuario y el plano z=0
- **perspective-origin**: Define el punto de origen de la perspectiva
- **transform-style**: Determina si los hijos conservan transformaciones 3D
- **backface-visibility**: Controla si la parte trasera es visible

## Punto de origen

La propiedad `transform-origin` especifica el punto desde el que se aplica la transformación:

```css
.elemento {
  transform: rotate(45deg);
  transform-origin: top left; /* Gira desde la esquina superior izquierda */
}
```

## Consideraciones de rendimiento

- Las transformaciones son procesadas por la GPU, resultando en mejor rendimiento
- Preferir transformaciones en lugar de cambiar propiedades como `top`, `left`, `width`, `height`
- Las transformaciones 3D fuerzan la aceleración por hardware incluso en elementos 2D

Las transformaciones CSS son fundamentales para crear interfaces modernas con efectos visuales sofisticados sin sacrificar el rendimiento. 