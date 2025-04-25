<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/variable-typing)
<!-- MULTILANGUAJE MENU END -->
<!--
# Tipado en variables y constantes

- Declaración con `let`, `const` y su relación con los tipos
- Inferencia de tipos vs. anotaciones explícitas
-->

# Tipado en Variables y Constantes en TypeScript

TypeScript, como superset de JavaScript, introduce un sistema de tipado estático opcional que enriquece enormemente las capacidades del lenguaje base. Este sistema permite definir explícitamente los tipos de datos que esperamos que almacenen nuestras variables y constantes. La adopción del tipado estático no solo mejora la robustez del código al detectar errores en tiempo de compilación (o incluso antes, en el editor de código), sino que también incrementa significativamente la legibilidad y facilita el mantenimiento a largo plazo, especialmente en proyectos grandes y colaborativos.

Entender cómo TypeScript gestiona el tipado al declarar variables con `let` (para valores que pueden cambiar) y constantes con `const` (para valores que no cambiarán una vez asignados) es un pilar fundamental para escribir código TypeScript efectivo y seguro. Además, TypeScript ofrece dos mecanismos principales para asignar tipos: la *inferencia de tipos*, donde el compilador deduce el tipo automáticamente, y las *anotaciones explícitas*, donde el desarrollador especifica el tipo de forma manual. Dominar cuándo y cómo usar cada uno de estos mecanismos permite un control preciso sobre la seguridad y la expresividad del código.

---

## Declaración con `let`, `const` y su relación con los tipos

En el núcleo de la declaración de variables en JavaScript moderno (ES6+) y, por ende, en TypeScript, encontramos las palabras clave `let` y `const`. Mientras que `let` se utiliza para declarar variables cuyo valor puede ser reasignado posteriormente, `const` se emplea para declarar constantes, cuyo valor, una vez asignado, no puede ser cambiado a través de una nueva asignación. TypeScript potencia estas declaraciones permitiendo asociar tipos de datos específicos, añadiendo una capa de seguridad y claridad.

### `let`: Variables Mutables con Tipado

Al declarar una variable con `let`, tenemos la opción de especificar su tipo de forma explícita mediante una anotación. Si optamos por no hacerlo y proporcionamos un valor inicial, TypeScript aplicará su mecanismo de inferencia para determinar el tipo más apropiado basándose en ese valor.

#### Ejemplos de `let`

#### Ejemplo: Anotación Explícita

```typescript
let counter: number; // Declaramos 'counter' explícitamente como tipo 'number'.
counter = 10;       // Correcto: Asignamos un valor numérico.
// counter = "diez"; // Error: TypeScript detecta que intentamos asignar un string a una variable de tipo number.

let userMessage: string = "Bienvenido"; // Declaración e inicialización con tipo explícito 'string'.
userMessage = "Hola de nuevo";         // Correcto: Podemos reasignar otro string.
// userMessage = true; // Error: No se puede asignar un booleano a una variable de tipo string.
```
Este ejemplo ilustra cómo la anotación explícita define un contrato claro para la variable, y TypeScript vigila que este contrato se respete durante todo el ciclo de vida de la variable.

#### Ejemplo: Inferencia de Tipo con `let`

```typescript
let score = 0; // TypeScript infiere el tipo 'number' basado en el valor inicial 0.
score = 100;   // Correcto: Reasignamos otro número.
// score = "100"; // Error: TypeScript infiere 'number', no se permite asignar un string.

let isValid = false; // TypeScript infiere el tipo 'boolean'.
isValid = true;      // Correcto.
```
La inferencia con `let` es útil para reducir la verbosidad cuando el tipo es evidente por el valor inicial. Sin embargo, el tipo inferido es generalmente el tipo primitivo (ej. `number`, `string`, `boolean`), no un tipo literal específico.

### `const`: Constantes Inmutables (en asignación) con Tipado

