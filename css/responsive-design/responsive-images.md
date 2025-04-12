<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/responsive-design/responsive-images)
<!-- MULTILANGUAJE MENU END -->

# Imágenes Responsivas en CSS

Las imágenes responsivas son una parte esencial del diseño web moderno, permitiendo que las imágenes se adapten a diferentes tamaños de pantalla, mejorando tanto el rendimiento como la experiencia del usuario.

## Técnicas básicas con CSS

### Método fluido simple

```css
img {
  max-width: 100%;
  height: auto;
}
```

Este método simple hace que las imágenes nunca excedan el ancho de su contenedor, manteniendo su relación de aspecto.

## HTML moderno para imágenes responsivas

### Atributo srcset

```html
<img 
  src="imagen-base.jpg" 
  srcset="imagen-pequena.jpg 480w,
          imagen-mediana.jpg 800w,
          imagen-grande.jpg 1200w"
  sizes="(max-width: 600px) 480px,
         (max-width: 1000px) 800px,
         1200px"
  alt="Descripción de la imagen">
```

### Elemento picture

```html
<picture>
  <source media="(max-width: 600px)" srcset="imagen-movil.jpg">
  <source media="(max-width: 1000px)" srcset="imagen-tablet.jpg">
  <img src="imagen-escritorio.jpg" alt="Descripción de la imagen">
</picture>
```

## Técnicas avanzadas con CSS

### Imágenes de fondo responsivas

```css
.hero-banner {
  background-image: url('imagen-grande.jpg');
  background-size: cover;
  background-position: center;
}

@media (max-width: 768px) {
  .hero-banner {
    background-image: url('imagen-pequena.jpg');
  }
}
```

### Relación de aspecto constante

```css
.aspect-ratio-container {
  position: relative;
  width: 100%;
  padding-bottom: 56.25%; /* Para relación 16:9 */
}

.aspect-ratio-container img {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

## Consideraciones de rendimiento

- **Tamaño de archivo**: Optimizar imágenes para reducir el peso
- **Formato adecuado**: Usar WebP, AVIF para mejor compresión
- **Carga diferida**: Implementar `loading="lazy"` para imágenes fuera de pantalla
- **Imágenes retina**: Considerar pantallas de alta densidad de píxeles

Las imágenes responsivas no solo mejoran la experiencia visual en diferentes dispositivos, sino que también contribuyen significativamente al rendimiento del sitio al cargar solo el tamaño de imagen necesario. 