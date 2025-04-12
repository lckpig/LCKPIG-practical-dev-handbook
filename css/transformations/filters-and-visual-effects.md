<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/transformations/filters-and-visual-effects)
<!-- MULTILANGUAJE MENU END -->

# Filtros y Efectos Visuales en CSS

Los filtros y efectos visuales CSS permiten aplicar efectos gráficos directamente a elementos HTML, similar a los que encontrarías en software de edición de imágenes.

## Filtros CSS

La propiedad `filter` proporciona efectos de procesamiento gráfico a elementos:

```css
.imagen {
  /* Un solo filtro */
  filter: blur(5px);
  
  /* Múltiples filtros */
  filter: grayscale(50%) brightness(120%) contrast(1.2);
}
```

### Funciones de filtro disponibles

- **blur(radio)**: Desenfoca el elemento
- **brightness(cantidad)**: Ajusta el brillo
- **contrast(cantidad)**: Ajusta el contraste
- **grayscale(porcentaje)**: Convierte a escala de grises
- **hue-rotate(ángulo)**: Rota los colores en el círculo cromático
- **invert(porcentaje)**: Invierte los colores
- **opacity(porcentaje)**: Ajusta la opacidad
- **saturate(cantidad)**: Ajusta la saturación
- **sepia(porcentaje)**: Convierte a tonos sepia
- **drop-shadow(x y difuminado color)**: Añade sombra similar a box-shadow

## Mezcla de modos (Blend Modes)

CSS ofrece dos propiedades para mezclar colores entre capas:

### Background Blend Mode

Mezcla capas de fondo entre sí:

```css
.elemento {
  background-image: url('textura.jpg'), linear-gradient(red, blue);
  background-blend-mode: multiply;
}
```

### Mix Blend Mode

Mezcla un elemento con su fondo:

```css
.elemento {
  background-color: blue;
  mix-blend-mode: screen;
}
```

### Modos de mezcla disponibles

- **normal**: Sin mezcla (predeterminado)
- **multiply**: Efecto multiplicador (oscurece)
- **screen**: Efecto pantalla (aclara)
- **overlay**: Combinación de multiply y screen
- **darken**: Conserva el más oscuro de cada canal
- **lighten**: Conserva el más claro de cada canal
- **color-dodge**: Aclara la capa inferior
- **color-burn**: Oscurece la capa inferior
- **hard-light**, **soft-light**: Variaciones de overlay
- **difference**, **exclusion**: Efectos de inversión
- **hue**, **saturation**, **color**, **luminosity**: Mezcla componentes específicos del color

## Máscara CSS

La propiedad `mask` permite ocultar partes de un elemento usando imágenes o gradientes:

```css
.elemento {
  mask-image: linear-gradient(black, transparent);
  /* o */
  mask-image: url('mascara.svg');
}
```

## Efectos de texto

CSS ofrece propiedades específicas para efectos de texto:

```css
.texto {
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  -webkit-text-stroke: 1px black;
  background-clip: text;
  -webkit-background-clip: text;
  color: transparent;
  background-image: linear-gradient(to right, red, blue);
}
```

## Consideraciones de rendimiento

- Los filtros pueden afectar el rendimiento, especialmente en dispositivos móviles
- Combinar múltiples filtros aumenta la carga de procesamiento
- Algunos efectos avanzados pueden no estar disponibles en navegadores antiguos

Estos efectos visuales permiten crear interfaces ricas visualmente sin necesidad de imágenes o JavaScript, aunque deben usarse con moderación para mantener un buen rendimiento. 