Las constantes declaradas con `const` siguen una lógica similar en cuanto al tipado (explícito o inferido), pero con una restricción fundamental: su valor no puede ser reasignado después de la inicialización. Una característica interesante de `const` es que TypeScript a menudo infiere tipos más específicos, conocidos como *tipos literales*, lo que puede aumentar la precisión del sistema de tipos.

#### Ejemplos de `const`

#### Ejemplo: Anotación Explícita

```typescript
const gravity: number = 9.81; // Constante 'gravity' explícitamente de tipo 'number'.
// gravity = 10; // Error: No se puede reasignar el valor de una constante.

const defaultAdminName: string = "admin"; // Constante 'defaultAdminName' de tipo 'string'.
// defaultAdminName = "root"; // Error: Intento de reasignación inválido.
```

#### Ejemplo: Inferencia de Tipo con `const` (Tipos Literales)

```typescript
const appVersion = "1.0.0"; // TypeScript infiere el tipo literal "1.0.0", no solo 'string'.
let currentVersion: string = appVersion; // Correcto: "1.0.0" es un subtipo de 'string'.
// let specificVersion: "2.0.0" = appVersion; // Error: El tipo "1.0.0" no es asignable al tipo "2.0.0".

const maxRetries = 3; // TypeScript infiere el tipo literal 3, no solo 'number'.
let attemptCount: number = maxRetries; // Correcto: 3 es un subtipo de 'number'.
// let specificAttempt: 5 = maxRetries; // Error: El tipo 3 no es asignable al tipo 5.
```
La inferencia de tipos literales con `const` es poderosa para casos donde el valor exacto es importante, como en estados, configuraciones fijas o discriminantes en tipos unión.

### Consideraciones Importantes

-   **Inmutabilidad de Asignación vs. Inmutabilidad de Valor**: Es crucial entender que `const` solo garantiza que la *variable* no pueda ser reasignada a un nuevo valor o referencia. **No hace que el valor en sí mismo sea inmutable** si se trata de un objeto o un array. Las propiedades de un objeto `const` o los elementos de un array `const` pueden ser modificados.

    ```typescript
    // Ejemplo con objeto
    const configuration = { theme: "dark", fontSize: 14 };
    configuration.fontSize = 16; // Correcto: Modificamos una propiedad del objeto.
    // configuration = { theme: "light", fontSize: 12 }; // Error: No podemos reasignar la constante 'configuration'.

    // Ejemplo con array
    const availableSizes = ["S", "M", "L"];
    availableSizes.push("XL"); // Correcto: Modificamos el contenido del array.
    // availableSizes = ["XS", "S"]; // Error: No podemos reasignar la constante 'availableSizes'.
    ```
    Para lograr una inmutabilidad más profunda (que el contenido del objeto o array tampoco cambie), se requieren técnicas adicionales como el uso de tipos `Readonly<T>`, `ReadonlyArray<T>` o la aserción `as const`.

-   **Tipos Literales y `const`**: Como se vio, `const` sin anotación explícita tiende a inferir tipos literales. Esto significa que el tipo representa el valor exacto asignado. `let`, por otro lado, infiere tipos más generales (ej., `string` en lugar de `"valor específico"`). Esta diferencia es clave para modelar datos de forma precisa.

-   **Ámbito (Scope)**: Tanto `let` como `const` tienen ámbito de bloque (`{ ... }`), a diferencia de `var` (que debe evitarse en código moderno) que tiene ámbito de función. Esto significa que solo son accesibles dentro del bloque donde fueron declaradas.

    ```typescript
    if (true) {
      const blockScopedConstant = "Visible aquí";
      let blockScopedVariable = 10;
    }
    // console.log(blockScopedConstant); // Error: No está definida en este ámbito.
    // console.log(blockScopedVariable); // Error: No está definida en este ámbito.
    ```

### Casos de Uso Reales y Recomendables

1.  **Configuraciones Fijas**: Usa `const` para valores que no deben cambiar durante la ejecución, como claves de API, URLs base, constantes matemáticas o configuraciones de aplicación.
    ```typescript
    const API_BASE_URL: string = "https://api.example.com/v1";
    const MAX_CONNECTIONS: number = 10;
    const FEATURE_FLAG_NEW_DESIGN: boolean = true;
    ```

