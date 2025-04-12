<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/transformations/animations-and-transitions)
<!-- MULTILANGUAJE MENU END -->

# Animaciones y Transiciones en CSS

CSS ofrece dos mecanismos principales para animar elementos: transiciones y animaciones. Ambos permiten crear efectos dinámicos sin necesidad de JavaScript.

## Transiciones CSS

Las transiciones permiten cambiar suavemente las propiedades de un elemento durante un período determinado.

```css
.boton {
  background-color: blue;
  color: white;
  padding: 10px 20px;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.boton:hover {
  background-color: darkblue;
  transform: scale(1.05);
}
```

### Propiedades de transición

- `transition-property`: Propiedades a animar
- `transition-duration`: Duración de la transición
- `transition-timing-function`: Curva de aceleración
- `transition-delay`: Retraso antes de iniciar
- `transition`: Atajo para todas las propiedades

## Animaciones CSS

Las animaciones permiten crear secuencias más complejas con múltiples estados intermedios.

```css
@keyframes rebote {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-20px);
  }
  100% {
    transform: translateY(0);
  }
}

.pelota {
  width: 50px;
  height: 50px;
  background-color: red;
  border-radius: 50%;
  animation: rebote 1s ease infinite;
}
```

### Propiedades de animación

- `animation-name`: Nombre de la animación (@keyframes)
- `animation-duration`: Duración total
- `animation-timing-function`: Curva de aceleración
- `animation-delay`: Retraso antes de iniciar
- `animation-iteration-count`: Número de repeticiones
- `animation-direction`: Dirección (normal, reverse, alternate)
- `animation-fill-mode`: Estado antes/después (forwards, backwards, both)
- `animation-play-state`: Reproduciendo o pausado
- `animation`: Atajo para todas las propiedades

## ¿Cuándo usar cada una?

- **Transiciones**: Para cambios simples entre dos estados (hover, focus, etc.)
- **Animaciones**: Para efectos más complejos, independientes o que necesitan repetirse

## Optimización de rendimiento

- Animar propiedades que no causan repintado (transform, opacity)
- Usar `will-change` para preparar el navegador
- Evitar animar muchos elementos simultáneamente
- Considerar `@media (prefers-reduced-motion)` para accesibilidad

Las animaciones y transiciones CSS son herramientas poderosas que, usadas correctamente, pueden mejorar significativamente la experiencia del usuario sin comprometer el rendimiento. 