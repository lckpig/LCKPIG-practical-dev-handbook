<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/never-type)
<!-- MULTILANGUAJE MENU END -->

<!-- 
# El tipo `never` para funciones que no devuelven valores

- Funciones que arrojan errores (`throw`)
- Funciones que nunca terminan (`while (true) {}`)
 -->

# El tipo `never` en TypeScript: Más allá de `void`

El tipo `never` en TypeScript es un concepto fundamental, aunque a veces esquivo, que representa la ausencia total de valor. A diferencia de `void`, que indica la ausencia de un valor *retornado* útil (pero la función sí completa su ejecución, retornando `undefined`), `never` significa que una expresión o función **nunca** llega a producir un valor o completar su ejecución de forma normal.

Su utilidad principal radica en dos áreas:

1.  **Tipado de funciones que nunca retornan**: Señala funciones que garantizan la interrupción del flujo de ejecución, ya sea lanzando una excepción o entrando en un bucle infinito.
2.  **Análisis de flujo de control (Control Flow Analysis - CFA)**: Ayuda a TypeScript a razonar sobre ramas de código inalcanzables, mejorando la seguridad y la precisión del tipado.

Comprender `never` es crucial para escribir código TypeScript más robusto y expresivo, especialmente al manejar errores y lógica condicional compleja.

---

## Funciones que garantizan la interrupción (`throw`)

Este es uno de los usos más directos de `never`. Cuando una función está diseñada específicamente para terminar la ejecución lanzando una excepción, su tipo de retorno adecuado es `never`.

### Definición y utilidad profunda

Declarar una función con retorno `never` no es solo una anotación; es un contrato con el compilador y otros desarrolladores. Indica que cualquier código que siga inmediatamente a una llamada a esta función dentro del mismo bloque *no se ejecutará* si la excepción no es capturada y manejada en un nivel superior. TypeScript utiliza esta información para refinar su análisis de flujo de control.

Considera una función de validación crítica:

#### Ejemplo: Validación estricta que detiene el flujo

```typescript
// Función de utilidad para lanzar errores específicos de la aplicación
function fail(message: string): never {
  throw new Error(`[Validation Error] ${message}`);
}

// Validador para asegurar que una cadena no esté vacía
function ensureNonEmpty(value: string | null | undefined, fieldName: string): string {
  if (value === null || value === undefined || value.trim() === '') {
    // Si la validación falla, llamamos a fail, que retorna never.
    // TypeScript sabe que después de esta llamada, la función no puede continuar
    // en esta ruta, por lo que no se necesita un 'return' aquí.
    fail(`El campo '${fieldName}' no puede estar vacío.`);
  }
  // Si la validación pasa, TypeScript sabe que 'value' no puede ser null ni undefined
  // y que es una cadena no vacía. El tipo se refina a 'string'.
  return value; 
}

// Uso
try {
  const username = ensureNonEmpty(prompt("Introduce tu nombre de usuario:"), "username");
  console.log(`Bienvenido, ${username.toUpperCase()}!`); // username es 'string' aquí
} catch (error) {
  console.error(error.message); 
  // El flujo continúa aquí si se captura la excepción
}

// Otro ejemplo: processInput mejorado
function processInputImproved(input: string | number | boolean): void {
  if (typeof input === 'string') {
    console.log(`Processing string: ${input.toUpperCase()}`);
  } else if (typeof input === 'number') {
    console.log(`Processing number: ${input.toFixed(2)}`);
  } else {
    // En esta rama, 'input' es de tipo 'boolean'.
    // Si quisiéramos asegurar que boolean no es válido, podríamos hacer esto:
    assertUnreachable(input); // Llama a una función que retorna 'never'
  }
}

// Función auxiliar para comprobaciones de exhaustividad
function assertUnreachable(x: never): never {
    // Si esta función se ejecuta, significa que 'x' no era 'never',
    // lo cual indica un error en la lógica del programa.
    throw new Error(`Caso inesperado alcanzado: ${JSON.stringify(x)}`);
}

// processInputImproved("hello"); // OK
// processInputImproved(123.456); // OK
// processInputImproved(true); // Lanza Error: Caso inesperado alcanzado: true
```

### Casos de uso reales y recomendables

Más allá de la validación simple, `never` brilla en:

