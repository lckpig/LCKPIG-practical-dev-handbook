<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/optimization/best-practices)
<!-- MULTILANGUAJE MENU END -->

# Buenas Prácticas en CSS

Seguir buenas prácticas en CSS es esencial para mantener hojas de estilo eficientes, mantenibles y con buen rendimiento. Estas recomendaciones ayudan a evitar problemas comunes y optimizar el código CSS.

## Organización y estructura

### Separación en archivos lógicos

```
styles/
  ├── base/
  │   ├── reset.css
  │   ├── typography.css
  │   └── colors.css
  ├── components/
  │   ├── buttons.css
  │   ├── forms.css
  │   └── cards.css
  ├── layout/
  │   ├── header.css
  │   ├── footer.css
  │   └── grid.css
  └── main.css
```

### Comentarios descriptivos

```css
/* ==========================================================================
   Sección: Encabezado principal
   ========================================================================== */

/* Navegación principal */
.main-nav {
  /* ... */
}
```

## Selectores eficientes

### Minimizar la especificidad

```css
/* Evitar */
body .wrapper .content article.post h2 { ... }

/* Preferir */
.post-title { ... }
```

### Evitar selectores anidados profundos

```css
/* Evitar */
.header .nav .dropdown .item .link { ... }

/* Preferir */
.nav-link { ... }
```

### Utilizar clases en lugar de selectores de ID

Los selectores de ID tienen alta especificidad y son difíciles de sobrescribir.

```css
/* Evitar */
#header { ... }

/* Preferir */
.header { ... }
```

## Propiedades y valores

### Shorthand cuando sea apropiado

```css
/* Verboso */
margin-top: 10px;
margin-right: 20px;
margin-bottom: 10px;
margin-left: 20px;

/* Conciso */
margin: 10px 20px;
```

### Evitar !important

```css
/* Evitar */
.elemento {
  color: red !important;
}

/* Preferir mejores selectores o mayor especificidad cuando sea necesario */
```

### Unidades coherentes

Mantener coherencia en las unidades de medida utilizadas:

- `rem` para tamaños tipográficos y espaciado general
- `em` para espaciado relacionado con el texto
- `%` para anchos relativos 
- `px` solo para bordes y tamaños muy pequeños

## Rendimiento

### Evitar selectores universales costosos

```css
/* Evitar como selector principal */
* { ... }
[class^="col-"] { ... }

/* Limitar el alcance */
.container * { ... }
```

### Limitar el uso de propiedades costosas

```css
/* Propiedades que pueden afectar el rendimiento */
box-shadow
text-shadow
opacity
transform
filter
position: fixed
```

### Simplificar selectores

```css
/* Evitar */
button:not([disabled]):not(.btn--secondary) { ... }

/* Preferir */
.btn--primary { ... }
```

## Mantenibilidad

### Seguir una convención de nomenclatura

Ejemplos: BEM, SMACSS, OOCSS

```css
/* Ejemplo BEM */
.block__element--modifier { ... }
```

### Mantener la consistencia

- Mantener un orden coherente para las propiedades
- Usar el mismo formato (indentación, espacios)
- Aplicar las mismas convenciones en todo el proyecto

### Evitar CSS altamente específico para componentes

Para facilitar la reutilización de componentes en diferentes contextos.

## Compatibilidad

### Usar prefijos solo cuando sea necesario

```css
/* Usar herramientas automatizadas como Autoprefixer */
display: flex;
```

### Probar en múltiples navegadores

Verificar el CSS en los principales navegadores y dispositivos.

Seguir estas buenas prácticas ayudará a crear código CSS más limpio, eficiente y mantenible, facilitando el trabajo en equipo y mejorando el rendimiento general de los sitios web. 