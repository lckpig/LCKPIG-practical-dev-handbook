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
- [Introducción: El Tipo `never`](#introduccion-el-tipo-never)
  - [Diferencia clave: `never` vs `void`](#diferencia-clave-never-vs-void)
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

## Introducción: El Tipo `never`

El tipo `never` en TypeScript es un tipo especial que representa valores que **nunca** pueden ocurrir o situaciones de las que una función **nunca** retorna de forma normal. Se utiliza para indicar que una función siempre lanza una excepción o entra en un bucle infinito.

Comprender `never` es crucial para el **análisis estático del flujo de control** que realiza TypeScript. Permite al compilador razonar sobre la alcanzabilidad del código, identificar estados imposibles y validar comprobaciones exhaustivas, llevando a tipos más precisos y código más seguro, especialmente en patrones avanzados como tipos condicionales, guardas de tipo y manejo robusto de errores.

### Diferencia clave: `never` vs `void`

Es fundamental distinguir `never` de `void`. Mientras ambos indican la ausencia de un valor de retorno *útil*, su significado es diferente:

| Característica    | `never`                                                                     | `void`                                                    |
| :---------------- | :-------------------------------------------------------------------------- | :-------------------------------------------------------- |
| **Significado**   | La función **nunca** retorna normalmente.                                   | La función **retorna** pero sin un valor explícito.       |
| **Finalización**  | Termina abruptamente (error) o no termina.                                  | Termina normalmente.                                      |
| **Valor**         | Representa la ausencia de *cualquier* valor.                                | Representa la ausencia de un valor *de retorno*.          |
| **Asignabilidad** | Asignable a **cualquier** tipo.                                             | Asignable solo a `any`, `unknown` y `void`.               |
| **Uso Típico**    | Funciones que lanzan errores, bucles infinitos, comprobaciones exhaustivas. | Funciones sin `return`, callbacks que no devuelven valor. |

Esta distinción es vital para el análisis de flujo de control del compilador.

[↑ Volver al Índice](#toc-container)

---

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
  // Lanza una excepción, deteniendo la ejecución normal.
  throw new Error(message);
  // Cualquier código aquí sería inalcanzable.
}

// Ejemplo de uso en validación de entrada para una unión de tipos.
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
    // 'never' es asignable a cualquier tipo, incluido 'void' (retorno implícito).
    fail(`Unexpected data type encountered: ${typeof data}`);
    // console.log('Esto nunca se ejecutará'); // Código inalcanzable detectado por TS
  }
}

try {
  processData("example");
  processData(123.456);
  processData(false);
  // Si, hipotéticamente, pasáramos un tipo no contemplado (p.ej., un objeto si el tipo fuera any),
  // la rama 'else' se ejecutaría, llamando a 'fail' y lanzando el error.
  // processData({ kind: 'unexpected' } as any); // Descomentar para probar el error
} catch (error: any) {
  // El error lanzado por 'fail' sería capturado aquí.
  console.error(`Caught an error: ${error.message}`);
}
```
En el ejemplo `processData`, la rama `else` final sólo se alcanzaría si `data` no fuera ni `string`, ni `number`, ni `boolean`. Dada la definición del tipo `string | number | boolean`, esta situación es lógicamente imposible bajo el sistema de tipos de TypeScript. El compilador reconoce esto e infiere que el tipo de `data` dentro de esa rama es `never`. Llamar a `fail` aquí no solo maneja semánticamente el caso imposible, sino que también satisface al compilador, ya que una función que retorna `never` puede ser utilizada en cualquier lugar donde se espere un retorno, incluso `void`, porque garantiza que ese punto de retorno nunca se alcanzará.

[↑ Volver al Índice](#toc-container)

### Casos de uso reales y recomendables

#### Validación estricta y fallos críticos

Ideal para funciones de validación que comprueban precondiciones esenciales para la ejecución. Si una validación crítica falla (ej: configuración faltante, estado inconsistente irrecuperable), una función `never` puede detener la ejecución inmediatamente, previniendo operaciones en un estado inválido. Esto es común en la inicialización de aplicaciones o módulos.

#### Ejemplo: Validación de configuración al inicio

```typescript
interface ServerConfig {
  port: number;
  host?: string; // Opcional, puede tener un valor por defecto
  apiKey: string; // Requerida para la funcionalidad principal
  databaseUrl: string; // Requerida
}

