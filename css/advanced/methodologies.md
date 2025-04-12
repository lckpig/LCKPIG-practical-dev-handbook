<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/advanced/methodologies)
<!-- MULTILANGUAJE MENU END -->

# CSS Methodologies

CSS methodologies are sets of rules and conventions that help write more maintainable, scalable, and organized CSS, especially in large projects.

## BEM (Block, Element, Modifier)

BEM is a naming convention that divides the interface into independent blocks.

### Naming Structure

- **Block**: Self-contained component (`.card`)
- **Element**: Part of a block (`.card__title`)
- **Modifier**: Variant or state (`.card--featured`, `.button--disabled`)

```css
/* Block */
.card {
  background: white;
  border-radius: 4px;
}

/* Element */
.card__title {
  font-size: 1.5rem;
}

/* Element */
.card__image {
  width: 100%;
}

/* Modifier */
.card--featured {
  border: 2px solid gold;
}
```

### BEM Advantages

- Avoids name collisions
- Clarifies the relationship between HTML and CSS
- Facilitates component reuse
- Provides a modular structure

## OOCSS (Object-Oriented CSS)

OOCSS is based on the separation of structure and appearance, and the independence of container and content.

### Principles

1. **Separation of structure and skin**
   - Define structures (width, margin, padding) separately from appearances (color, border, background)

2. **Separation of container and content**
   - Elements should not depend on their location

```css
/* Structure (object) */
.media {
  display: flex;
  align-items: flex-start;
}

.media__body {
  flex: 1;
}

/* Skin (theme) */
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

SMACSS categorizes CSS rules into five different types.

### Categories

1. **Base**
   - Default styles for HTML elements (reset/normalize)

2. **Layout**
   - Main page divisions (header, footer, main)

3. **Module**
   - Reusable and standalone components

4. **State**
   - Describes how modules look in particular states

5. **Theme**
   - Defines colors and appearances (optional)

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

/* Module */
.nav {
  display: flex;
}

/* State */
.is-active {
  font-weight: bold;
}

/* Theme */
.theme-admin .nav {
  background-color: #333;
}
```

## Atomic CSS / Utility-First CSS

An approach based on small, single-purpose utility classes (like Tailwind CSS).

```html
<div class="p-4 bg-white shadow rounded flex items-center">
  <img class="w-16 h-16 rounded-full mr-4" src="avatar.jpg">
  <div>
    <h2 class="text-xl font-bold">Title</h2>
    <p class="text-gray-600">Description</p>
  </div>
</div>
```

## Methodology Comparison

| Methodology       | Focus              | Complexity | Best for                |
| ----------------- | ------------------ | ---------- | ----------------------- |
| **BEM**           | Naming             | Medium     | Projects of any size    |
| **OOCSS**         | Concept separation | Medium     | Component libraries     |
| **SMACSS**        | Categorization     | High       | Large, complex projects |
| **Utility-First** | Composition        | Low        | Rapid development       |

## Choosing a Methodology

The choice depends on:
- Project size and complexity
- Team preferences
- Long-term maintenance requirements
- Compatibility with existing frameworks

The most important thing is to maintain consistency within a project and team. 