2.  **Variables de Bucle o Temporales**: Usa `let` para contadores en bucles `for`, variables que acumulan resultados o cualquier valor que necesite ser actualizado dentro de un bloque específico.
    ```typescript
    let totalSum = 0;
    for (let i = 0; i < 10; i++) { // 'i' necesita ser mutable
      totalSum += i; // 'totalSum' acumula valores
    }

    let requestStatus: 'pending' | 'success' | 'error' = 'pending';
    // ...lógica asíncrona...
    requestStatus = 'success'; // Actualizamos el estado
    ```

3.  **Modelado de Estados con Tipos Literales**: Aprovecha la inferencia de tipos literales de `const` para definir conjuntos fijos de valores posibles, a menudo combinados con tipos unión.
    ```typescript
    const PENDING = 'pending';
    const SUCCESS = 'success';
    const FAILED = 'failed';
    type LoadingStatus = typeof PENDING | typeof SUCCESS | typeof FAILED;

    let currentStatus: LoadingStatus = PENDING;
    // currentStatus = "loading"; // Error: "loading" no es parte de LoadingStatus
    ```

### Buenas Prácticas

-   **Favorecer `const` por defecto**: Adopta la costumbre de declarar todas las variables con `const` inicialmente. Cambia a `let` solo si descubres que necesitas reasignar la variable. Esto mejora la previsibilidad y reduce errores accidentales por reasignación.
-   **Ser explícito en las "fronteras" del código**: Aunque la inferencia es útil, sé explícito con las anotaciones de tipo en parámetros de funciones, valores de retorno y miembros públicos de clases o interfaces. Esto define contratos claros y mejora la mantenibilidad.
-   **Usar tipos literales cuando aporten semántica**: Si un valor solo puede pertenecer a un conjunto pequeño y fijo de cadenas o números, usa tipos literales (a menudo inferidos por `const`) para expresar esa restricción.
-   **Entender la inmutabilidad superficial de `const`**: No asumas que `const` protege el contenido interno de objetos y arrays. Si necesitas inmutabilidad profunda, utiliza las herramientas adecuadas (`Readonly`, `as const`, bibliotecas de inmutabilidad).

### Malas Prácticas

-   **Usar `let` cuando `const` es suficiente**: Declarar con `let` variables que nunca se reasignan añade confusión. Indica incorrectamente que el valor podría cambiar.
-   **Abusar de `let` para todo**: No diferenciar entre valores que cambian y los que no, dificulta la comprensión del flujo de datos y las intenciones del código.
-   **Ignorar el poder de los tipos literales**: No aprovechar la inferencia de tipos literales de `const` cuando podrían proporcionar un tipado más seguro y expresivo (ej. usar `let status = "active"` cuando `const status = "active"` infería `"active"`).
-   **Confundir `const` con inmutabilidad real (profunda)**: Asumir erróneamente que `const user = { name: 'Alice' }` impide cambiar `user.name`. Esto lleva a errores lógicos si se espera que el objeto sea totalmente inmutable.
-   **Usar `var`**: En código TypeScript (y JavaScript moderno), el uso de `var` está desaconsejado debido a su ámbito de función y hoisting, que pueden causar comportamientos inesperados. Siempre prefiere `let` y `const`.

---

## Inferencia de Tipos vs. Anotaciones Explícitas

TypeScript nos brinda flexibilidad a la hora de asignar tipos a nuestras variables y constantes a través de dos mecanismos principales: la inferencia automática de tipos y las anotaciones explícitas manuales. Comprender las fortalezas y debilidades de cada uno y saber cuándo aplicarlos es esencial para escribir código TypeScript claro, seguro y mantenible.

### Inferencia de Tipos: Dejando que TypeScript deduzca