// Función auxiliar que siempre lanza un error, retornando 'never'
function configurationError(missingField: keyof ServerConfig, value?: any): never {
  throw new Error(`Invalid or missing configuration: Field '${missingField}' is invalid. ${value !== undefined ? `Received: ${value}` : ''}`);
}

// Función de aserción que valida la configuración.
// Utiliza 'asserts' para refinar el tipo si la validación pasa.
function assertConfigIsValid(config: Partial<ServerConfig>): asserts config is ServerConfig {
  if (config.port === undefined || config.port <= 0 || config.port > 65535) {
    configurationError('port', config.port); // Lanza error -> never
  }
  if (config.apiKey === undefined || config.apiKey.trim() === '') {
    configurationError('apiKey'); // Lanza error -> never
  }
  if (config.databaseUrl === undefined || !config.databaseUrl.startsWith('postgres://')) {
     configurationError('databaseUrl', config.databaseUrl); // Lanza error -> never
  }
  // Si la función llega a este punto sin lanzar error, TypeScript sabe
  // que 'config' cumple con la interfaz 'ServerConfig' gracias a 'asserts'.
}

// Simula una configuración leída de un archivo o variables de entorno
const initialConfig: Partial<ServerConfig> = {
  port: 8080,
  // apiKey falta intencionalmente para probar el error
  databaseUrl: 'postgres://user:pass@host:5432/db'
};

try {
  assertConfigIsValid(initialConfig);
  // Si no hubo error, TypeScript sabe que 'initialConfig' es ahora 'ServerConfig'
  // y podemos acceder a sus propiedades de forma segura.
  console.log(`Configuration validated successfully.`);
  console.log(`Starting server on port ${initialConfig.port}...`);
  console.log(`Using API Key: ${initialConfig.apiKey.substring(0, 3)}...`); // Acceso seguro
  console.log(`Connecting to DB: ${initialConfig.databaseUrl}`); // Acceso seguro

} catch (error: any) {
  // Captura el error lanzado por configurationError
  console.error(`[FATAL] Application startup failed due to configuration error: ${error.message}`);
  // En un escenario real, aquí podríamos terminar el proceso de forma controlada.
  // process.exit(1);
}
```
Aquí, `assertConfigIsValid` utiliza una función auxiliar `configurationError` (que retorna `never`) para detenerse ante una configuración inválida. El uso de `asserts config is ServerConfig` es una característica clave que informa al compilador: si esta función retorna normalmente (es decir, no lanza error), entonces el tipo del objeto `config` debe ser refinado a `ServerConfig` en el código que sigue a la llamada. Esto evita la necesidad de comprobaciones redundantes después de la validación.

#### Comprobaciones exhaustivas (`Exhaustiveness Checks`)

Una técnica fundamental en TypeScript, especialmente con uniones discriminadas o `enum`. Se usa `never` en la rama `default` de un `switch` o en la rama `else` final de un `if/else if` para garantizar que todos los casos posibles han sido explícitamente manejados. Si se añade un nuevo miembro a la unión/enum y no se actualiza el `switch`/`if`, el compilador detectará un error porque el valor en la rama `default`/`else` ya no será asignable a `never`.

#### Ejemplo: Procesando diferentes tipos de eventos

```typescript
// Unión discriminada para representar diferentes tipos de eventos
type AppEvent =
  | { type: 'USER_LOGIN'; userId: string; timestamp: Date }
  | { type: 'USER_LOGOUT'; userId: string; timestamp: Date }
  | { type: 'ITEM_ADDED'; itemId: string; quantity: number; timestamp: Date };
  // | { type: 'ORDER_PLACED'; orderId: string; timestamp: Date }; // Si descomentamos esto, el switch fallará

// Función auxiliar para manejar casos imposibles
function assertUnreachable(x: never): never {
  throw new Error(`Unhandled case: ${JSON.stringify(x)}`);
}

