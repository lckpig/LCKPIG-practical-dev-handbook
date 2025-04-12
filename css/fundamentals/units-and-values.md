<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/fundamentals/units-and-values)
<!-- MULTILANGUAJE MENU END -->

# Unidades y Valores en CSS

CSS utiliza diferentes tipos de unidades y valores para definir tamaños, colores y otras propiedades visuales. Comprender estas unidades es esencial para crear diseños precisos y adaptables.

## Unidades de longitud

### Unidades absolutas
- `px` (píxeles): La unidad más común, un punto en la pantalla
- `pt` (puntos): Utilizado principalmente para impresión (1pt = 1/72 pulgada)
- `cm`, `mm`, `in` (centímetros, milímetros, pulgadas): Unidades físicas

### Unidades relativas
- `%` (porcentaje): Relativo al elemento padre
- `em`: Relativo al tamaño de fuente del elemento
- `rem`: Relativo al tamaño de fuente del elemento raíz (html)
- `vw`, `vh`: 1% del ancho o alto de la ventana
- `vmin`, `vmax`: 1% de la dimensión mínima o máxima de la ventana

## Valores de color

- **Nombres**: `red`, `blue`, `transparent`...
- **Hexadecimal**: `#FF0000` (rojo), `#00FF00` (verde)
- **RGB/RGBA**: `rgb(255, 0, 0)`, `rgba(255, 0, 0, 0.5)`
- **HSL/HSLA**: `hsl(0, 100%, 50%)`, `hsla(0, 100%, 50%, 0.5)`

## Otros valores comunes

- **Números**: `1`, `2.5`, `-10` (sin unidades)
- **Tiempo**: `1s` (segundos), `500ms` (milisegundos)
- **Funciones**: `calc()`, `min()`, `max()`, `clamp()`
- **Identificadores**: `inherit`, `initial`, `unset`

## Cuándo usar diferentes unidades

- Usar `rem` para texto para permitir escalado accesible
- Usar `%` y unidades de viewport para layouts responsivos
- Usar `em` para espaciado relacionado con el tamaño de texto

Elegir las unidades correctas para cada propiedad ayuda a crear diseños flexibles y mantenibles que se adaptan a diferentes dispositivos y preferencias de usuario. 