<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/transformations/animations-and-transitions)
<!-- MULTILANGUAJE MENU END -->

# Animations and Transitions in CSS

CSS offers powerful tools for creating fluid animations and transitions without JavaScript, improving user experience and interface interactivity.

## Transitions

Transitions provide a way to control animation speed when changing CSS properties.

### Basic Syntax

```css
.element {
  transition-property: property-to-animate;
  transition-duration: time-duration;
  transition-timing-function: timing-curve;
  transition-delay: delay-time;
  
  /* Shorthand */
  transition: property-to-animate time-duration timing-curve delay-time;
}
```

### Example

```css
.button {
  background-color: blue;
  color: white;
  padding: 10px 20px;
  transition: background-color 0.3s ease-in-out;
}

.button:hover {
  background-color: darkblue;
}
```

### Timing Functions

* `ease`: Default, slow start, then fast, then slow end
* `linear`: Constant speed
* `ease-in`: Slow start
* `ease-out`: Slow end
* `ease-in-out`: Slow start and end
* `cubic-bezier(n,n,n,n)`: Custom curve

## Animations

Animations allow more complex effects with keyframes defining intermediate steps.

### Basic Syntax

```css
@keyframes animation-name {
  0% {
    /* Start state properties */
  }
  50% {
    /* Intermediate state */
  }
  100% {
    /* End state properties */
  }
}

.element {
  animation-name: animation-name;
  animation-duration: time-duration;
  animation-timing-function: timing-curve;
  animation-delay: delay-time;
  animation-iteration-count: number | infinite;
  animation-direction: normal | reverse | alternate | alternate-reverse;
  animation-fill-mode: none | forwards | backwards | both;
  animation-play-state: running | paused;
  
  /* Shorthand */
  animation: animation-name duration timing-function delay iteration-count direction fill-mode;
}
```

### Example

```css
@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-20px);
  }
}

.ball {
  width: 50px;
  height: 50px;
  background-color: red;
  border-radius: 50%;
  animation: bounce 1s ease infinite;
}
```

## Performance Considerations

For smooth animations:

* **Prefer animating transforms and opacity**: They don't trigger layout recalculations
* **Use `will-change`**: Hints the browser about properties that will change
* **Be cautious with animations on mobile**: They can impact battery life
* **Test performance**: Use browser dev tools to identify jank

```css
.optimized-animation {
  will-change: transform, opacity;
  transform: translateZ(0); /* Hardware acceleration */
}
```

## Advanced Techniques

### Multi-step Animations

```css
@keyframes rainbow {
  0% { background-color: red; }
  14% { background-color: orange; }
  28% { background-color: yellow; }
  42% { background-color: green; }
  56% { background-color: blue; }
  70% { background-color: indigo; }
  84% { background-color: violet; }
  100% { background-color: red; }
}
```

### Animation Sequencing

```css
.sequence-element-1 {
  animation: fadeIn 1s ease forwards;
}

.sequence-element-2 {
  animation: fadeIn 1s ease 0.5s forwards;
}

.sequence-element-3 {
  animation: fadeIn 1s ease 1s forwards;
}
```

### State Transitions with CSS Variables

```css
:root {
  --menu-height: 0;
}

.menu {
  height: var(--menu-height);
  transition: height 0.3s ease;
}

.menu.open {
  --menu-height: 200px;
}
```

CSS animations and transitions provide a powerful way to create engaging, performant user interfaces that respond to user interactions in a natural and intuitive way. 