function handleEvent(event: AppEvent): void {
  console.log(`Handling event type: ${event.type} at ${event.timestamp.toISOString()}`);

  switch (event.type) {
    case 'USER_LOGIN':
      // TypeScript sabe que 'event' aquí es { type: 'USER_LOGIN'; ... }
      console.log(`User ${event.userId} logged in.`);
      // Lógica específica para login...
      break;

    case 'USER_LOGOUT':
      // TypeScript sabe que 'event' aquí es { type: 'USER_LOGOUT'; ... }
      console.log(`User ${event.userId} logged out.`);
      // Lógica específica para logout...
      break;

    case 'ITEM_ADDED':
      // TypeScript sabe que 'event' aquí es { type: 'ITEM_ADDED'; ... }
      console.log(`${event.quantity} of item ${event.itemId} added.`);
      // Lógica específica para añadir item...
      break;

    // La rama 'default' asegura que todos los casos sean manejados.
    default:
      // Si hemos manejado todos los casos de 'AppEvent', TypeScript infiere
      // que 'event' en esta rama tiene el tipo 'never'.
      // Si añadiéramos un nuevo tipo a 'AppEvent' sin añadir un 'case',
      // 'event' ya no sería 'never', y la asignación a 'exhaustiveCheck' fallaría.
      const exhaustiveCheck: never = event;
      // Podemos llamar a una función 'never' para lanzar un error explícito.
      assertUnreachable(exhaustiveCheck);
  }
}

// Ejemplos de eventos
const loginEvent: AppEvent = { type: 'USER_LOGIN', userId: 'user123', timestamp: new Date() };
const addItemEvent: AppEvent = { type: 'ITEM_ADDED', itemId: 'itemABC', quantity: 5, timestamp: new Date() };

handleEvent(loginEvent);
handleEvent(addItemEvent);

