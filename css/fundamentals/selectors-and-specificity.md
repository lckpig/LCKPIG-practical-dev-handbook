<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/fundamentals/selectors-and-specificity)
<!-- MULTILANGUAJE MENU END -->

# Selectores y Especificidad en CSS

Los selectores CSS son patrones que se utilizan para seleccionar los elementos HTML a los que se aplicarán las reglas de estilo. La especificidad es el mecanismo que utiliza el navegador para determinar qué estilos aplicar cuando varios selectores coinciden con un mismo elemento.

## Tipos de selectores básicos

- **Selector de elemento**: `p`, `div`, `h1`
- **Selector de clase**: `.clase`
- **Selector de ID**: `#identificador`
- **Selector universal**: `*`
- **Selector de atributo**: `[atributo=valor]`

## Combinadores

- **Descendiente**: `div p`
- **Hijo directo**: `div > p`
- **Hermano adyacente**: `h1 + p`
- **Hermano general**: `h1 ~ p`

## Pseudoclases y pseudoelementos

- **Pseudoclases**: `:hover`, `:active`, `:focus`, `:nth-child()`
- **Pseudoelementos**: `::before`, `::after`, `::first-letter`

## Especificidad

La especificidad determina qué regla CSS tiene prioridad cuando varias reglas se aplican al mismo elemento:

1. Estilos en línea (`style="..."`)
2. IDs (#id)
3. Clases (.clase), atributos y pseudoclases
4. Elementos y pseudoelementos

Comprender los selectores y la especificidad es crucial para escribir CSS eficiente y evitar conflictos entre reglas de estilo. 