1.  **Comprobaciones de exhaustividad (Exhaustiveness Checks)**: En bloques `switch` o cadenas `if-else if` que manejan uniones discriminadas (discriminated unions), una rama `default` o `else` final puede usar una función que acepte `never` para asegurar que todos los casos posibles de la unión han sido contemplados. Si se añade un nuevo miembro a la unión sin actualizar el `switch`, TypeScript generará un error en la rama `never`, detectando el problema en tiempo de compilación.

    #### Ejemplo: Comprobación de exhaustividad con uniones discriminadas

    ```typescript
    type Shape = 
      | { kind: "circle"; radius: number }
      | { kind: "square"; sideLength: number }
      | { kind: "triangle"; base: number; height: number }; // Nuevo miembro añadido

    function getArea(shape: Shape): number {
      switch (shape.kind) {
        case "circle":
          return Math.PI * shape.radius ** 2;
        case "square":
          return shape.sideLength ** 2;
        // Falta el caso "triangle"
        default:
          // 'shape' aquí tendría el tipo { kind: "triangle"; ... }
          // Si intentamos pasarlo a assertUnreachable, TypeScript dará un error
          // porque { kind: "triangle"; ... } no es asignable a 'never'.
          // Esto nos alerta de que no hemos manejado todos los casos.
          return assertUnreachable(shape); 
      }
    }
    
    // Función auxiliar (la misma que antes)
    function assertUnreachable(x: never): never {
      throw new Error("Caso no manejado en la declaración switch.");
    }

    // Si compilamos esto, TypeScript mostrará un error en la línea 'return assertUnreachable(shape);'
    // indicando que el tipo 'Shape' (específicamente el caso 'triangle') no es asignable a 'never'.
    ```

2.  **Funciones de utilidad para errores centralizados**: Crear funciones como `fail`, `panic`, `throwInvalidArgument` que encapsulan la lógica de lanzamiento de errores específicos, mejorando la consistencia y legibilidad.

3.  **API de bibliotecas**: Algunas bibliotecas pueden exponer funciones que garantizan la terminación (por ejemplo, una función `exitProcess` en un entorno de servidor).

### Consideraciones importantes

-   **`never` vs `void`**: La distinción es crucial. Una función `void` completa su ejecución (retornando `undefined`), mientras que una función `never` *nunca* la completa normalmente. Usar `void` donde `never` es apropiado puede ocultar errores de lógica o hacer que el análisis de flujo de control de TypeScript sea menos preciso.

-   **Captura de excepciones (`try...catch`)**: El hecho de que una función retorne `never` debido a un `throw` no significa que el programa se detenga irrevocablemente. Si la excepción es capturada, el flujo puede continuar en el bloque `catch`. `never` describe el flujo *dentro* de la función y su salida normal, no el comportamiento global del programa ante excepciones.

    ```typescript
    function mightThrow(): never {
        console.log("Intentando...");
        throw new Error("¡Algo salió mal!");
        // Código inalcanzable aquí
        // console.log("Esto nunca se ejecuta"); 
    }

    console.log("Antes de llamar a mightThrow");
    try {
        mightThrow(); 
        // TypeScript sabe que esta línea es inalcanzable si mightThrow no es capturada antes
        console.log("Después de llamar a mightThrow (inalcanzable si no hay catch)"); 
    } catch (e) {
        console.log(`Excepción capturada: ${e.message}`); // El flujo continúa aquí
    }
    console.log("Después del bloque try...catch"); // Esta línea se ejecuta
    ```
-   **Inferencia de tipo**: TypeScript a menudo puede inferir `never` correctamente, pero anotarlo explícitamente mejora la claridad y la intención del código.

### Buenas prácticas

-   **Anotar explícitamente `: never`**: Mejora la intención y la legibilidad, aunque TypeScript pueda inferirlo.
-   **Usar para comprobaciones de exhaustividad**: Es una de las aplicaciones más potentes de `never` para prevenir errores en tiempo de compilación.
-   **Centralizar el lanzamiento de errores**: Utilizar funciones auxiliares que retornen `never` para manejar errores de forma consistente.

### Malas prácticas

-   **Usar `never` incorrectamente**: Anotar una función como `never` si existe *alguna* ruta de código que permita un retorno normal (incluso `return undefined;`). En esos casos, `void` u otro tipo sería más apropiado.
-   **Depender implícitamente del lanzamiento**: No usar `never` cuando una función *siempre* lanza un error. Esto reduce la precisión del análisis de flujo de control.
-   **Suprimir errores sin lógica de manejo**: Capturar una excepción de una función `never` con un bloque `catch` vacío o que simplemente ignora el error. Esto anula el propósito de señalar una condición excepcional.

---

## Funciones que nunca terminan (Bucles infinitos)

El segundo escenario clave para `never` implica funciones que, por diseño, nunca completan su ejecución porque contienen un bucle infinito (p.ej., `while (true) {}`) o alguna forma de recursión que no tiene caso base de salida.

### Definición y utilidad profunda

