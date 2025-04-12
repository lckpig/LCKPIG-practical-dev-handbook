<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/fundamentals/selectors-and-specificity)
<!-- MULTILANGUAJE MENU END -->

# Selectors and Specificity in CSS

CSS selectors are patterns used to select the HTML elements to which style rules will be applied. Specificity is the mechanism browsers use to determine which styles to apply when multiple selectors match the same element.

## Basic Selector Types

- **Element selector**: `p`, `div`, `h1`
- **Class selector**: `.class`
- **ID selector**: `#identifier`
- **Universal selector**: `*`
- **Attribute selector**: `[attribute=value]`

## Combinators

- **Descendant**: `div p`
- **Direct child**: `div > p`
- **Adjacent sibling**: `h1 + p`
- **General sibling**: `h1 ~ p`

## Pseudo-classes and Pseudo-elements

- **Pseudo-classes**: `:hover`, `:active`, `:focus`, `:nth-child()`
- **Pseudo-elements**: `::before`, `::after`, `::first-letter`

## Specificity

Specificity determines which CSS rule takes precedence when multiple rules apply to the same element:

1. Inline styles (`style="..."`)
2. IDs (#id)
3. Classes (.class), attributes, and pseudo-classes
4. Elements and pseudo-elements

Understanding selectors and specificity is crucial for writing efficient CSS and avoiding conflicts between style rules. 