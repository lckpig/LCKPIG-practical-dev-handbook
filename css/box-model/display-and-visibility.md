<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/box-model/display-and-visibility)
<!-- MULTILANGUAJE MENU END -->

# Display y Visibilidad en CSS

Las propiedades `display` y `visibility` controlan cómo se renderizan los elementos en la página y son fundamentales para el diseño web.

## Propiedad display

La propiedad `display` determina el tipo de caja de renderizado que usa un elemento.

### Valores comunes de display

- **block**: Ocupa todo el ancho disponible, crea saltos de línea
- **inline**: Se ajusta al contenido, no acepta dimensiones
- **inline-block**: Híbrido que permite dimensiones sin forzar saltos de línea
- **none**: Elimina completamente el elemento del flujo (no ocupa espacio)
- **flex**: Crea un contenedor flexible para diseños unidimensionales
- **grid**: Crea un contenedor de cuadrícula para diseños bidimensionales
- **table**, **table-row**, **table-cell**: Emula comportamiento de tablas

```css
.bloque { display: block; }
.enlinea { display: inline; }
.enlinea-bloque { display: inline-block; }
.oculto { display: none; }
```

## Propiedad visibility

La propiedad `visibility` controla si un elemento es visible, pero a diferencia de `display: none`, mantiene el espacio que ocupa el elemento.

- **visible**: El elemento es visible (predeterminado)
- **hidden**: El elemento es invisible pero mantiene su espacio
- **collapse**: Especial para filas/columnas de tablas (se colapsan)

```css
.invisible { visibility: hidden; }
```

## Diferencias clave

| Propiedad            | Efecto visual | Espacio en documento | Eventos           | Accesibilidad                         |
| -------------------- | ------------- | -------------------- | ----------------- | ------------------------------------- |
| `display: none`      | No visible    | No ocupa espacio     | No recibe eventos | Removido del árbol de accesibilidad   |
| `visibility: hidden` | No visible    | Mantiene espacio     | No recibe eventos | Presente en el árbol de accesibilidad |

Estas propiedades son esenciales para controlar la presentación y el comportamiento de los elementos en diferentes contextos y dispositivos. 