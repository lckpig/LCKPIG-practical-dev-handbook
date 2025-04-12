<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/optimization/critical-css)
<!-- MULTILANGUAJE MENU END -->

# Critical CSS

El Critical CSS es una técnica de optimización de rendimiento que identifica y entrega primero el CSS esencial para renderizar la parte visible de una página web (above-the-fold), permitiendo una visualización más rápida del contenido inicial.

## ¿Qué es el Critical CSS?

Es el conjunto mínimo de CSS necesario para mostrar correctamente la parte de la página que el usuario ve sin desplazarse (above-the-fold). Al entregar este CSS crítico de forma prioritaria, se elimina el bloqueo de renderizado causado por hojas de estilo grandes.

## Beneficios

- **Renderizado más rápido**: El contenido visible se muestra antes
- **Mejora en métricas web vitales**: Reduce LCP (Largest Contentful Paint) y FCP (First Contentful Paint)
- **Mejor experiencia de usuario**: Reducción de la percepción de carga
- **Mejora en SEO**: Las métricas de rendimiento son factores de ranking

## Implementación del Critical CSS

### 1. Extracción del CSS crítico

Existen varias herramientas automatizadas:

- **Critical**: Herramienta Node.js para extraer CSS crítico
- **criticalCSS**: Otra opción basada en Node.js
- **Penthouse**: Generador de Critical CSS
- **CriticalPath**: Plugin para tareas de construcción

### 2. Inclusión inline del CSS crítico

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    /* CSS crítico incrustado */
    header { background-color: #fff; height: 60px; }
    .hero { padding: 20px; font-size: 2rem; }
    /* ... más estilos críticos ... */
  </style>
  
  <!-- Cargar el resto del CSS de forma no bloqueante -->
  <link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="styles.css"></noscript>
</head>
<body>
  <!-- Contenido de la página -->
</body>
</html>
```

### 3. Carga asíncrona del CSS no crítico

```html
<script>
  // Función para cargar CSS de forma asíncrona
  function loadCSS(src) {
    const link = document.createElement('link');
    link.rel = 'stylesheet';
    link.href = src;
    document.head.appendChild(link);
  }
  
  // Cargar CSS completo de forma asíncrona
  if (window.requestIdleCallback) {
    requestIdleCallback(() => {
      loadCSS('styles.css');
    });
  } else {
    setTimeout(() => {
      loadCSS('styles.css');
    }, 1);
  }
</script>
```

## Enfoques alternativos

### Sistema basado en componentes

Con frameworks como React, Vue o Angular, puedes cargar CSS específico por componente, entregando solo los estilos necesarios:

```jsx
// Componente React con CSS específico
import './Button.css';

function Button() {
  return <button className="btn">Click Me</button>;
}
```

### CSS-in-JS con extracción crítica

Algunas bibliotecas CSS-in-JS permiten extracción automática de CSS crítico:

```jsx
// Usando styled-components
import styled from 'styled-components';

const Button = styled.button`
  background: blue;
  color: white;
  padding: 10px 15px;
`;
```

## Consideraciones importantes

- **Mantenimiento**: El Critical CSS debe actualizarse cuando cambia el contenido above-the-fold
- **Múltiples plantillas**: Cada tipo de página puede necesitar su propio CSS crítico
- **Caché del navegador**: Considerar cómo afecta a visitantes recurrentes
- **Viewport variables**: El contenido above-the-fold varía según el dispositivo

## Automatización en el proceso de desarrollo

Para proyectos grandes, la generación manual del Critical CSS puede ser ineficiente. Integra la extracción en tu proceso de build:

- **Webpack**: critical-css-webpack-plugin
- **Gulp**: gulp-critical-css
- **Netlify/Vercel**: Soluciones integradas para generación automatizada

El Critical CSS es una optimización avanzada que puede mejorar significativamente la experiencia de carga inicial, pero debe implementarse cuidadosamente y mantenerse al día con los cambios en el diseño. 