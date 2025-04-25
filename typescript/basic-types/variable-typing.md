<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/variable-typing)
<!-- MULTILANGUAJE MENU END -->
<!--
# Tipado en variables y constantes

- Declaración con `let`, `const` y su relación con los tipos
- Inferencia de tipos vs. anotaciones explícitas
-->

# Tipado en Variables y Constantes en TypeScript

TypeScript introduce un sistema de tipado estático sobre JavaScript, lo que significa que podemos definir explícitamente los tipos de datos que nuestras variables y constantes almacenarán. Esto mejora la robustez del código, facilita la detección temprana de errores y mejora la legibilidad y el mantenimiento.

Comprender cómo TypeScript maneja el tipado en la declaración de variables (`let`) y constantes (`const`) es fundamental para aprovechar al máximo sus capacidades. Además, TypeScript ofrece mecanismos como la inferencia de tipos y las anotaciones explícitas, que nos permiten elegir el nivel de detalle que queremos en nuestras declaraciones.

---

## Declaración con `let`, `const` y su relación con los tipos

En JavaScript moderno (y por extensión, en TypeScript), utilizamos `let` para declarar variables cuyo valor puede cambiar y `const` para declarar constantes cuyo valor no cambiará una vez asignado. TypeScript extiende esta funcionalidad permitiendo asociar tipos específicos a estas declaraciones.

### `let`: Variables con Tipado

Cuando declaras una variable con `let`, puedes especificar su tipo explícitamente. Si no lo haces, TypeScript intentará inferir el tipo basándose en el valor inicial asignado.

#### Ejemplo de `let` con anotación explícita

```typescript
let age: number; // Declaramos una variable 'age' de tipo 'number'
age = 30;       // Correcto: asignamos un número
// age = "treinta"; // Error: No se puede asignar un string a un tipo 'number'

let username: string = "lckpig"; // Declaración e inicialización con tipo explícito
username = "john.doe"; // Correcto: podemos reasignar otro string
// username = 123; // Error: No se puede asignar un número a un tipo 'string'
```

### `const`: Constantes con Tipado

Las constantes declaradas con `const` también pueden tener tipos explícitos o inferidos. La principal diferencia es que su valor no puede ser reasignado después de la inicialización. Además, TypeScript puede inferir tipos más específicos para las constantes.

#### Ejemplo de `const` con anotación explícita

```typescript
const pi: number = 3.14159; // Constante de tipo 'number'
// pi = 3.14; // Error: No se puede reasignar una constante

const appName: string = "MyApp"; // Constante de tipo 'string'
// appName = "AnotherApp"; // Error: No se puede reasignar
```

### Consideraciones Importantes

-   **Inmutabilidad con `const`**: Usar `const` por defecto mejora la predictibilidad del código. Declara una variable con `let` solo si sabes que necesitarás reasignar su valor.
-   **Tipos más específicos con `const`**: Cuando TypeScript infiere el tipo de una constante, a menudo utiliza un *tipo literal*. Por ejemplo, si `const city = "Madrid"`, el tipo inferido no es `string`, sino `"Madrid"` (un tipo literal string). Esto puede ser útil para un tipado más preciso.
    ```typescript
    const city = "Madrid"; // Tipo inferido: "Madrid"
    let currentCity: string = city; // Correcto: "Madrid" es asignable a 'string'
    // let anotherCity: "Barcelona" = city; // Error: "Madrid" no es asignable a "Barcelona"
    ```
-   **`const` con objetos y arrays**: Es importante recordar que `const` previene la reasignación de la variable, pero no hace que el valor en sí sea inmutable si es un objeto o un array. Las propiedades del objeto o los elementos del array pueden modificarse.
    ```typescript
    const person = { name: "Alice", age: 25 };
    person.age = 26; // Correcto: Modificar una propiedad del objeto
    // person = { name: "Bob", age: 30 }; // Error: Reasignar la constante

    const numbers = [1, 2, 3];
    numbers.push(4); // Correcto: Modificar el array
    // numbers = [5, 6]; // Error: Reasignar la constante
    ```
    Para lograr inmutabilidad en objetos y arrays, se necesitan técnicas adicionales como `Readonly` o `as const`.

### Buenas Prácticas

-   **Prefiere `const` sobre `let`**: Usa `const` siempre que sea posible para indicar que una variable no será reasignada. Esto hace el código más fácil de seguir y menos propenso a errores por reasignaciones inesperadas.
-   **Sé explícito cuando sea necesario**: Aunque la inferencia de tipos es potente, añade anotaciones de tipo explícitas si mejora la claridad o si TypeScript no puede inferir el tipo deseado (por ejemplo, en parámetros de función sin valor por defecto).

### Malas Prácticas

-   **Usar `let` innecesariamente**: Declarar con `let` variables que nunca se reasignan puede llevar a confusión sobre la intención del código.
-   **Ignorar los tipos literales de `const`**: No aprovechar los tipos literales inferidos por `const` cuando podrían proporcionar un tipado más seguro y expresivo.
-   **Confundir `const` con inmutabilidad profunda**: Asumir que `const` hace inmutables los contenidos de objetos o arrays.

---

## Inferencia de Tipos vs. Anotaciones Explícitas

TypeScript ofrece dos maneras principales de asignar tipos a las variables y constantes: la inferencia de tipos y las anotaciones explícitas. La elección entre una y otra depende del contexto y de la claridad que se quiera lograr.

### Inferencia de Tipos

La inferencia de tipos es el proceso mediante el cual TypeScript determina automáticamente el tipo de una variable basándose en el valor con el que se inicializa. Es una característica muy conveniente que reduce la verbosidad del código.

#### Ejemplo de Inferencia de Tipos