La inferencia de tipos es una de las características más convenientes de TypeScript. Permite al compilador determinar automáticamente el tipo de una variable o constante basándose en el valor con el que se inicializa, sin necesidad de que el desarrollador lo escriba explícitamente. Esto reduce la verbosidad del código manteniendo la seguridad de tipos.

#### Ejemplos de Inferencia de Tipos

#### Ejemplo Básico

```typescript
let framework = "React"; // TypeScript infiere 'string'
// framework = 123; // Error: No se puede asignar un número a un tipo 'string' inferido.

const year = 2024; // TypeScript infiere 'number' (o el tipo literal 2024 si es 'const')
// year = 2025; // Error si es 'const'. Si fuera 'let', sería correcto.

const isProduction = process.env.NODE_ENV === "production"; // TypeScript infiere 'boolean'

const userInfo = { id: 101, role: "guest" };
// TypeScript infiere el tipo: { id: number; role: string; }
// userInfo.role = "admin"; // Correcto
// userInfo.id = "abc"; // Error: 'id' debe ser 'number'.
```

#### Ejemplo con `let` sin inicialización

```typescript
let responseData; // ¡Peligro! TypeScript infiere 'any' si no hay inicialización ni anotación.
responseData = { data: [1, 2, 3] }; // Correcto (porque es 'any')
responseData = "Error message";        // Correcto (porque es 'any')
responseData = 500;                    // Correcto (porque es 'any')

// El problema: perdemos toda la seguridad de tipos.
// console.log(responseData.data.length); // Puede fallar en tiempo de ejecución si responseData es un string o number.
```

{% hint style="danger" %}
Cuando una variable se declara con `let` sin un valor inicial y sin una anotación de tipo explícita (`let data;`), TypeScript, por defecto (si la opción `noImplicitAny` no está activada), inferirá el tipo `any`. **Esto es generalmente una mala práctica**, ya que desactiva por completo la comprobación de tipos para esa variable, perdiendo uno de los principales beneficios de TypeScript. Es mucho mejor ser explícito o proporcionar un valor inicial.
{% endhint %}

### Anotaciones Explícitas: Definiendo el Contrato

Las anotaciones explícitas implican declarar manualmente el tipo de una variable, constante, parámetro de función o valor de retorno utilizando la sintaxis de dos puntos seguida del tipo (`: Tipo`). Se utilizan en situaciones donde:

1.  Queremos ser absolutamente claros sobre el tipo esperado.
2.  TypeScript no puede inferir el tipo deseado (o infiere uno demasiado amplio o incorrecto).
3.  Declaramos una variable sin inicializarla.
4.  Queremos mejorar la legibilidad y el auto-documentado del código.

#### Ejemplos de Anotaciones Explícitas

#### Ejemplo: Variables y Constantes

```typescript
let userId: number | string; // Anotación explícita con un tipo unión (puede ser number o string).
userId = 123;
userId = "user-456";
// userId = true; // Error: boolean no es asignable a number | string.

const initialSettings: { theme: 'light' | 'dark'; notifications: boolean } = {
  theme: 'dark',
  notifications: true,
};
// initialSettings.theme = 'blue'; // Error: 'blue' no es 'light' ni 'dark'.

let processedData: string[] | null = null; // Puede ser un array de strings o null.
// Inicialmente es null, luego puede asignarse un array.
processedData = ["item1", "item2"];
```

#### Ejemplo: Funciones

```typescript
// Anotaciones explícitas en parámetros y valor de retorno.
function calculateDiscount(price: number, discountPercentage: number): number {
  if (discountPercentage < 0 || discountPercentage > 100) {
    throw new Error("Invalid discount percentage");
  }
  return price * (1 - discountPercentage / 100);
}

const result: number = calculateDiscount(100, 15); // result es 'number'

// Anotación para una función callback
type DataProcessor = (data: unknown) => boolean;

const processIncomingData: DataProcessor = (data) => {
  console.log("Processing:", data);
  // ...lógica de procesamiento...
  return true; // Debe retornar un boolean según DataProcessor
};
```