// Si se añade 'ORDER_PLACED' a AppEvent sin actualizar el switch,
// al pasar un evento de ese tipo, la rama default se ejecutaría,
// TypeScript detectaría que 'event' no es 'never', y la llamada
// a assertUnreachable(event) daría un error de compilación.
```


{% hint style="info" %}
La comprobación exhaustiva mediante `never` es una red de seguridad crucial durante el desarrollo y mantenimiento. Transforma errores lógicos (olvidar manejar un caso nuevo) en errores detectables en tiempo de compilación, facilitando la refactorización segura y la evolución de los tipos de datos de la aplicación.
{% endhint %}

[↑ Volver al Índice](#toc-container)

### Consideraciones importantes

*   **Interrupción Inmediata del Flujo:** La principal consecuencia de llamar a una función `never` que lanza un error es la detención inmediata del flujo de ejecución normal en ese punto. Es crucial asegurarse de que este comportamiento abrupto sea intencional y adecuado para la situación (errores críticos que impiden continuar, estados lógicamente imposibles de alcanzar). No es un mecanismo para control de flujo normal.
*   **Distinción entre Errores de Fallo y Errores Operacionales:** Las funciones `never` que usan `throw` son parte del mecanismo de manejo de excepciones de JavaScript/TypeScript. Deben reservarse para señalar *fallos* o *errores excepcionales* (bugs en la lógica, estados inconsistentes irrecuperables, violación de precondiciones críticas). Para errores *operacionales* que forman parte del flujo esperado de la aplicación (entrada inválida del usuario, un recurso no encontrado que puede manejarse, un timeout de red esperado), es preferible utilizar otros patrones como devolver `null`, `undefined`, un objeto `Error` específico sin lanzar, un tipo `Result<T, E>`, o usar `Promise.reject` en código asíncrono. Esto permite un manejo más controlado y localizado por parte del código que llama a la función, sin detener abruptamente toda la cadena de ejecución.
*   **Tipado Explícito Mejora la Intención:** Aunque TypeScript a menudo puede inferir `never` correctamente (especialmente si la única instrucción es `throw`), anotar explícitamente el tipo de retorno como `never` (`function fail(): never { ... }`) mejora significativamente la legibilidad del código. Documenta de manera inequívoca la intención del desarrollador: esta función está diseñada para nunca retornar normalmente.
*   **Impacto en la Depuración:** Cuando una función `never` lanza una excepción, la traza de la pila (stack trace) generada apuntará directamente al lugar donde se ejecutó la instrucción `throw`. Esto es extremadamente útil para la depuración, ya que permite identificar rápidamente el origen de un fallo crítico en la aplicación.

[↑ Volver al Índice](#toc-container)

### Errores comunes y trampas habituales

*   **Confundir `never` con `void`:** Este es el error conceptual más frecuente. Recordar la tabla comparativa: `void` significa que la función retorna sin valor, pero *sí retorna* y completa su ejecución. `never` significa que la función *nunca retorna* de forma normal (lanza error o entra en bucle infinito). Usar `void` donde se necesita `never` puede ocultar código inalcanzable o fallos en comprobaciones exhaustivas. Usar `never` donde basta `void` es incorrecto semánticamente.


{% hint style="warning" %}
**No uses `throw` para control de flujo normal:** Las excepciones y, por extensión, las funciones `never` basadas en `throw`, están diseñadas para situaciones excepcionales o irrecuperables. Utilizarlas para lógica de negocio habitual (como validaciones simples que podrían devolver un booleano o un mensaje de error, decisiones condicionales normales, o indicar éxito/fracaso de una operación común) es una mala práctica. Este abuso de `throw` complica innecesariamente el código, lo hace más difícil de seguir, de depurar y de razonar sobre su flujo, ya que obliga a usar bloques `try...catch` en lugares donde no debería ser necesario.
{% endhint %}

*   **Código inalcanzable no detectado:** Si una función *podría* lanzar un error bajo ciertas condiciones, pero no *siempre* lo hace (es decir, tiene rutas de código que retornan normalmente), su tipo de retorno inferido **no** será `never` (será `void` o el tipo del valor retornado). Si se asume erróneamente que una función retorna `never` cuando no es así, se podría escribir código que se cree inalcanzable pero que en realidad podría ejecutarse, llevando a errores lógicos sutiles que el compilador no detectará.
*   **Olvidar el Manejo de Excepciones (`catch`):** Si una función `never` que lanza un error se invoca fuera de un bloque `try...catch` (o si el `catch` correspondiente no maneja específicamente ese tipo de error), la excepción no capturada se propagará hacia arriba en la pila de llamadas. Si no se captura en ningún nivel superior, detendrá la ejecución del programa de forma abrupta (en Node.js, típicamente terminará el proceso; en el navegador, detendrá la ejecución del script en ese hilo). Es esencial asegurarse de que los errores lanzados intencionalmente sean capturados en el nivel apropiado para evitar la terminación inesperada de la aplicación.

[↑ Volver al Índice](#toc-container)

### Buenas prácticas

*   **Reservar `never` (con `throw`) para Errores Irrecuperables y Estados Imposibles:** Utilízalo de forma específica y justificada para:
    *   Fallos de aserción (condiciones que *siempre* deben ser verdaderas).
    *   Errores críticos de configuración o inicialización que impiden el funcionamiento.
    *   Marcar ramas de código (como el `default` en un `switch` exhaustivo) que lógicamente nunca deberían alcanzarse si la lógica previa es correcta.
*   **Implementar Comprobaciones Exhaustivas Sistemáticamente:** Es uno de los usos más valiosos y recomendados de `never`. Asegúrate de que tus estructuras `switch` y cadenas `if/else if` que operan sobre uniones discriminadas o enums terminen con una rama `default` o `else` que asigne la variable discriminante a un tipo `never` o llame a una función auxiliar `never` (como `assertUnreachable`). Esto convierte la omisión de un caso en un error de compilación.
*   **Documentar Claramente la Razón del `never`:** Si una función tiene explícitamente el tipo de retorno `never`, explica de forma concisa pero clara en su documentación (JSDoc o comentarios) *por qué* nunca retorna. Ejemplos: `@returns {never} Siempre lanza una excepción si la configuración es inválida.` o `@returns {never} Función de utilidad para marcar casos inalcanzables en comprobaciones exhaustivas.`.
*   **Crear Funciones de Utilidad Reutilizables:** Define funciones auxiliares pequeñas y bien nombradas como `fail(message: string): never` o `assertUnreachable(x: never, message?: string): never` para centralizar y estandarizar la lógica de manejo de errores irrecuperables o estados imposibles. Esto mejora la consistencia y reduce la duplicación de código.

[↑ Volver al Índice](#toc-container)

### Malas prácticas

*   **Manejar Errores Operacionales Esperados con `throw`/`never`:** Evita usar este mecanismo para situaciones como validación de formularios (donde se espera feedback al usuario), fallos de red previsibles (que pueden requerir reintentos), búsqueda de datos que pueden no existir (`find` podría devolver `null` o `undefined`). Para estos casos, usa tipos de retorno más informativos y manejables (`Result<T, E>`, `Promise<T>`, `T | null`, `T | undefined`, objetos de error específicos sin `throw`).
*   **Lanzar Errores Excesivamente Genéricos:** En lugar de simplemente `throw new Error("Ocurrió un error")`, cuando uses `throw` (incluso en funciones `never`), intenta lanzar tipos de error más específicos (`throw new ConfigurationError(...)`, `throw new ValidationError(...)`, `throw new UnreachableCaseError(...)`). Esto facilita la captura y el manejo diferenciado de errores en los bloques `catch` de niveles superiores.
*   **Colocar Código Ejecutable Después de una Llamada a `never`:** No escribas código que lógicamente deba ejecutarse inmediatamente después de invocar una función `never` dentro del mismo bloque o ruta de control. Aunque TypeScript generalmente lo marcará como inalcanzable, es confuso para la lectura humana y denota una posible incomprensión del propósito de `never`.

{% hint style="warning" %}
**Evita código tras llamada `never`:** No coloques código ejecutable después de invocar una función `never` en el mismo bloque lógico. Es confuso y siempre será inalcanzable, TypeScript suele advertirlo.
{% endhint %}

*   **Suprimir Errores de Comprobación Exhaustiva:** Nunca ignores deliberadamente (por ejemplo, usando `//@ts-ignore`) los errores del compilador que surgen en las ramas `default`/`else` diseñadas para `never`. Esos errores son valiosos: indican que falta manejar un caso legítimo en tu lógica, y suprimirlos introduce potenciales bugs.

