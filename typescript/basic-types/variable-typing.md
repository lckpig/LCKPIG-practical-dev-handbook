<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/variable-typing)
<!-- MULTILANGUAJE MENU END -->

<!--
# Tipado en variables y constantes

- Declaración con `let`, `const` y su relación con los tipos
- Inferencia de tipos vs. anotaciones explícitas
-->

# Tipado en variables y constantes

## Declaración con `let`, `const` y su relación con los tipos

La declaración de variables y constantes en TypeScript se realiza principalmente utilizando las palabras clave `let` y `const`. La elección entre ambas no solo afecta la mutabilidad del valor almacenado, sino que también tiene implicaciones directas en el tipado y en la seguridad del código.

Cuando se utiliza `let`, se declara una variable cuyo valor puede cambiar a lo largo del tiempo. Por defecto, TypeScript infiere el tipo a partir del valor inicial, pero es posible especificar el tipo explícitamente para mayor claridad o para restringir el tipo permitido.

Por otro lado, `const` se emplea para declarar constantes, es decir, identificadores cuyo valor no puede ser reasignado después de su inicialización. Sin embargo, es importante destacar que si el valor es un objeto o un array, sus propiedades o elementos sí pueden modificarse, aunque la referencia no pueda cambiar.

El uso adecuado de `let` y `const` contribuye a la robustez del código, ya que permite al compilador detectar intentos de reasignación indebida y ayuda a los desarrolladores a expresar de forma clara la intención de inmutabilidad o mutabilidad de los datos.

### Consideraciones importantes sobre el tipado

- Cuando se declara una variable con `let` o `const` y se le asigna un valor inicial, TypeScript infiere automáticamente el tipo a partir de ese valor. Esto reduce la necesidad de anotaciones explícitas, pero puede llevar a errores si el valor inicial no representa todos los posibles valores futuros.
- Es recomendable utilizar `const` siempre que sea posible, ya que favorece la inmutabilidad y reduce la probabilidad de errores relacionados con reasignaciones accidentales.
- En el caso de objetos y arrays declarados con `const`, solo la referencia es inmutable; sus propiedades o elementos pueden seguir siendo modificables.

#### Ejemplo de declaración con `let` y `const`

```typescript
// Declaración mutable con let
let userName: string = "Alice"; // Puede ser reasignado posteriormente
userName = "Bob"; // Válido

// Declaración inmutable con const
const userAge: number = 30; // No puede ser reasignado
// userAge = 31; // Error: no se puede reasignar una constante

// Objeto declarado con const
const user = { name: "Alice", age: 30 };
user.age = 31; // Permitido: se modifica una propiedad, no la referencia
// user = { name: "Bob", age: 25 }; // Error: no se puede cambiar la referencia
```

### Casos de uso recomendados

Utilizar `const` para todas las variables cuyo valor no deba cambiar tras su inicialización es una buena práctica ampliamente aceptada. Esto facilita la lectura del código y permite a los desarrolladores identificar rápidamente qué valores son inmutables. Por ejemplo, las configuraciones, claves de acceso, rutas o cualquier valor que no deba modificarse durante la ejecución del programa deben declararse con `const`.

En cambio, `let` debe reservarse para variables cuyo valor pueda cambiar, como contadores en bucles, acumuladores o valores que dependan de condiciones dinámicas.

### Buenas y malas prácticas

- **Buena práctica:** Declarar con `const` por defecto y solo usar `let` cuando sea necesario.
- **Mala práctica:** Declarar todas las variables con `let` sin considerar la inmutabilidad, lo que puede llevar a errores difíciles de detectar.
- **Buena práctica:** Especificar el tipo explícitamente cuando la inferencia no es suficiente o puede inducir a error.
- **Mala práctica:** Modificar las propiedades de objetos declarados con `const` sin tener en cuenta las implicaciones de la mutabilidad interna.

### Errores comunes y advertencias

Uno de los errores más frecuentes es asumir que declarar un objeto con `const` lo hace completamente inmutable. En realidad, solo la referencia es inmutable, pero las propiedades internas pueden cambiar. Para lograr inmutabilidad total, es necesario utilizar técnicas adicionales como la congelación de objetos (`Object.freeze`) o el uso de librerías especializadas.

{% hint style="warning" %}
No confíes en `const` para garantizar la inmutabilidad profunda de objetos o arrays. Si necesitas estructuras realmente inmutables, considera patrones funcionales o utilidades específicas.
{% endhint %}

---

## Inferencia de tipos vs. anotaciones explícitas

TypeScript ofrece un sistema de inferencia de tipos muy potente que permite deducir el tipo de una variable a partir de su valor inicial. Esto simplifica el código y reduce la necesidad de anotaciones explícitas, pero también puede ocultar errores si no se utiliza con precaución.

La anotación explícita de tipos consiste en indicar de forma directa el tipo de la variable, lo que aporta mayor claridad y control, especialmente en situaciones donde el valor inicial no refleja todos los posibles valores futuros o cuando se desea restringir el tipo aceptado.

### Ventajas y desventajas de la inferencia y la anotación explícita

La inferencia de tipos agiliza la escritura del código y mejora la legibilidad, pero puede llevar a situaciones ambiguas si el valor inicial es demasiado genérico o si la variable debe aceptar valores de diferentes tipos en el futuro.

La anotación explícita, por su parte, aporta mayor seguridad y claridad, aunque puede resultar más verbosa. Es especialmente útil en funciones, parámetros y estructuras complejas donde la inferencia no es suficiente.

#### Ejemplo de inferencia y anotación explícita

```typescript
// Inferencia de tipo
let city = "Madrid"; // TypeScript infiere que city es string
city = "Barcelona"; // Válido
// city = 123; // Error: no se puede asignar un número a un string

// Anotación explícita de tipo
let country: string = "España";
country = "Francia"; // Válido
// country = 42; // Error: no se puede asignar un número a un string
```

### Casos de uso recomendados

- Utilizar la inferencia de tipos para variables locales y valores cuyo tipo es evidente y no cambiará.
- Emplear anotaciones explícitas en interfaces públicas, parámetros de funciones, valores retornados y estructuras complejas donde la claridad y la seguridad sean prioritarias.
- En proyectos grandes o colaborativos, es recomendable ser más explícito con los tipos para evitar ambigüedades y facilitar el mantenimiento.

### Buenas y malas prácticas

- **Buena práctica:** Aprovechar la inferencia de tipos para reducir la verbosidad, pero sin sacrificar la claridad.
- **Mala práctica:** Omitir la anotación de tipos en contextos donde la inferencia puede ser ambigua o insuficiente.
- **Buena práctica:** Documentar las decisiones de tipado, especialmente en APIs públicas o librerías.
- **Mala práctica:** Forzar la inferencia en situaciones donde la anotación explícita aportaría mayor seguridad y comprensión.

### Errores comunes y advertencias

Un error habitual es confiar excesivamente en la inferencia de tipos, lo que puede llevar a aceptar valores no deseados o a perder el control sobre la evolución del tipo de una variable. También es frecuente olvidar actualizar la anotación de tipo cuando cambian los requisitos del código.

{% hint style="info" %}
La inferencia de tipos es una herramienta poderosa, pero debe utilizarse con criterio. Evalúa siempre si la claridad y la seguridad del código se benefician más de la inferencia o de la anotación explícita en cada caso.
{% endhint %} 