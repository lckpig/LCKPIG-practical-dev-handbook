<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/advanced/methodologies)
<!-- MULTILANGUAJE MENU END -->

# Metodologías CSS

Las metodologías CSS son conjuntos de reglas y convenciones que ayudan a escribir CSS más mantenible, escalable y organizado, especialmente en proyectos grandes.

## BEM (Block, Element, Modifier)

BEM es una convención de nomenclatura que divide la interfaz en bloques independientes.

### Estructura de nomenclatura

- **Bloque**: Componente autónomo (`.card`)
- **Elemento**: Parte de un bloque (`.card__title`)
- **Modificador**: Variante o estado (`.card--featured`, `.button--disabled`)

```css
/* Bloque */
.card {
  background: white;
  border-radius: 4px;
}

/* Elemento */
.card__title {
  font-size: 1.5rem;
}

/* Elemento */
.card__image {
  width: 100%;
}

/* Modificador */
.card--featured {
  border: 2px solid gold;
}
```

### Ventajas de BEM

- Evita colisiones de nombres
- Clarifica la relación entre HTML y CSS
- Facilita la reutilización de componentes
- Proporciona una estructura modular

## OOCSS (Object-Oriented CSS)

OOCSS se basa en la separación de estructura y apariencia, y la independencia de contenedor y contenido.

### Principios

1. **Separación de estructura y piel**
   - Definir estructuras (width, margin, padding) separadas de apariencias (color, border, background)

2. **Separación de contenedor y contenido**
   - Los elementos no deberían depender de su ubicación

```css
/* Estructura (objeto) */
.media {
  display: flex;
  align-items: flex-start;
}

.media__body {
  flex: 1;
}

/* Piel (tema) */
.theme-light {
  background-color: white;
  color: black;
}

.theme-dark {
  background-color: black;
  color: white;
}
```

## SMACSS (Scalable and Modular Architecture for CSS)

SMACSS categoriza las reglas CSS en cinco tipos diferentes.

### Categorías

1. **Base**
   - Estilos predeterminados para elementos HTML (reset/normalize)

2. **Layout**
   - División principal de la página (header, footer, main)

3. **Módulo**
   - Componentes reutilizables y autónomos

4. **Estado**
   - Describe cómo se ven los módulos en estados particulares

5. **Tema**
   - Define colores y apariencias (opcional)

```css
/* Base */
body {
  margin: 0;
  font-family: sans-serif;
}

/* Layout */
.l-header {
  position: fixed;
  top: 0;
  width: 100%;
}

/* Módulo */
.nav {
  display: flex;
}

/* Estado */
.is-active {
  font-weight: bold;
}

/* Tema */
.theme-admin .nav {
  background-color: #333;
}
```

## Atomic CSS / Utility-First CSS

Enfoque basado en pequeñas clases de utilidad de propósito único (como Tailwind CSS).

```html
<div class="p-4 bg-white shadow rounded flex items-center">
  <img class="w-16 h-16 rounded-full mr-4" src="avatar.jpg">
  <div>
    <h2 class="text-xl font-bold">Título</h2>
    <p class="text-gray-600">Descripción</p>
  </div>
</div>
```

## Comparación de metodologías

| Metodología       | Enfoque                 | Complejidad | Mejor para                    |
| ----------------- | ----------------------- | ----------- | ----------------------------- |
| **BEM**           | Nomenclatura            | Media       | Proyectos de cualquier tamaño |
| **OOCSS**         | Separación de conceptos | Media       | Bibliotecas de componentes    |
| **SMACSS**        | Categorización          | Alta        | Proyectos grandes y complejos |
| **Utility-First** | Composición             | Baja        | Desarrollo rápido             |

## Elegir una metodología

La elección depende de:
- Tamaño y complejidad del proyecto
- Preferencias del equipo
- Requisitos de mantenimiento a largo plazo
- Compatibilidad con frameworks existentes

Lo más importante es mantener la consistencia dentro de un proyecto y equipo. 