[↑ Volver al Índice](#toc-container)

---

## Funciones que nunca terminan (`while (true) {}`)

La segunda vía principal, aunque menos común en la práctica diaria de muchas aplicaciones, para que una función tenga el tipo `never` es mediante la presencia de un bucle infinito del cual no existe una ruta de salida posible (como un `while (true)` que no contiene instrucciones `break`, `return` o `throw` alcanzables dentro del bucle). Este tipo de función nunca completa su ejecución normal y, por lo tanto, nunca devuelve el control al punto desde el que fue llamada.

### Definición y utilidad

En ciertos contextos muy específicos, como la implementación del punto de entrada principal de servidores de larga duración, procesos demonio (daemons), o bucles centrales de procesamiento de eventos (game loops, ciertos frameworks reactivos muy específicos), una función puede estar diseñada intencionalmente para ejecutarse de forma indefinida. TypeScript es capaz de reconocer esta estructura de bucle infinito y, si puede determinar estáticamente que no hay una ruta de escape lógica, inferirá que el tipo de retorno de la función es `never`.

La anotación o inferencia de `never` en este contexto indica que la función está destinada a **monopolizar el hilo de ejecución** (si es un bucle síncrono bloqueante) o, más conceptualmente, a representar un **proceso persistente** que se ejecuta hasta que es terminado externamente.

#### Ejemplo: Función con bucle infinito síncrono (Ilustrativo, ¡Peligroso!)

```typescript
// Función diseñada para ejecutarse indefinidamente, simulando procesamiento.
// ¡ADVERTENCIA! Este ejemplo es puramente ilustrativo del concepto 'never'.
// Un bucle infinito síncrono como este bloqueará el hilo principal.
function infiniteProcessor(): never {
  console.log("Infinite processor started. This will block the main thread.");
  let counter = 0;
  while (true) {
    // Simula algún trabajo computacional
    const start = Date.now();
    while(Date.now() - start < 500) { /* Busy wait - consume CPU */ }

    counter++;
    console.log(`Processing cycle ${counter}...`);

    // No hay 'break', 'return' ni 'throw' dentro del bucle.
    // La función nunca saldrá de este bucle 'while(true)'.
  }
  // Este código es inalcanzable y TypeScript lo sabe.
  // console.log("Processor finished."); // Nunca se imprime
}

// --- NO EJECUTAR ESTO EN UN ENTORNO REAL ---
// Llamar a infiniteProcessor() bloquearía completamente el hilo actual.
// Si esto fuera Node.js, el bucle de eventos se detendría.
// Si fuera en un navegador, la interfaz de usuario se congelaría.
// try {
//   infiniteProcessor();
// } catch (error) {
//   // Este catch sólo se activaría si ocurriera un error DENTRO del bucle
//   // que no fuera manejado internamente y lo hiciera terminar.
//   console.error("Infinite processor crashed:", error);
// }
// console.log("This line would never be reached if infiniteProcessor() was called.");
```
La función `infiniteProcessor` contiene un `while (true)` sin ninguna condición de salida explícita (`break`, `return`, `throw`). Por lo tanto, TypeScript infiere correctamente `never` como su tipo de retorno, indicando que nunca devolverá el control de forma normal. **Es crucial entender que este patrón síncrono es generalmente una mala práctica en JavaScript/TypeScript por el bloqueo que genera.**

[↑ Volver al Índice](#toc-container)

### Casos de uso reales y recomendables

Aunque los bucles infinitos *síncronos* son problemáticos, el concepto de `never` para funciones que no retornan se aplica conceptualmente a puntos de entrada de procesos diseñados para ejecutarse indefinidamente. Sin embargo, en implementaciones reales y robustas, estos procesos casi siempre utilizan mecanismos **asíncronos** para evitar el bloqueo.

#### Puntos de entrada de Servidores/Demonios (Modelo Asíncrono)

La función principal que inicia un servidor HTTP, un worker de colas de mensajes, o cualquier proceso destinado a ejecutarse continuamente hasta ser terminado externamente, generalmente usa patrones asíncronos. Aunque la función *iniciadora* pueda retornar (p.ej., después de configurar listeners), el *proceso* que inicia se ejecuta indefinidamente. El `never` aquí es más conceptual sobre el ciclo de vida del proceso que sobre el tipo de retorno literal de la función de arranque síncrona.

#### Ejemplo: Inicio de un servidor HTTP (Node.js - Asíncrono)

```typescript
import http from 'http'; // Módulo estándar de Node.js

// Esta función configura e inicia el servidor, pero retorna después de llamar a listen.
// El servidor mismo se ejecuta "indefinidamente" de forma asíncrona.
// No retorna 'never' directamente, pero inicia un proceso que conceptualmente nunca termina.
function startHttpServer(port: number): http.Server {
  const server = http.createServer((req, res) => {
    // Manejo de peticiones (asíncrono por naturaleza en Node.js)
    console.log(`Received request: ${req.method} ${req.url}`);
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello from the asynchronous server!
');
  });

  server.listen(port, () => {
    // Este callback se ejecuta una vez que el servidor está escuchando.
    console.log(`Server listening asynchronously on http://localhost:${port}`);
    console.log('Node.js event loop is running. Press Ctrl+C to stop.');
  });

  // Manejo de errores del servidor (importante en producción)
  server.on('error', (error) => {
    console.error('Server error:', error);
    // Podríamos intentar reiniciar o terminar el proceso aquí.
  });

  console.log('Server initialization setup complete. Returning server instance.');
  // La función retorna la instancia del servidor, no es 'never'.
  return server;
}