```typescript
let message = "Hola Mundo"; // TypeScript infiere 'string'
// message = 100; // Error: 'message' es de tipo 'string'

const count = 0; // TypeScript infiere 'number' (tipo primitivo)
// count = 1; // Error: 'count' es una constante

const isActive = true; // TypeScript infiere 'boolean'

const user = { id: 1, name: "Admin" }; // TypeScript infiere { id: number; name: string; }

let data; // TypeScript infiere 'any' porque no hay inicialización ni anotación
data = 10; // Correcto
data = "texto"; // Correcto
data = false; // Correcto
```

{% hint style="info" %}
Cuando una variable se declara sin inicialización ni tipo explícito (`let data;`), TypeScript infiere el tipo `any`. Esto desactiva la comprobación de tipos para esa variable, lo cual suele ser desaconsejable. Es mejor ser explícito o inicializarla.
{% endhint %}

### Anotaciones Explícitas

Las anotaciones explícitas consisten en declarar manualmente el tipo de una variable o constante usando la sintaxis `: Type`. Se utilizan cuando queremos ser específicos sobre el tipo, cuando TypeScript no puede inferir el tipo deseado, o para mejorar la legibilidad del código.

#### Ejemplo de Anotaciones Explícitas

```typescript
let score: number; // Anotación explícita, sin inicialización
score = 100;

let email: string | null = null; // Anotación explícita con un tipo unión
email = "test@example.com";

// Útil en parámetros de función y valores de retorno
function greet(name: string): string {
  return `Hello, ${name}!`;
}

// Cuando se trabaja con objetos complejos o interfaces
interface Product {
  id: number;
  name: string;
  price?: number; // Propiedad opcional
}

let product: Product;
product = { id: 1, name: "Laptop" }; // 'price' es opcional
```

### Cuándo usar cada una

#### Usa Inferencia de Tipos cuando:

-   El tipo es obvio por el valor de inicialización (`let name = "Alice";`).
-   Se busca reducir la verbosidad sin sacrificar la seguridad de tipos.
-   En variables locales simples donde el contexto es claro.

#### Usa Anotaciones Explícitas cuando:

-   **Declaras una variable sin inicializarla**: `let value: number;`
-   **La inferencia no es suficiente o es demasiado amplia**: Por ejemplo, si quieres un tipo unión y la inicialización solo cubre uno de los casos: `let status: 'active' | 'inactive' = 'active';`
-   **Defines parámetros de función y tipos de retorno**: Es esencial para la claridad y el contrato de la función: `function add(a: number, b: number): number { return a + b; }`
-   **Trabajas con estructuras de datos complejas (objetos, arrays)** donde quieres asegurar una forma específica: `let users: { id: number; name: string }[] = [];`
-   **Quieres mejorar la legibilidad**: A veces, una anotación explícita, aunque redundante, puede hacer el código más fácil de entender a primera vista, especialmente en código complejo o APIs públicas.

### Consideraciones Importantes

-   **Balance entre verbosidad y claridad**: Demasiadas anotaciones explícitas pueden hacer el código verboso (`let name: string = "Bob";` es a menudo redundante). Muy pocas pueden ocultar la intención o llevar a tipos `any` implícitos.
-   **Tipos `any` implícitos**: Si TypeScript no puede inferir un tipo (por ejemplo, una variable sin inicialización ni anotación) y la opción de compilador `noImplicitAny` está desactivada, inferirá `any`. Es una buena práctica activar `noImplicitAny` en la configuración de TypeScript (`tsconfig.json`) para evitar esto.
    ```json
    // tsconfig.json
    {
      "compilerOptions": {
        "noImplicitAny": true
        // ... otras opciones
      }
    }
    ```
-   **Contextual Typing**: En ciertos contextos, como en callbacks o asignaciones, TypeScript puede inferir tipos basándose en la ubicación de la variable o función.
    ```typescript
    const numbers = [1, 2, 3];
    // TypeScript infiere que 'n' es de tipo 'number' por el contexto del array
    numbers.forEach(n => {
      console.log(n.toFixed(2));
    });
    ```

### Buenas Prácticas

-   **Activa `noImplicitAny`**: Es fundamental para asegurar un tipado robusto en todo el proyecto.
-   **Confía en la inferencia para tipos simples**: Deja que TypeScript infiera tipos para inicializaciones obvias (strings, numbers, booleans).
-   **Sé explícito en las "fronteras"**: Usa anotaciones explícitas en parámetros de función, tipos de retorno y propiedades de objetos exportados o complejos para definir contratos claros.
-   **Usa tipos unión explícitos**: Si una variable puede tener múltiples tipos, decláralo explícitamente: `let result: number | string;`

### Malas Prácticas

-   **Anotar tipos obvios**: Escribir `let name: string = "Alice";` añade ruido visual sin aportar valor sobre `let name = "Alice";`.
-   **Depender de `any` implícito**: Permitir que TypeScript infiera `any` desactiva la comprobación de tipos y reduce los beneficios de usar TypeScript.
-   **No anotar parámetros de función**: Dejar los parámetros sin tipo explícito a menudo resulta en `any` implícito si `noImplicitAny` está desactivado, o en un error si está activado.
-   **Usar `any` explícito innecesariamente**: Recurrir a `: any` como solución rápida en lugar de definir el tipo correcto o usar `unknown` cuando la seguridad de tipos es importante.

{% hint style="warning" %}
El uso excesivo de `any` (implícito o explícito) anula muchas de las ventajas de TypeScript. Debe evitarse siempre que sea posible, prefiriendo tipos más específicos o el tipo `unknown` cuando el tipo exacto no se conoce de antemano pero se quiere mantener la seguridad.
{% endhint %}
