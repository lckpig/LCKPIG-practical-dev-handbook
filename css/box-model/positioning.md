<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/box-model/positioning)
<!-- MULTILANGUAJE MENU END -->

# Posicionamiento en CSS

El posicionamiento en CSS permite controlar la ubicación exacta de los elementos en la página, sacándolos del flujo normal de documento cuando sea necesario.

## Propiedad position

CSS ofrece cinco valores para la propiedad `position`:

1. **Static** (estático): Posicionamiento normal (predeterminado)
2. **Relative** (relativo): Posicionado relativo a su posición normal
3. **Absolute** (absoluto): Posicionado relativo al ancestro posicionado más cercano
4. **Fixed** (fijo): Posicionado relativo a la ventana del navegador
5. **Sticky** (pegajoso): Híbrido entre relative y fixed

## Propiedades de desplazamiento

Una vez establecido un posicionamiento no estático, puedes usar:

- `top`: Distancia desde el borde superior de referencia
- `right`: Distancia desde el borde derecho de referencia
- `bottom`: Distancia desde el borde inferior de referencia
- `left`: Distancia desde el borde izquierdo de referencia
- `z-index`: Control del apilamiento vertical (qué elemento aparece encima)

```css
.relativo {
  position: relative;
  top: 20px;
  left: 30px;
}

.absoluto {
  position: absolute;
  top: 0;
  right: 0;
}
```

## Casos de uso comunes

- **Relative**: Ajustar ligeramente la posición sin afectar a otros elementos
- **Absolute**: Crear elementos superpuestos, menús desplegables
- **Fixed**: Barras de navegación que permanecen visibles al desplazarse
- **Sticky**: Encabezados de secciones que se fijan al desplazarse

El posicionamiento es una herramienta poderosa, pero debe usarse con cuidado para evitar problemas de accesibilidad y diseño responsivo. 