// Iniciar el servidor
// const myServer = startHttpServer(3000);

// El script de Node.js no terminará aquí porque el servidor iniciado
// mantiene el bucle de eventos activo. El proceso sigue vivo.
// console.log('Script execution finished, but server is running.');
```
En este caso realista, `startHttpServer` retorna una instancia `http.Server`. No es `never`. Sin embargo, la *operación* que inicia (el servidor escuchando peticiones) es persistente y no termina por sí sola. El `never` síncrono bloqueante rara vez es la forma correcta de implementar esto.

#### Bucles de juego o renderizado continuo (Modelo con `requestAnimationFrame`)

En aplicaciones gráficas o juegos en el navegador, un bucle principal se encarga de leer entradas, actualizar el estado y renderizar la pantalla repetidamente. La forma moderna y eficiente de hacerlo no es con `while(true)` síncrono, sino con `requestAnimationFrame`, que es asíncrono y gestionado por el navegador.

#### Ejemplo: Bucle de animación simple (Navegador - Asíncrono)

```typescript
// No retorna 'never', pero representa un bucle conceptualmente infinito gestionado por el navegador.
function animationLoop(timestamp: DOMHighResTimeStamp): void {
  // timestamp es proporcionado por requestAnimationFrame

  // 1. Update animation state based on time elapsed
  const element = document.getElementById('animated-box');
  if (element) {
    const rotation = timestamp / 10; // Simple rotation based on time
    element.style.transform = `rotate(${rotation}deg)`;
  }

  // 2. Render (el navegador lo hace después de actualizar estilos)

  // 3. Schedule the next frame
  // La llamada recursiva asegura que el bucle continúe.
  requestAnimationFrame(animationLoop);
}

