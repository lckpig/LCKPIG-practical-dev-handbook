<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/responsive-design/media-queries)
<!-- MULTILANGUAJE MENU END -->

# Media Queries en CSS

Las Media Queries son una característica clave de CSS que permite aplicar estilos condicionalmente según las características del dispositivo o ventana donde se muestra el contenido.

## Sintaxis básica

```css
@media tipo-de-medio and (característica: valor) {
  /* Reglas CSS que se aplicarán cuando se cumpla la condición */
}
```

## Tipos de medios

- **all**: Todos los dispositivos (por defecto)
- **screen**: Pantallas de computadoras, tablets, smartphones
- **print**: Impresoras o vista previa de impresión
- **speech**: Sintetizadores de voz

## Características comunes

- **width**, **min-width**, **max-width**: Ancho de la ventana
- **height**, **min-height**, **max-height**: Alto de la ventana
- **orientation**: `portrait` o `landscape`
- **aspect-ratio**: Relación de aspecto
- **resolution**: Resolución en dpi (puntos por pulgada)
- **hover**: Si el dispositivo permite hover
- **prefers-color-scheme**: Preferencia de tema oscuro/claro

## Ejemplo práctico

```css
/* Estilos base para todos los dispositivos */
body {
  font-size: 16px;
}

/* Estilos para tablets (768px a 1023px) */
@media screen and (min-width: 768px) and (max-width: 1023px) {
  body {
    font-size: 18px;
  }
}

/* Estilos para escritorio (1024px y más) */
@media screen and (min-width: 1024px) {
  body {
    font-size: 20px;
  }
}

/* Estilos para impresión */
@media print {
  body {
    font-size: 12pt;
    color: black;
  }
}
```

## Operadores lógicos

- **and**: Combina condiciones (todas deben cumplirse)
- **,** (coma): Equivalente a OR (cualquiera puede cumplirse)
- **not**: Niega una media query
- **only**: Previene que navegadores antiguos apliquen los estilos

Las media queries son fundamentales para implementar el diseño responsivo y garantizar que los sitios web se vean correctamente en cualquier dispositivo. 