Estas funciones se encuentran típicamente en procesos de larga duración o servicios que necesitan ejecutarse continuamente. El tipo `never` comunica que la llamada a esta función será el último punto de ejecución en esa secuencia de código; ninguna instrucción que le siga será alcanzada.

#### Ejemplo: Servidor simple o worker

```typescript
function runServer(): never {
  console.log("Iniciando el servidor...");
  let connectionCount = 0;
  // Bucle principal del servidor (simplificado)
  while (true) {
    // Simula la espera de una nueva conexión
    const hasNewConnection = Math.random() > 0.8; // Simulación
    if (hasNewConnection) {
      connectionCount++;
      console.log(`Nueva conexión recibida. Total: ${connectionCount}`);
      // Procesar la conexión... (lógica omitida)
    }
    // Pequeña pausa para no consumir CPU al 100%
    // En un servidor real, esto sería manejo de eventos asíncronos (no un bucle busy-wait)
    // sleep(100); // Función hipotética de pausa
  }
  // Esta parte es inalcanzable debido al while(true)
  // console.log("Servidor deteniéndose..."); 
}

// Función auxiliar hipotética
// function sleep(ms: number): Promise<void> {
//   return new Promise(resolve => setTimeout(resolve, ms));
// }

// Punto de entrada principal
console.log("Aplicación iniciada.");
// runServer(); // Si descomentamos esto, el programa se quedaría aquí indefinidamente.

// TypeScript sabe que esta línea es inalcanzable si runServer() se llama antes.
console.log("Aplicación terminada."); 
```

### Casos de uso reales y recomendables

1.  **Servicios de fondo (Daemons/Workers)**: Procesos que escuchan eventos, procesan colas de tareas o mantienen conexiones abiertas indefinidamente.
2.  **Bucles principales de aplicaciones**: En juegos, simulaciones o algunas aplicaciones de interfaz de usuario (aunque menos común en web debido al modelo de eventos), un bucle principal puede ejecutarse continuamente.
3.  **Puntos de entrada de procesos persistentes**: El punto de inicio de un script diseñado para nunca terminar.

### Consideraciones importantes

{% hint style="danger" %}
**¡Cuidado con el bloqueo!** Un bucle infinito síncrono en el hilo principal congela la aplicación.
{% endhint %}

En entornos de un solo hilo como JavaScript (navegadores, Node.js por defecto), una función `never` basada en un bucle infinito **bloqueará completamente** ese hilo. Esto significa que ninguna otra tarea (como responder a interacciones del usuario, manejar eventos de red, o ejecutar `setTimeout`/`setInterval`) podrá ejecutarse en ese hilo. Esto es generalmente **inaceptable** para el hilo principal de aplicaciones interactivas. Los bucles infinitos síncronos deben usarse con extrema precaución y generalmente relegarse a workers dedicados si es necesario.

-   **Alternativas asíncronas**: En Node.js y navegadores modernos, los servicios de larga duración se implementan típicamente usando operaciones asíncronas (p.ej., `async/await`, callbacks, listeners de eventos) en lugar de bucles `while(true)` síncronos, para evitar el bloqueo del hilo. Una función `async function` que nunca resuelve su `Promise` *no* tiene tipo de retorno `never` (su tipo de retorno es `Promise<never>`, que es diferente).
-   **Intención clara**: El uso de un bucle infinito debe ser deliberado y bien justificado por la naturaleza de la tarea (p.ej., un proceso que *debe* estar siempre activo).

### Buenas prácticas

-   **Reservar para puntos de entrada**: Usar funciones `never` de bucle infinito principalmente como la función principal de un script o proceso diseñado para ejecución continua.
-   **Utilizar Workers para tareas intensivas/bloqueantes**: Si se necesita un bucle computacionalmente intensivo o potencialmente bloqueante en una aplicación interactiva, moverlo a un Web Worker (navegador) o Worker Thread (Node.js).
-   **Documentar el comportamiento**: Explicar claramente por qué la función está diseñada para nunca terminar y sus implicaciones (como el bloqueo del hilo).

### Malas prácticas

-   **Bucles infinitos accidentales**: Crear un `while(true)` sin una lógica de `break` o `return` alcanzable por error.
-   **Bloquear el hilo principal innecesariamente**: Usar un bucle `never` síncrono en el hilo principal de una aplicación web o de escritorio, congelando la interfaz de usuario.
-   **Consumo de recursos**: No considerar el impacto en CPU y memoria de un bucle que se ejecuta continuamente sin pausas o manejo de eventos adecuado.
-   **Confundir con `async`**: Asumir que una función `async` que espera indefinidamente retorna `never`. Retorna `Promise<never>`.

---

## `never` en el Análisis de Flujo de Control

