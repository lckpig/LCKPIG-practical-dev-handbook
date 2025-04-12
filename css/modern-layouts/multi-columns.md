<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/modern-layouts/multi-columns)
<!-- MULTILANGUAJE MENU END -->

# Multi-columnas en CSS

El módulo CSS Multi-column permite crear diseños de múltiples columnas similares a los periódicos o revistas, donde el contenido fluye automáticamente de una columna a otra.

## Propiedades principales

```css
.contenedor {
  /* Establecer número de columnas */
  column-count: 3;
  
  /* O establecer ancho de columnas y dejar que el navegador calcule el número */
  column-width: 200px;
  
  /* Atajo para ambas propiedades */
  columns: 3 200px;
  
  /* Espacio entre columnas */
  column-gap: 2em;
  
  /* Línea divisoria entre columnas */
  column-rule: 1px solid #ddd;
  
  /* Estabilización de ancho */
  column-fill: auto | balance;
}
```

## Control de elementos dentro de columnas

```css
.titulo {
  /* Hacer que un elemento abarque todas las columnas */
  column-span: all;
}

.no-romper {
  /* Evitar que un elemento se rompa entre columnas */
  break-inside: avoid;
}
```

## Casos de uso comunes

- Textos largos para mejorar la legibilidad
- Listas de elementos (como tarjetas de productos)
- Formularios con múltiples campos
- Galerías de imágenes

## Consideraciones

- El contenido fluye de arriba a abajo y luego a la siguiente columna
- Funciona mejor con contenido homogéneo
- Puede ser difícil controlar el equilibrio exacto entre columnas
- Combina bien con media queries para diseños responsivos

Las multi-columnas son ideales para mejorar la legibilidad de textos largos en pantallas anchas y crear diseños tipo revista o periódico. 