// Para iniciar el bucle:
// requestAnimationFrame(animationLoop);

// El script continúa ejecutándose, y el navegador llama a animationLoop
// de forma óptima antes de cada repintado. No bloquea la UI.
```
De nuevo, la función `animationLoop` retorna `void`, no `never`. El bucle es mantenido por llamadas recursivas a `requestAnimationFrame`. El patrón `never` con `while(true)` síncrono no es adecuado aquí.

[↑ Volver al Índice](#toc-container)

### Consideraciones importantes


{% hint style="danger" %}
**¡CRÍTICO! Bloqueo del Hilo Principal con Bucles Síncronos:** Es la consideración más importante. Un bucle `while(true)` síncrono dentro de una función (sea `never` o no) **congela completamente** el hilo de ejecución donde corre.
*   En **Node.js**, esto detiene el bucle de eventos (Event Loop). El servidor dejará de aceptar nuevas conexiones, no procesará timeouts, no realizará operaciones de I/O pendientes. La aplicación se vuelve completamente insensible.
*   En el **navegador**, congela la interfaz de usuario (UI). La página dejará de responder a clics, desplazamientos, y cualquier interacción. El navegador a menudo mostrará un mensaje de "Página no responde".
**Este patrón es casi siempre inaceptable en el hilo principal.**
{% endhint %}


{% hint style="info" %}
**Prioriza Siempre las Alternativas Asíncronas:** Para cualquier tarea que necesite ejecutarse de forma continua, persistente o de larga duración en el ecosistema JavaScript/TypeScript (servidores, workers, listeners, animaciones, manejo de eventos), los modelos **asíncronos** son la norma y la práctica recomendada. Utiliza:
*   `async/await` con `Promises` para operaciones I/O.
*   `EventEmitter` o streams en Node.js para manejo de eventos.
*   `setTimeout`, `setInterval` para tareas periódicas (con precaución).
*   `requestAnimationFrame` para animaciones y renderizado en el navegador.
*   Web Workers para tareas computacionalmente intensivas en segundo plano (navegador y Node.js).
Estos mecanismos permiten la concurrencia (o pseudo-concurrencia en el caso de JS single-thread) y evitan el bloqueo del hilo principal, manteniendo la aplicación responsiva. Una función `never` basada en un bucle síncrono bloqueante es raramente, si acaso alguna vez, la solución adecuada en producción.
{% endhint %}

*   **Intencionalidad y Contexto:** Si *realmente* necesitas un bucle infinito síncrono (lo cual es extremadamente raro y generalmente limitado a scripts muy específicos fuera del contexto web/servidor típico), asegúrate de que sea absolutamente intencional y de que comprendes plenamente las consecuencias del bloqueo del hilo. Documenta exhaustivamente por qué es necesario.
*   **Mecanismos de Terminación:** Una función `never` basada en un bucle infinito solo termina si:
    *   El proceso que la ejecuta es detenido externamente (ej: `Ctrl+C` en la terminal, `kill` del proceso).
    *   Ocurre un error *no capturado* dentro del bucle que provoca su terminación abrupta. No hay salida "limpia" programada desde dentro del bucle.
*   **Evitar el `Busy-Waiting` a toda costa:** Un bucle `while(true)` que no realiza operaciones de espera significativas (como esperar I/O asíncrono, usar `setTimeout`, `await delay`, etc.) y en su lugar comprueba condiciones repetidamente en lugar de usar mecanismos de espera basados en eventos o I/O. Esto consumirá el 100% de un núcleo de CPU. Siempre deben introducirse mecanismos de espera apropiados si un bucle debe mantenerse activo.
*   **Confusión con Procesos Asíncronos de Larga Duración:** Es fácil confundir una función `never` síncrona bloqueante con un proceso asíncrono que se ejecuta durante mucho tiempo (como un servidor Node.js típico). El servidor Node.js, aunque se ejecuta "indefinidamente", lo hace de forma asíncrona, sin bloquear el bucle de eventos, lo que le permite manejar múltiples conexiones concurrentemente. No utiliza un `while(true)` síncrono bloqueante para su operación principal.

[↑ Volver al Índice](#toc-container)

### Buenas prácticas

*   **Limitar Estrictamente a Puntos de Entrada Muy Específicos (Si Acaso):** Si excepcionalmente se requiere un bucle infinito síncrono (difícil de justificar en JS/TS moderno), debería estar confinado al punto de entrada *principal* de un script o proceso diseñado específicamente para operar de esa manera, y preferiblemente *nunca* en el hilo principal si se necesita algún tipo de concurrencia o responsividad (I/O, UI).


{% hint style="success" %}
**Favorece Abrumadoramente los Modelos Asíncronos:** Para prácticamente todas las tareas persistentes, de larga duración, o basadas en eventos en JavaScript/TypeScript (servidores, workers, listeners de eventos, animaciones), **siempre** prefiere los patrones asíncronos (`async/await`, `Promises`, `EventEmitter`, `requestAnimationFrame`, Web Workers). Son más eficientes, no bloquean el hilo principal y permiten que la aplicación siga siendo responsiva.
{% endhint %}

*   **Incluir Manejo de Errores Robusto Dentro del Bucle:** Si un bucle está diseñado para ser persistente (aunque sea asíncrono), debe ser resiliente a errores internos. Implementa bloques `try...catch` *dentro* del bucle para manejar errores esperados en cada iteración sin que detengan todo el proceso, permitiendo que el bucle continúe si es apropiado.
*   **Introducir Mecanismos de Espera Eficientes:** Incluso en bucles asíncronos, evita el `busy-waiting`. Utiliza `await` en operaciones de I/O, `await delay(...)`, `setTimeout`, `setInterval`, o los mecanismos de espera específicos del entorno (como `server.listen` en Node.js o `requestAnimationFrame` en el navegador) para liberar el hilo y reducir el consumo innecesario de CPU.
*   **Documentar Rigurosamente:** Si creas una función cuyo ciclo de vida implica un bucle conceptualmente infinito (incluso asíncrono), explica claramente en la documentación su propósito, cómo se mantiene activo, cómo se manejan los errores y, crucialmente, si implica algún tipo de bloqueo o consideración de rendimiento. Ejemplo: `@returns {Promise<void>} // Esta función inicia un listener que se ejecuta indefinidamente de forma asíncrona.`.

