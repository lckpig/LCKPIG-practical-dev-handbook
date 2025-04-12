<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/modern-layouts/css-grid)
<!-- MULTILANGUAJE MENU END -->

# CSS Grid

CSS Grid Layout es un sistema de diseño bidimensional que permite crear layouts complejos de filas y columnas simultáneamente. Es especialmente poderoso para diseños de página completos y componentes complejos.

## Conceptos básicos

- **Contenedor grid**: Elemento con `display: grid` o `display: inline-grid`
- **Elementos grid**: Hijos directos del contenedor grid
- **Líneas grid**: Líneas horizontales y verticales que dividen la cuadrícula
- **Pistas**: Filas y columnas de la cuadrícula
- **Celdas**: Intersecciones entre filas y columnas
- **Áreas**: Regiones rectangulares que pueden ocupar múltiples celdas

## Propiedades del contenedor

```css
.contenedor {
  display: grid;
  grid-template-columns: 100px 1fr 2fr; /* Tres columnas */
  grid-template-rows: auto 200px; /* Dos filas */
  gap: 10px; /* Espacio entre celdas */
  grid-template-areas: 
    "header header header"
    "sidebar contenido anuncios"
    "footer footer footer";
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
```

## Propiedades de los elementos

```css
.elemento {
  grid-column: 1 / 3; /* De línea 1 a línea 3 (2 columnas) */
  grid-row: 2 / 4; /* De línea 2 a línea 4 (2 filas) */
  grid-area: header; /* Colocación en un área nombrada */
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```

## Funciones especiales

- `repeat()`: `grid-template-columns: repeat(3, 1fr);`
- `minmax()`: `grid-template-columns: minmax(100px, 1fr);`
- `auto-fill`/`auto-fit`: `grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));`

CSS Grid simplifica la creación de layouts que antes requerían soluciones complejas, y ofrece un control preciso sobre la colocación de elementos en dos dimensiones. 