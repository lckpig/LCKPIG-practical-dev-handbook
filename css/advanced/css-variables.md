<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/advanced/css-variables)
<!-- MULTILANGUAJE MENU END -->

# Variables CSS (Custom Properties)

Las variables CSS, también conocidas como custom properties, permiten almacenar valores específicos para reutilizarlos a lo largo de un documento, añadiendo dinamismo y simplificando el mantenimiento de hojas de estilo.

## Sintaxis básica

```css
/* Declaración de variables (generalmente en el elemento :root) */
:root {
  --color-primario: #3498db;
  --color-secundario: #2ecc71;
  --espaciado-base: 1rem;
  --fuente-principal: 'Roboto', sans-serif;
}

/* Uso de las variables */
.boton {
  background-color: var(--color-primario);
  padding: var(--espaciado-base);
  font-family: var(--fuente-principal);
}
```

## Características clave

### Alcance (Scope)

Las variables CSS respetan el alcance del DOM donde se definen:

```css
:root {
  --color: blue; /* Variable global */
}

.componente {
  --color: red; /* Variable local para .componente y sus descendientes */
  color: var(--color); /* Rojo */
}

.otro-elemento {
  color: var(--color); /* Azul, usa la variable global */
}
```

### Valores fallback (predeterminados)

```css
.elemento {
  /* Si --ancho no está definido, se usará 100px */
  width: var(--ancho, 100px);
}
```

### Anidación de variables

```css
:root {
  --base-hue: 200;
  --color-primario: hsl(var(--base-hue), 80%, 50%);
}
```

## Manipulación con JavaScript

Las variables CSS pueden ser leídas y modificadas con JavaScript, permitiendo estilos dinámicos:

```javascript
// Leer una variable CSS
const colorPrimario = getComputedStyle(document.documentElement)
  .getPropertyValue('--color-primario');

// Modificar una variable CSS
document.documentElement.style.setProperty('--color-primario', '#ff0000');
```

## Casos de uso comunes

- **Temas de color**: Facilitar cambios de tema (modo claro/oscuro)
- **Sistemas de diseño**: Mantener consistencia en componentes
- **Responsive design**: Cambiar valores según el tamaño de pantalla
- **Animaciones**: Animar propiedades cambiando variables
- **Estados de componentes**: Modificar variables según estados de UI

## Ventajas sobre preprocesadores

- **Dinamismo**: Las variables CSS son reactivas, no solo en compilación
- **Interactividad**: Pueden modificarse con JavaScript
- **Alcance DOM**: Respetan la cascada y el alcance de elementos
- **Estándar nativo**: No requieren compilación

## Limitaciones

- **Soporte navegadores**: No disponible en IE11 y navegadores muy antiguos
- **Funcionalidad limitada**: No permiten operaciones complejas sin funciones `calc()`
- **Sin condicionales nativos**: No tienen estructuras de control incorporadas

Las variables CSS son una herramienta poderosa para crear estilos más dinámicos, mantenibles y sistemáticos en aplicaciones web modernas. 