Más allá de las funciones, `never` juega un papel sutil pero importante en cómo TypeScript analiza el código. Cuando el compilador determina que una rama de código es inalcanzable, puede asignar el tipo `never` a las variables dentro de esa rama.

### Tipos Unión y Estrechamiento (Narrowing)

Cuando se trabaja con tipos unión, TypeScript estrecha (narrows) el tipo de una variable a medida que se realizan comprobaciones. Si todas las posibilidades de la unión se descartan en un punto específico, el tipo de la variable se convierte en `never`.

#### Ejemplo: Estrechamiento a `never`

```typescript
function processValue(value: string | number) {
  if (typeof value === 'string') {
    console.log(value.toUpperCase());
  } else if (typeof value === 'number') {
    console.log(value.toFixed(2));
  } else {
    // En esta rama, TypeScript ha descartado 'string' y 'number'.
    // Como no hay otras posibilidades en la unión 'string | number',
    // el tipo de 'value' aquí es 'never'.
    console.log("Esto nunca debería suceder.");
    // value.algo // Error: 'value' es de tipo 'never', no tiene propiedades.
    assertUnreachable(value); // Podemos usar nuestra función auxiliar aquí también.
  }
}
```

### Tipos Condicionales

`never` también es fundamental en tipos condicionales avanzados para filtrar miembros de uniones o mapear tipos.

#### Ejemplo: Filtrando propiedades con `never`

```typescript
// Tipo condicional para extraer solo las claves de tipo string de un objeto
type StringKeys<T> = { 
  [K in keyof T]: T[K] extends string ? K : never 
}[keyof T];

interface UserProfile {
  id: number;
  username: string;
  email: string;
  lastLogin: Date;
}

// StringKeys<UserProfile> se resuelve a:
// {
//   id: never;
//   username: "username";
//   email: "email";
//   lastLogin: never;
// }["id" | "username" | "email" | "lastLogin"]
//
// Esto evalúa a: never | "username" | "email" | never
// Que simplifica a: "username" | "email"

type ProfileStringKeys = StringKeys<UserProfile>; // Tipo resultante: "username" | "email"

const key: ProfileStringKeys = "email"; // OK
// const invalidKey: ProfileStringKeys = "id"; // Error: Type 'number' is not assignable to type '"username" | "email"'.
```

En este ejemplo, cuando `T[K]` no es `string`, el tipo condicional resuelve a `never`. Al indexar el tipo mapeado con `keyof T`, los miembros `never` son eliminados de la unión resultante, dejando solo las claves deseadas.

### Implicaciones

-   **Seguridad de tipos**: El uso de `never` en el CFA ayuda a detectar código muerto o lógica imposible en tiempo de compilación.
-   **Precisión del tipado**: Permite a TypeScript entender con mayor precisión qué tipos son posibles en cada punto del código.

---

## Errores Comunes y Malentendidos

1.  **Confundir `never` con `void`**: El error más común. Recuerda: `void` = la función retorna, pero sin valor útil; `never` = la función *no* retorna normalmente.
2.  **Confundir `never` con `unknown`**: `unknown` representa un valor cuyo tipo no se conoce todavía, pero *existe*. `never` representa la *ausencia* de un valor. Una variable `unknown` debe ser estrechada antes de usarse; una variable `never` no puede tener un valor (excepto, teóricamente, `never` mismo, lo cual es imposible de instanciar directamente).
3.  **Asumir que `throw` siempre significa `never` en el contexto global**: Una función que lanza puede tener `never` como tipo de retorno, pero si la excepción se captura, el programa continúa. `never` aplica al flujo *interno* de la función.
4.  **Olvidar la comprobación de exhaustividad**: No usar `never` en la rama `default` o `else` final al manejar uniones puede llevar a errores silenciosos si la unión se extiende más tarde.
5.  **Crear bucles `never` bloqueantes sin darse cuenta**: Especialmente peligroso en el hilo principal de aplicaciones interactivas.

{% hint style="info" %}
**Subtipo Universal:** 'never' es subtipo de todos los tipos, pero nada (excepto 'never') es asignable a 'never'.
{% endhint %}

- `never` es el subtipo de *todos* los demás tipos en TypeScript. Esto significa que un valor de tipo `never` puede ser asignado a una variable de cualquier otro tipo (aunque no puedes crear un valor `never` directamente).
- Ningún otro tipo (excepto `never` mismo) es asignable a `never`. Esto es lo que hace que las comprobaciones de exhaustividad funcionen: si intentas asignar algo que no es `never` a una variable o parámetro `never`, TypeScript dará un error.

Comprender `never` y su papel en el sistema de tipos y el análisis de flujo de control de TypeScript es un paso importante para dominar el lenguaje y escribir código más seguro y expresivo.