#### Ejemplo: Interfaces y Tipos Complejos

```typescript
interface UserProfile {
  id: string;
  displayName: string;
  email?: string; // Propiedad opcional
  lastLogin: Date;
}

let currentUser: UserProfile; // Declaración con tipo explícito usando la interfaz

currentUser = {
  id: "uuid-1234",
  displayName: "Alice",
  lastLogin: new Date(),
  // email es opcional, no necesita estar presente
};
```

### Cuándo usar Inferencia vs. Anotaciones Explícitas

La decisión entre inferencia y anotación explícita es contextual. Aquí hay una guía general:

#### Usa Inferencia de Tipos cuando:

-   **El tipo es obvio e inequívoco por el valor de inicialización**: `let name = "Bob";` (claramente `string`), `const pi = 3.14;` (claramente `number`). Añadir una anotación aquí (`let name: string = "Bob";`) es redundante y añade ruido visual.
-   **En variables locales simples dentro de funciones**: Donde el ciclo de vida es corto y el contexto es claro, la inferencia suele ser suficiente.
-   **Se busca concisión sin sacrificar la seguridad**: La inferencia reduce la cantidad de código a escribir.

#### Usa Anotaciones Explícitas cuando:

-   **Declaras una variable sin inicializarla**: `let data: MyDataType;`. Es obligatorio para evitar el tipo `any` implícito (si `noImplicitAny` está activado) o para definir el tipo correcto desde el principio.
-   **La inferencia de TypeScript no es la deseada o es demasiado amplia**:
    -   Si inicializas con `null` o `undefined` pero esperas otro tipo después: `let user: User | null = null;`
    -   Si quieres un tipo unión específico y la inicialización solo cubre un caso: `let state: 'loading' | 'success' | 'error' = 'loading';` (sin anotación, `let state = 'loading'` inferiría solo `"loading"`).
    -   Si trabajas con tipos complejos donde la inferencia podría ser ambigua o menos precisa que un tipo o interfaz definido.
-   **Defines las "fronteras" de tu código**:
    -   **Parámetros de función**: Siempre deben tener tipos explícitos. Es la forma de definir el contrato de la función.
    -   **Valores de retorno de función**: Es una muy buena práctica anotarlos explícitamente, especialmente en funciones exportadas o complejas, para claridad y para detectar errores si la función no retorna lo esperado.
    -   **Propiedades de clases, miembros de interfaces, tipos exportados**: Definen la API pública de tu código y deben ser explícitos.
-   **Quieres mejorar la legibilidad y el auto-documentado**: En código complejo o colaborativo, una anotación explícita puede aclarar inmediatamente la intención del desarrollador, incluso si técnicamente TypeScript podría inferirlo.
-   **Trabajas con bibliotecas externas o APIs**: Donde los tipos pueden no ser perfectamente inferidos o necesitas asegurar la compatibilidad con tipos específicos definidos por la biblioteca.

### Consideraciones Importantes

-   **Balance Verbosidad-Claridad**: El objetivo no es anotar todo ni no anotar nada. Busca un equilibrio. Anota donde aporte valor (claridad, precisión, definición de contrato) y deja que la inferencia actúe donde el tipo sea trivial.
-   **`noImplicitAny` es tu amigo**: Activar la opción del compilador `noImplicitAny: true` en tu `tsconfig.json` es fundamental. Obliga a TypeScript a señalar cualquier lugar donde infiera `any` porque no pudo determinar un tipo, forzándote a ser explícito y manteniendo la seguridad de tipos en todo el proyecto.
    ```json
    // tsconfig.json
    {
      "compilerOptions": {
        "strict": true, // 'strict: true' incluye 'noImplicitAny' y otras comprobaciones útiles
        // o específicamente:
        // "noImplicitAny": true,
        // ... otras opciones
      }
    }
    ```
