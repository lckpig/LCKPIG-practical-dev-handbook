<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/fundamentals/cascade-and-inheritance)
<!-- MULTILANGUAJE MENU END -->

# Cascade and Inheritance in CSS

Cascade and inheritance are two fundamental concepts that determine how styles are applied in CSS.

## Cascade

The cascade is the algorithm that determines which CSS rules are applied when there are multiple rules affecting the same element. The order of priority is:

1. **Importance**: Declarations with `!important`
2. **Specificity**: According to selector type (inline > id > class > element)
3. **Order of appearance**: The last defined rule prevails

```css
p { color: blue; }        /* Lower priority */
.text { color: green; }   /* Higher priority */
```

## Inheritance

Inheritance is the mechanism by which some CSS properties are automatically passed from parent elements to their descendants.

### Properties That Inherit

- `color`
- `font-family`, `font-size`
- `text-align`
- `line-height`

### Properties That Don't Inherit

- `margin`, `padding`
- `border`
- `background`
- `height`, `width`

## Controlling Inheritance

CSS provides special values to control inheritance:

- `inherit`: Forces inheritance from the parent element
- `initial`: Sets the initial value defined by the CSS specification
- `unset`: Combines `inherit` and `initial` depending on the property
- `revert`: Reverts to the browser style

Understanding cascade and inheritance is essential for predicting how styles will be applied and resolving conflicts between CSS rules. 