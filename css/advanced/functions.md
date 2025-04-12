<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/advanced/functions)
<!-- MULTILANGUAJE MENU END -->

# Funciones en CSS

Las funciones CSS permiten realizar cálculos, transformaciones y operaciones dinámicas directamente en las hojas de estilo, haciendo que CSS sea más potente y flexible.

## Función calc()

La función `calc()` permite realizar cálculos dinámicos, mezclando diferentes unidades.

```css
.elemento {
  /* Resta un margen fijo de un ancho porcentual */
  width: calc(100% - 40px);
  
  /* Combina diferentes unidades */
  margin-top: calc(2rem + 5vh);
  
  /* Operaciones más complejas */
  height: calc((100vh - 10rem) / 2);
}
```

Operadores soportados: `+`, `-`, `*`, `/`

## Funciones de color

### rgb() y rgba()

```css
.elemento {
  color: rgb(255, 0, 0);            /* Rojo */
  background-color: rgba(0, 0, 255, 0.5); /* Azul semi-transparente */
}
```

### hsl() y hsla()

```css
.elemento {
  /* Matiz (0-360), Saturación (%), Luminosidad (%) */
  color: hsl(120, 100%, 50%);            /* Verde */
  background-color: hsla(240, 100%, 50%, 0.5); /* Azul semi-transparente */
}
```

### Nuevas notaciones

```css
/* Notación moderna para rgb/rgba */
color: rgb(255 0 0 / 80%);

/* Funciones de color con valores en canales alfa */
color: hsl(0deg 100% 50% / 80%);
```

## Funciones de selección

### min(), max() y clamp()

```css
.elemento {
  /* Usa el valor más pequeño */
  width: min(1000px, 100%);
  
  /* Usa el valor más grande */
  font-size: max(1rem, 2vw);
  
  /* Limita un valor entre un mínimo y un máximo */
  padding: clamp(1rem, 5vw, 3rem);
}
```

## Funciones de gradiente

```css
.boton {
  /* Gradiente lineal */
  background: linear-gradient(to right, red, blue);
  
  /* Gradiente radial */
  background: radial-gradient(circle, yellow, orange);
  
  /* Gradiente cónico */
  background: conic-gradient(from 45deg, red, blue, green, yellow);
  
  /* Gradiente repetido */
  background: repeating-linear-gradient(45deg, 
                red, red 10px, 
                blue 10px, blue 20px);
}
```

## Funciones de filtro

```css
.imagen {
  filter: brightness(150%) contrast(1.2);
  backdrop-filter: blur(5px);
}
```

## Función var()

```css
.elemento {
  color: var(--color-primario, blue);
}
```

## Funciones de formas (shapes)

```css
.elemento {
  clip-path: circle(50%);
  shape-outside: circle(50%);
}
```

## Funciones attr()

Accede a atributos HTML:

```css
[data-tooltip]:hover::after {
  content: attr(data-tooltip);
}
```

## Funciones de tiempo

```css
.elemento {
  transition-timing-function: cubic-bezier(0.42, 0, 0.58, 1);
}
```

Las funciones CSS agregan capacidades programáticas a las hojas de estilo, permitiendo crear diseños más dinámicos, adaptables y expresivos sin depender de JavaScript o preprocesadores. 