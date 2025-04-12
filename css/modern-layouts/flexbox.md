<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/modern-layouts/flexbox)
<!-- MULTILANGUAJE MENU END -->

# Flexbox en CSS

Flexbox (Flexible Box Layout) es un modelo de diseño unidimensional que permite distribuir el espacio entre elementos en una interfaz y mejorar las capacidades de alineación.

## Conceptos básicos

- **Contenedor flex**: El elemento padre con `display: flex` o `display: inline-flex`
- **Elementos flex**: Los hijos directos del contenedor flex
- **Eje principal**: Dirección principal (horizontal o vertical)
- **Eje cruzado**: Perpendicular al eje principal

## Propiedades del contenedor

```css
.contenedor {
  display: flex;
  flex-direction: row | row-reverse | column | column-reverse;
  flex-wrap: nowrap | wrap | wrap-reverse;
  justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
  align-items: flex-start | flex-end | center | stretch | baseline;
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
  gap: 10px; /* Espacio entre elementos */
}
```

## Propiedades de los elementos

```css
.elemento {
  order: 0; /* Orden de aparición */
  flex-grow: 0; /* Factor de crecimiento */
  flex-shrink: 1; /* Factor de contracción */
  flex-basis: auto; /* Tamaño inicial */
  flex: 0 1 auto; /* Atajo: grow shrink basis */
  align-self: auto | flex-start | flex-end | center | stretch | baseline;
}
```

## Casos de uso comunes

- Centrado vertical y horizontal
- Navegación responsiva
- Layouts de tarjetas
- Distribución equilibrada de elementos
- Reordenamiento de elementos en diferentes tamaños de pantalla

Flexbox es ideal para componentes de una dimensión (filas o columnas) y ofrece soluciones elegantes para muchos problemas de diseño que antes requerían hacks complicados. 