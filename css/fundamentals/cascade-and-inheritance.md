<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/fundamentals/cascade-and-inheritance)
<!-- MULTILANGUAJE MENU END -->

# Cascada y Herencia en CSS

La cascada y la herencia son dos conceptos fundamentales que determinan cómo se aplican los estilos en CSS.

## Cascada

La cascada es el algoritmo que determina qué reglas CSS se aplican cuando hay múltiples reglas que afectan a un mismo elemento. El orden de prioridad es:

1. **Importancia**: Declaraciones con `!important`
2. **Especificidad**: Según el tipo de selector (inline > id > clase > elemento)
3. **Orden de aparición**: La última regla definida prevalece

```css
p { color: blue; }         /* Menor prioridad */
.texto { color: green; }   /* Mayor prioridad */
```

## Herencia

La herencia es el mecanismo por el cual algunas propiedades CSS se transmiten automáticamente de los elementos padres a sus descendientes.

### Propiedades que se heredan

- `color`
- `font-family`, `font-size`
- `text-align`
- `line-height`

### Propiedades que no se heredan

- `margin`, `padding`
- `border`
- `background`
- `height`, `width`

## Control de herencia

CSS proporciona valores especiales para controlar la herencia:

- `inherit`: Fuerza la herencia desde el elemento padre
- `initial`: Establece el valor inicial definido por la especificación CSS
- `unset`: Combina `inherit` e `initial` según la propiedad
- `revert`: Revierte al estilo del navegador

Comprender la cascada y la herencia es esencial para predecir cómo se aplicarán los estilos y resolver conflictos entre reglas CSS. 