-   **Tipado Contextual (Contextual Typing)**: TypeScript es inteligente. En ciertos contextos, como argumentos de funciones callback o asignaciones a variables ya tipadas, puede inferir tipos basándose en el contexto circundante, incluso sin un valor inicial directo.
    ```typescript
    const names = ["Alice", "Bob", "Charlie"];
    // TypeScript infiere 'name' como 'string' y 'index' como 'number'
    // basándose en la firma esperada por Array.prototype.map
    const nameLengths = names.map((name, index) => {
      console.log(`Processing index ${index}`);
      return name.length; // Correcto, 'name' es string
    });
    // nameLengths es inferido como number[]
    ```

### Errores Comunes y Malentendidos Frecuentes

1.  **Anotar tipos obvios**: Escribir `let count: number = 0;` es innecesario. `let count = 0;` es más conciso y TypeScript infiere `number` correctamente.
2.  **Depender de `any` implícito**: No activar `noImplicitAny` o ignorar sus errores lleva a código que parece TypeScript pero carece de seguridad de tipos en puntos críticos.
3.  **No anotar parámetros de función**: Es uno de los errores más comunes. Sin anotación, los parámetros suelen ser `any` implícitamente (si `noImplicitAny` está desactivado) o causan un error (si está activado). Las funciones son contratos; sus entradas deben estar claramente tipadas.
4.  **Usar `any` explícito como "solución rápida"**: Recurrir a `: any` para silenciar errores de tipo es tentador pero peligroso. Indica que no entiendes el tipo o que hay un problema subyacente. Es mejor investigar y usar el tipo correcto, `unknown` si el tipo es verdaderamente desconocido pero quieres seguridad, o refactorizar si es necesario.
5.  **Confundir inferencia de `let` y `const`**: Recordar que `let name = "admin"` infiere `string`, mientras que `const role = "admin"` infiere el tipo literal `"admin"`. Esta diferencia afecta cómo puedes usar esas variables.

{% hint style="warning" %}
El tipo `any` debe ser el último recurso. Su uso elimina las garantías que TypeScript ofrece. Si te encuentras usando `any` frecuentemente, es una señal para revisar tu modelado de tipos o buscar tipos más específicos, incluyendo `unknown` cuando necesites manejar datos de tipo desconocido de forma segura (forzando comprobaciones antes de usarlos).
{% endhint %}

### Buenas Prácticas

-   **Activa `strict: true` (o al menos `noImplicitAny: true`) en `tsconfig.json`**: Es la base para un proyecto TypeScript robusto.
-   **Confía en la inferencia para inicializaciones simples y obvias**: Reduce el ruido visual.
-   **Sé explícito en las "fronteras"**: Parámetros de función, valores de retorno, APIs públicas. Define contratos claros.
-   **Usa tipos unión explícitos cuando una variable pueda tener múltiples tipos**: `let id: number | string;`
-   **Prefiere `unknown` sobre `any`**: Si realmente no conoces el tipo de un valor pero quieres mantener la seguridad, usa `unknown`. TypeScript te obligará a realizar comprobaciones de tipo (con `typeof`, `instanceof`, aserciones de tipo, etc.) antes de poder operar con el valor.

### Malas Prácticas

-   **Anotar excesivamente tipos triviales**: `let isActive: boolean = true;` es redundante.
-   **Permitir `any` implícito**: Desactivar `noImplicitAny` o ignorar sus errores.
-   **No anotar parámetros ni retornos de función**: Deja ambiguo el contrato de la función.
-   **Abusar de `any` explícito**: Usarlo como atajo para evitar resolver problemas de tipos reales.
-   **Crear anotaciones complejas inline en lugar de usar `type` o `interface`**: Si un tipo de objeto se repite o es complejo, defínelo con un nombre usando `type` o `interface` para mejorar la reutilización y legibilidad.
    ```typescript
    // Mal: Difícil de leer y repetir
    let user: { id: number; name: string; preferences: { theme: string; lang: string } };

    // Bien: Reutilizable y legible
    type UserPreferences = { theme: string; lang: string };
    type User = { id: number; name: string; preferences: UserPreferences };
    let user: User;
    ```
