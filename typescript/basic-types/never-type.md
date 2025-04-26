<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/never-type)
<!-- MULTILANGUAJE MENU END -->

<!--
# El tipo `never` para funciones que no devuelven valores

- Funciones que arrojan errores (`throw`)
- Funciones que nunca terminan (`while (true) {}`)
-->

<details id="toc-container">
<summary>Índice de contenidos</summary>

<!-- no toc -->
- [Funciones que arrojan errores (`throw`)](#funciones-que-arrojan-errores-throw)
  - [Definición y utilidad](#definicion-y-utilidad)
  - [Casos de uso reales y recomendables](#casos-de-uso-reales-y-recomendables)
  - [Consideraciones importantes](#consideraciones-importantes)
  - [Errores comunes y trampas habituales](#errores-comunes-y-trampas-habituales)
  - [Buenas prácticas](#buenas-practicas)
  - [Malas prácticas](#malas-practicas)
- [Funciones que nunca terminan (`while (true) {}`)](#funciones-que-nunca-terminan-while-true)
  - [Definición y utilidad](#definicion-y-utilidad-1)
  - [Casos de uso reales y recomendables](#casos-de-uso-reales-y-recomendables-1)
  - [Consideraciones importantes](#consideraciones-importantes-1)
  - [Errores comunes y trampas habituales](#errores-comunes-y-trampas-habituales-1)
  - [Buenas prácticas](#buenas-practicas-1)
  - [Malas prácticas](#malas-practicas-1)

</details>

# El tipo `never` para funciones que no devuelven valores

El tipo `never` en TypeScript es un tipo especial que representa valores que **nunca** pueden ocurrir. Se diferencia fundamentalmente de `void`, que indica la ausencia *intencional* de un valor de retorno (la función termina pero no devuelve nada), mientras que `never` significa que la función **nunca alcanza su punto final** de manera normal.

Este escenario se materializa principalmente de dos formas:
1.  La función siempre lanza una excepción (`throw`).
2.  La función entra en un bucle infinito que nunca termina (ej: `while (true) {}`).

Comprender `never` es crucial para el **análisis estático del flujo de control** que realiza TypeScript. Permite al compilador razonar sobre la alcanzabilidad del código, identificar estados imposibles y validar comprobaciones exhaustivas, llevando a tipos más precisos y código más seguro, especialmente en patrones avanzados como tipos condicionales, guardas de tipo y manejo robusto de errores.

[↑ Volver al Índice](#toc-container)


## Funciones que arrojan errores (`throw`)

Una aplicación primordial y frecuente de `never` es en funciones diseñadas exclusivamente para señalar un error y detener la ejecución abruptamente mediante `throw`. Cuando TypeScript puede determinar estáticamente que una función *siempre* terminará lanzando una excepción, infiere (o se le puede anotar explícitamente) que su tipo de retorno es `never`.

### Definición y utilidad

Lanzar una excepción es el mecanismo estándar para señalar condiciones de error graves o irrecuperables que impiden la continuación normal del programa. Al asignar `never` al tipo de retorno de una función que siempre lanza un error, TypeScript formaliza esta garantía: cualquier código que siga inmediatamente a la invocación de dicha función dentro del mismo bloque lógico se considera **código inalcanzable**.

Esta información es valiosa para el compilador, ya que:
*   Permite detectar código muerto o lógicamente inaccesible.
*   Refuerza la validez de las comprobaciones exhaustivas en estructuras como `switch` o `if/else if/else`.
*   Ayuda a prevenir errores sutiles al asegurar que ciertas rutas de código, bajo condiciones específicas (como un estado inválido detectado), nunca se ejecuten.

#### Ejemplo: Función que siempre lanza un error

```typescript
// Esta función está diseñada únicamente para lanzar un error.
// Su tipo de retorno es 'never' porque nunca devuelve un valor normalmente.
function fail(message: string): never {
  throw new Error(message);
}

// Ejemplo de uso en validación de entrada
function processData(data: string | number | boolean): void {
  if (typeof data === 'string') {
    console.log(`Processing string: "${data.toUpperCase()}"`);
  } else if (typeof data === 'number') {
    console.log(`Processing number: ${data.toFixed(2)}`);
  } else if (typeof data === 'boolean') {
    console.log(`Processing boolean: ${data ? 'Yes' : 'No'}`);
  } else {
    // ¡Esta rama es teóricamente imposible!
    // Hemos cubierto todos los casos de la unión (string | number | boolean).
    // TypeScript infiere que 'data' aquí tiene el tipo 'never'.
    // Llamar a 'fail' es la forma correcta de manejar este estado imposible.
    fail(`Unexpected data type encountered: ${typeof data}`);
    // console.log('Esto nunca se ejecutará'); // Código inalcanzable
  }
}

try {
  processData("example");
  processData(123.456);
  processData(false);
  // Si pasáramos un tipo no contemplado (aunque el tipado lo impide en TS),
  // la rama 'else' se ejecutaría, llamando a 'fail' y lanzando el error.
} catch (error: any) {
  console.error(`Caught an error: ${error.message}`);
}
```
En `processData`, la rama `else` final sólo se alcanzaría si `data` no fuera ni `string`, ni `number`, ni `boolean`. Dado el tipado `string | number | boolean`, esta situación es lógicamente imposible. TypeScript reconoce esto e infiere `data` como `never` en esa rama. Usar `fail` aquí no solo maneja el caso imposible sino que también satisface al compilador, ya que `never` es asignable a cualquier tipo (incluido el `void` implícito de `processData`).

[↑ Volver al Índice](#toc-container)


### Casos de uso reales y recomendables

#### Validación estricta y fallos críticos

Ideal para funciones de validación que comprueban precondiciones esenciales para la ejecución. Si una validación crítica falla (ej: configuración faltante, estado inconsistente irrecuperable), una función `never` puede detener la ejecución inmediatamente, previniendo operaciones en un estado inválido.

```typescript
interface ServerConfig {
  port: number;
  host?: string; // Opcional
  apiKey: string; // Requerida
}

function assertConfigIsValid(config: Partial<ServerConfig>): asserts config is ServerConfig {
  if (config.port === undefined || config.port <= 0 || config.port > 65535) {
    fail(`Invalid configuration: Port must be between 1 and 65535. Received: ${config.port}`);
  }
  if (config.apiKey === undefined || config.apiKey.trim() === '') {
    fail("Invalid configuration: API Key is required and cannot be empty.");
  }
  // Si llega aquí, la configuración tiene las propiedades requeridas (port, apiKey)
  // 'asserts config is ServerConfig' actúa como una guarda de tipo.
}

const initialConfig: Partial<ServerConfig> = { port: 8080 /* apiKey falta */ };

try {
  assertConfigIsValid(initialConfig);
  // Si no hubo error, TypeScript sabe que 'initialConfig' es ahora 'ServerConfig'
  console.log(`Configuration validated. Starting server on port ${initialConfig.port} with key ${initialConfig.apiKey}.`);
} catch (error: any) {
  console.error(`Application startup failed: ${error.message}`);
  // Podríamos terminar el proceso aquí: process.exit(1);
}

```
Aquí, `assertConfigIsValid` utiliza `fail` (que retorna `never`) para detenerse ante una configuración inválida. El uso de `asserts config is ServerConfig` es una característica avanzada de TypeScript que informa al compilador que si la función no lanza un error, el tipo del objeto `config` se refina a `ServerConfig`.

#### Comprobaciones exhaustivas (`Exhaustiveness Checks`)

Una técnica fundamental en TypeScript, especialmente con uniones discriminadas o `enum`. Se usa `never` en la rama `default` de un `switch` o en la rama `else` final de un `if/else if` para garantizar que todos los casos posibles han sido explícitamente manejados. Si se añade un nuevo miembro a la unión/enum y no se actualiza el `switch`/`if`, el compilador detectará un error porque el valor en la rama `default`/`else` ya no será asignable a `never`.

```typescript
type Result<T, E> = { success: true; value: T } | { success: false; error: E };

function processResult<T, E>(result: Result<T, E>): string {
  if (result.success) {
    // TypeScript sabe que 'result' aquí es { success: true; value: T }
    return `Operation succeeded with value: ${result.value}`;
  } else {
    // TypeScript sabe que 'result' aquí es { success: false; error: E }
    return `Operation failed with error: ${result.error}`;
  }
  // Si hubiera una rama 'else' adicional aquí, 'result' sería 'never'.
  // Ejemplo con switch:
  // switch (result.success) {
  //   case true: return `Success: ${result.value}`;
  //   case false: return `Error: ${result.error}`;
  //   default:
  //     const exhaustiveCheck: never = result; // Error si 'result' no es 'never'
  //     return fail(`Unhandled result state: ${exhaustiveCheck}`);
  // }
}

const successResult: Result<string, Error> = { success: true, value: "Data fetched" };
const errorResult: Result<string, Error> = { success: false, error: new Error("Network timeout") };

console.log(processResult(successResult));
console.log(processResult(errorResult));
```

{% hint style="info" %}
La comprobación exhaustiva mediante `never` es una red de seguridad crucial. Transforma errores lógicos (olvidar manejar un caso) en errores de compilación, facilitando el mantenimiento y la refactorización segura al añadir nuevas variantes a los tipos.
{% endhint %}

[↑ Volver al Índice](#toc-container)


### Consideraciones importantes

*   **Interrupción del Flujo:** La principal consecuencia de llamar a una función `never` es la detención inmediata del flujo de ejecución normal. Asegúrate de que este comportamiento sea intencional y adecuado para la situación (errores críticos, estados imposibles).
*   **Manejo de Errores vs. Excepciones:** Las funciones `never` que lanzan errores son parte del mecanismo de excepciones. Deben usarse para señalar *fallos* (bugs, estados irrecuperables). Para errores *operacionales* esperados (entrada inválida del usuario, recurso no encontrado), prefiere devolver valores como `null`, `undefined`, un objeto `Error` específico, o usar `Promise.reject`, permitiendo un manejo más controlado por parte del código cliente.
*   **Tipado Explícito:** Aunque TypeScript a menudo infiere `never`, anotarlo explícitamente (`function fail(): never { ... }`) mejora la legibilidad y documenta la intención de manera clara.
*   **Depuración:** Cuando una función `never` lanza un error, la traza de la pila (stack trace) apuntará al lugar donde se lanzó la excepción. Esto facilita la depuración de errores críticos.

[↑ Volver al Índice](#toc-container)


### Errores comunes y trampas habituales

*   **Confundir `never` con `void`:** `void` significa que la función retorna sin valor, pero *retorna*. `never` significa que *nunca retorna* (lanza error o bucle infinito). Usar `void` donde se necesita `never` (o viceversa) puede llevar a lógica incorrecta o errores no detectados por el compilador.

{% hint style="warning" %}
**No uses `throw` para control de flujo normal:** Las excepciones y las funciones `never` son para errores excepcionales o irrecuperables, no para lógica de negocio habitual como validaciones simples o decisiones condicionales. Abusar de `throw` hace el código más difícil de seguir y depurar.
{% endhint %}

*   **Código inalcanzable no detectado:** Si una función *podría* lanzar un error pero no *siempre* lo hace, su tipo no será `never`. Si asumes erróneamente que es `never`, podrías tener código inalcanzable que el compilador no detecta.
*   **Olvidar el `catch`:** Si una función `never` que lanza error se llama fuera de un bloque `try...catch`, detendrá la ejecución global (en Node.js, el proceso; en el navegador, el script) si el error no se captura en un nivel superior.

[↑ Volver al Índice](#toc-container)


### Buenas prácticas

*   **Reservar `never` (con `throw`) para errores irrecuperables:** Utilízalo para fallos de aserción, errores de configuración críticos, o para marcar estados que lógicamente nunca deberían alcanzarse.
*   **Implementar comprobaciones exhaustivas:** Es uno de los usos más valiosos de `never`. Asegúrate de que tus `switch` y `if/else` sobre uniones discriminadas terminen con una rama `default`/`else` que asigne a `never` o llame a una función `never`.
*   **Documentar la razón:** Si una función retorna `never`, explica claramente en su JSDoc por qué (ej: `@returns {never} Siempre lanza una excepción si...`).
*   **Crear funciones de utilidad:** Define funciones auxiliares reutilizables como `fail(message: string): never` o `assertUnreachable(x: never): never` para centralizar la lógica de error irrecuperable.

[↑ Volver al Índice](#toc-container)


### Malas prácticas

*   **Manejar errores esperados con `throw`/`never`:** No lo uses para validación de formularios, fallos de red previsibles, etc. Usa tipos de retorno más informativos (`Result<T, E>`, `Promise<T>`, `T | null`).
*   **Lanzar errores genéricos:** En lugar de `throw new Error("Error")`, lanza errores específicos (`throw new ConfigurationError(...)`) para facilitar la captura y manejo diferenciado.

{% hint style="warning" %}
**Evita código tras llamada `never`:** No coloques código ejecutable después de invocar una función `never` en el mismo bloque lógico. Es confuso y siempre será inalcanzable.
{% endhint %}

*   **Suprimir errores de comprobación exhaustiva:** No ignores los errores del compilador en las ramas `default`/`else` con `never`. Indican que falta manejar un caso.

[↑ Volver al Índice](#toc-container)


---

## Funciones que nunca terminan (`while (true) {}`)

La segunda vía para que una función tenga el tipo `never` es mediante un bucle infinito del cual no hay salida posible (como `while (true)` sin `break`, `return` o `throw` dentro). Este tipo de función nunca completa su ejecución y, por lo tanto, nunca devuelve el control al punto de llamada.

### Definición y utilidad

En ciertos contextos específicos, como la implementación de servidores, procesos demonio (daemons), o bucles principales de eventos (game loops, ciertos frameworks reactivos), una función puede estar diseñada para ejecutarse indefinidamente. TypeScript reconoce esta estructura de bucle infinito y, si puede determinar que no hay ruta de escape, infiere el tipo de retorno `never`.

Esto indica que la función está destinada a **monopolizar el hilo de ejecución** (si es síncrona) o a representar un **proceso persistente**.

#### Ejemplo: Función con bucle infinito

```typescript
// Función diseñada para ejecutarse indefinidamente, procesando eventos.
function eventLoop(): never {
  console.log("Event loop started. Waiting for events...");
  while (true) {
    // 1. Esperar por un evento (simulado con un delay)
    // En un caso real, esto podría ser I/O, timers, etc.
    const startTime = Date.now();
    while(Date.now() - startTime < 2000) { /* Simular espera activa */ }

    // 2. Procesar el evento (simulado)
    const eventData = `Event at ${new Date().toLocaleTimeString()}`;
    console.log(`Processing: ${eventData}`);

    // El bucle continúa indefinidamente, nunca hay un 'return' o 'break'.
  }
  // Este código es inalcanzable
  // console.log("Event loop finished.");
}

// Llamar a eventLoop() bloquearía el hilo actual.
// No lo ejecutamos directamente aquí para no bloquear la explicación.
// try {
//   eventLoop();
// } catch (error) {
//   // Este catch no se activaría a menos que el bucle lanzara un error.
//   console.error("Event loop crashed:", error);
// }
// console.log("This line would never be reached if eventLoop() was called.");
```
La función `eventLoop` contiene un `while (true)` sin ninguna condición de salida. Por lo tanto, TypeScript infiere `never` como su tipo de retorno.

[↑ Volver al Índice](#toc-container)


### Casos de uso reales y recomendables

#### Puntos de entrada de Servidores/Demonios

La función principal que inicia un servidor HTTP, un worker de colas de mensajes, o cualquier proceso destinado a ejecutarse continuamente hasta que sea terminado externamente.

```typescript
import http from 'http'; // Ejemplo con módulo Node.js

function startHttpServer(port: number): never {
  const server = http.createServer((req, res) => {
    console.log(`Received request: ${req.method} ${req.url}`);
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello from the never-ending server!\n');
  });

  server.listen(port, () => {
    console.log(`Server listening on http://localhost:${port}`);
    // Aunque 'listen' retorna, la función 'startHttpServer' entra
    // en un estado de espera "infinito" conceptualmente.
    // Para hacerla explícitamente 'never', podríamos añadir un bucle.
  });

  // Para que sea rigurosamente 'never', podríamos añadir un bucle post-listen,
  // aunque en la práctica de Node.js no suele ser necesario así.
  while(true) {
     // Mantener el proceso vivo explícitamente (aunque listen ya lo hace)
     // Podría hacer comprobaciones de estado, etc.
     const start = Date.now();
     while(Date.now() - start < 60000) { /* Espera 1 minuto */ }
     console.log("Server heartbeat...");
  }
  // Nota: En Node.js real, el 'listen' por sí solo mantiene el proceso vivo.
  // El bucle while(true) aquí es más ilustrativo del concepto 'never'.
}

// startHttpServer(3000); // Iniciaría el servidor y nunca retornaría.
```

#### Bucles de juego o renderizado continuo

En aplicaciones gráficas o juegos, un bucle principal puede encargarse de leer entradas, actualizar el estado y renderizar la pantalla repetidamente.

```typescript
function gameEngineLoop(): never {
  let frameCount = 0;
  console.log("Game Engine Initialized. Starting loop...");
  while (true) {
    const startTime = performance.now();

    // 1. Handle Input (e.g., keyboard, mouse)
    // const input = readGameInput();

    // 2. Update Game State
    // updateWorld(input, frameCount);

    // 3. Render Graphics
    // renderScene();

    const endTime = performance.now();
    const elapsedTime = endTime - startTime;
    console.log(`Frame ${frameCount++}: Rendered in ${elapsedTime.toFixed(2)} ms`);

    // Control de FPS (ejemplo simple, no muy preciso)
    const targetFrameTime = 1000 / 60; // 60 FPS
    const waitTime = Math.max(0, targetFrameTime - elapsedTime);
    const waitStart = Date.now();
    while(Date.now() - waitStart < waitTime) { /* Esperar */ }
  }
}
```

[↑ Volver al Índice](#toc-container)


### Consideraciones importantes

{% hint style="danger" %}
**¡CRÍTICO! Bloqueo del Hilo Principal con Bucles Síncronos:** Un bucle `while(true)` síncrono en una función `never` **congela** el hilo de ejecución. En Node.js, esto detiene el bucle de eventos, impidiendo manejar nuevas peticiones o I/O. En el navegador, congela la UI. **Nunca uses bucles infinitos síncronos bloqueantes en el hilo principal.**
{% endhint %}

{% hint style="info" %}
**Prioriza Alternativas Asíncronas:** Para servidores, workers o tareas de larga duración, los modelos asíncronos (`async/await`, `Promises`, `EventEmitter`, Web Workers) son la norma en JS/TS. Permiten la concurrencia y evitan el bloqueo del hilo principal. Una función `never` bloqueante es raramente la solución adecuada.
{% endhint %}

*   **Intencionalidad:** Asegúrate de que el bucle infinito sea verdaderamente intencional y la única forma adecuada de modelar el comportamiento (lo cual es raro para código síncrono bloqueante).
*   **Terminación:** Una función `never` de este tipo solo termina si el proceso es detenido externamente (ej: `Ctrl+C`, `kill`) o si ocurre un error no capturado *dentro* del bucle que lo haga terminar.

{% hint style="warning" %}
**Evita el `Busy-Waiting`:** Un bucle `while(true)` sin pausas (I/O, `setTimeout`, `await delay`) consumirá el 100% de CPU. Esto es ineficiente y perjudicial. Siempre introduce mecanismos de espera apropiados.
{% endhint %}

*   **Confusión con procesos asíncronos:** No confundas una función `never` síncrona bloqueante con un proceso asíncrono de larga duración (como un servidor Node.js típico) que *no* bloquea el hilo.

[↑ Volver al Índice](#toc-container)


### Buenas prácticas

*   **Limitar a puntos de entrada:** Usa funciones `never` con bucles infinitos síncronos *solo* en el punto de entrada principal de un proceso diseñado específicamente para ejecutarse de esa manera (y preferiblemente no en el hilo principal si la concurrencia es necesaria).

{% hint style="success" %}
**Favorece los Modelos Asíncronos:** Para casi todas las tareas persistentes o de larga duración en JS/TS (servidores, workers, listeners), usa `async/await`, `Promises`, `EventEmitter` o Web Workers. Son más eficientes y no bloquean.
{% endhint %}

*   **Incluir manejo de errores interno:** Si el bucle debe continuar a pesar de errores internos, implementa `try...catch` dentro del bucle.
*   **Introducir mecanismos de espera:** Evita el `busy-waiting`. Usa I/O asíncrono, `setTimeout`, `setInterval`, o mecanismos de espera apropiados para liberar el hilo y reducir el consumo de CPU.
*   **Documentar:** Explica por qué la función nunca retorna y sus implicaciones (ej: `@returns {never} Entra en un bucle infinito para procesar tareas. Bloquea el hilo.` ).

[↑ Volver al Índice](#toc-container)


### Malas prácticas

{% hint style="danger" %}
**NUNCA Bloquees el Hilo Principal:** los bucles `while(true)` síncronos son extremadamente peligrosos en el hilo principal de Node.js o del navegador. Es la principal mala práctica a evitar con `never` y bucles infinitos.
{% endhint %}

*   **`Busy-waiting`:** Bucles infinitos que consumen CPU sin realizar trabajo útil o esperar eficientemente.
*   **No manejar errores dentro del bucle:** Permitir que errores internos detengan un proceso que debería ser resiliente.
*   **Usarlo cuando un bucle normal con condición de salida es más apropiado:** No fuerces un `never` si el proceso tiene una condición de finalización natural.

[↑ Volver al Índice](#toc-container)

