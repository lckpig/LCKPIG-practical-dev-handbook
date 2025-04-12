<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/box-model/box-model)
<!-- MULTILANGUAJE MENU END -->

# El Modelo de Caja (Box Model) en CSS

El modelo de caja es fundamental en CSS y describe cómo cada elemento HTML se representa como una caja rectangular compuesta por diferentes áreas.

## Componentes del modelo de caja

1. **Contenido (Content)**: El área donde se muestra el texto, imágenes u otros medios
2. **Relleno (Padding)**: Espacio transparente alrededor del contenido
3. **Borde (Border)**: Línea que rodea el padding y el contenido
4. **Margen (Margin)**: Espacio transparente fuera del borde

```css
div {
  width: 300px;        /* Ancho del contenido */
  height: 200px;       /* Alto del contenido */
  padding: 20px;       /* Relleno en todos los lados */
  border: 5px solid;   /* Borde en todos los lados */
  margin: 10px;        /* Margen en todos los lados */
}
```

## Tipos de modelo de caja

CSS ofrece dos modelos de caja diferentes:

1. **Modelo de caja estándar (content-box)**: 
   - El ancho y alto especificados aplican solo al contenido
   - El tamaño total = contenido + padding + border + margin

2. **Modelo de caja alternativo (border-box)**:
   - El ancho y alto especificados incluyen contenido, padding y border
   - El tamaño total = ancho/alto especificado + margin

```css
* {
  box-sizing: border-box; /* Cambiar al modelo alternativo */
}
```

## Propiedades relacionadas importantes

- `width` y `height`: Dimensiones del contenido (o de contenido+padding+border en border-box)
- `min-width`, `max-width`, `min-height`, `max-height`: Límites de dimensiones
- `overflow`: Controla qué sucede cuando el contenido desborda sus dimensiones

Comprender el modelo de caja es esencial para calcular correctamente las dimensiones de los elementos y crear layouts precisos. 