[↑ Volver al Índice](#toc-container)

### Malas prácticas


{% hint style="danger" %}
**NUNCA Bloquear el Hilo Principal con `while(true)` Síncrono:** Esta es la principal y más crítica mala práctica a evitar con funciones `never` (o cualquier función) que contienen bucles infinitos. El impacto en la responsividad de la aplicación (Node.js o navegador) es severo e inaceptable en la mayoría de los casos. **Evítalo a toda costa.**
{% endhint %}

*   **Implementar `Busy-waiting`:** Crear bucles (síncronos o incluso asíncronos sin `await` adecuado) que consumen ciclos de CPU innecesariamente comprobando condiciones repetidamente en lugar de usar mecanismos de espera basados en eventos o I/O.
*   **No Incluir Manejo de Errores Dentro de Bucles Persistentes:** Permitir que errores ocurridos durante una iteración de un proceso que debería ser resiliente (como un servidor o un worker) detengan todo el proceso sin un manejo adecuado o estrategia de recuperación.
*   **Usar un Bucle Infinito (`never`) Cuando un Bucle Condicional es Suficiente:** No fuerces el uso de `while(true)` y un tipo `never` si el proceso tiene una condición de finalización natural o debería terminar bajo ciertas circunstancias lógicas. Usa `while (condition)` o `for` con condiciones de salida claras en esos casos.

[↑ Volver al Índice](#toc-container)

