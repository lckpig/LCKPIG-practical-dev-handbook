<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/void-type)
<!-- MULTILANGUAJE MENU END -->

<!--
# El tipo `void` y su uso en funciones
- Diferencias entre `void` y `undefined` en retornos
- Uso en funciones sin retorno explícito
-->

<details id="toc-container">
<summary>Índice de contenidos</summary>

<!-- no toc -->
- [Diferencias entre `void` y `undefined` en retornos](#diferencias-entre-void-y-undefined-en-retornos)
  - [Definición y Propósito](#definicion-y-proposito)
  - [Tabla Comparativa: `void` vs `undefined` en Retornos](#tabla-comparativa-void-vs-undefined-en-retornos)
  - [Implicaciones en el Tipado](#implicaciones-en-el-tipado)
  - [Consideraciones Importantes](#consideraciones-importantes)
  - [Buenas Prácticas](#buenas-practicas)
  - [Malas Prácticas](#malas-practicas)
- [Uso en funciones sin retorno explícito](#uso-en-funciones-sin-retorno-explicito)
  - [Inferencia de Tipo `void`](#inferencia-de-tipo-void)
  - [Casos de Uso Comunes](#casos-de-uso-comunes)
  - [`async` y `Promise<void>`](#async-y-promisevoid)
  - [Consideraciones Importantes](#consideraciones-importantes-1)
  - [Buenas Prácticas](#buenas-practicas-1)
  - [Malas Prácticas](#malas-practicas-1)

</details>

# El tipo `void` y su uso en funciones

En TypeScript, el tipo `void` representa la **ausencia intencional de un valor de retorno** en una función. Es una forma explícita y semántica de indicar que una función realiza una acción o efecto secundario (como modificar el estado, imprimir en consola, llamar a otra API, etc.), pero **no está diseñada para devolver un resultado útil** que el código invocante deba procesar o utilizar. Comprender `void` es fundamental para escribir código TypeScript claro, mantenible y alineado con la filosofía del lenguaje, especialmente al diferenciarlo de `undefined`.

---

## Diferencias entre `void` y `undefined` en retornos

Aunque a primera vista `void` y `undefined` pueden parecer similares, y de hecho una función `void` en JavaScript subyacente devuelve `undefined`, en el contexto de TypeScript tienen propósitos distintos y comunican intenciones fundamentalmente diferentes. La elección entre uno y otro afecta la claridad y la robustez del código.

### Definición y Propósito

-   **`void`**:
    -   Se utiliza **exclusivamente** como tipo de retorno para funciones que *intencionadamente* no devuelven ningún valor significativo.
    -   Su propósito es la **claridad semántica**: señaliza de forma explícita que la función se ejecuta por sus efectos secundarios y que cualquier valor que pudiera retornar (implícitamente `undefined` en JS) **no debe ser utilizado**.
    -   Forma parte del sistema de tipos de TypeScript para mejorar la expresividad y seguridad.

-   **`undefined`**:
    -   Es tanto un **tipo primitivo** como un **valor literal** en JavaScript y TypeScript.
    -   Indica la ausencia de un valor asignado: una variable no inicializada, una propiedad de objeto inexistente, o el valor devuelto por una función que no tiene `return` explícito (en JavaScript) o que retorna `undefined` explícitamente.
    -   A diferencia de `void` (que es solo un tipo de retorno), `undefined` es un valor *real* que puede ser asignado a variables, comparado y utilizado en expresiones (aunque a menudo su uso directo indica un posible error o estado inesperado).


[↑ Volver al Índice](#toc-container)


### Tabla Comparativa: `void` vs `undefined` en Retornos

La siguiente tabla resume las diferencias clave entre usar `void` y `undefined` como tipo de retorno de una función en TypeScript:

| Característica          | `void`                                                             | `undefined`                                                                            |
| :---------------------- | :----------------------------------------------------------------- | :------------------------------------------------------------------------------------- |
| **Naturaleza**          | Tipo de retorno exclusivo para funciones.                          | Tipo primitivo y valor literal.                                                        |
| **Intención Semántica** | Indica explícitamente la ausencia de valor de retorno *útil*.      | Indica la ausencia de un valor asignado; puede ser un valor *esperado*.                |
| **Propósito Principal** | Señalar funciones ejecutadas por sus efectos secundarios.          | Representar un valor faltante o no inicializado; puede ser un retorno *significativo*. |
| **Uso Principal**       | Tipo de retorno (`function fn(): void {}`).                        | Tipo de variable (`let x: number                                                       | undefined;`), valor (`return undefined;`). |
| **Valor en Runtime**    | La función devuelve `undefined` implícitamente en JavaScript.      | El valor literal `undefined`.                                                          |
| **Uso del Resultado**   | **Desaconsejado**. El tipado `void` indica no usar el resultado.   | **Permitido y esperado**. El código debe manejar el posible valor `undefined`.         |
| **Claridad**            | Alta. Deja clara la intención de no devolver nada útil.            | Menor si se usa solo para indicar "sin retorno", puede ser ambiguo.                    |
| **Compatibilidad JS**   | Permite `return undefined;` por compatibilidad (mala práctica TS). | Comportamiento estándar de JS para funciones sin `return`.                             |
| **Alternativas**        | Ninguna (para esta intención específica).                          | `null`, tipos unión (`T                                                                | null`), lanzar errores, Result Types.      |


[↑ Volver al Índice](#toc-container)


### Implicaciones en el Tipado

La principal diferencia radica en la **intención** que comunican al compilador y a otros desarrolladores:

-   **`function miFuncion(): void { ... }`**: Esta firma declara inequívocamente: "Esta función realiza una acción, pero no esperes obtener un valor útil de ella. Si intentas usar su resultado, probablemente estés cometiendo un error".
    -   Aunque técnicamente podrías hacer `return undefined;` dentro de una función `void` (por compatibilidad histórica con JavaScript), **hacerlo es confuso y considerado una mala práctica en TypeScript**. Oscurece la intención de `void`.

        {% hint style="warning" %}
        Evita `return undefined;` en funciones tipadas como `void` en TypeScript. Utiliza simplemente `return;` para salidas tempranas si es necesario.
        {% endhint %}

    -   TypeScript *permite* asignar el resultado de una función `void` a una variable (ej: `const res = miFuncion();`), pero el tipo inferido para `res` será `void`, y su valor en tiempo de ejecución será `undefined`. Sin embargo, la propia declaración `void` **desaconseja fuertemente** este uso. El compilador no te impedirá hacerlo directamente, pero herramientas de linting más estrictas o revisiones de código deberían señalarlo como un posible error de lógica.

-   **`function otraFuncion(): T | undefined { ... }`**: Esta firma indica que la función *puede* devolver un valor de tipo `T`, pero también *puede* devolver `undefined` como un resultado *significativo* y esperado en ciertos escenarios (por ejemplo, una función de búsqueda que devuelve el objeto encontrado o `undefined` si no lo encuentra). Aquí, `undefined` es un valor legítimo que el código invocante debe manejar explícitamente (por ejemplo, con una comprobación `if (resultado !== undefined) { ... }`).


[↑ Volver al Índice](#toc-container)


#### Ejemplos

```typescript
// Función explícitamente tipada como void: Realiza acción, no devuelve nada útil.
function mostrarMensaje(mensaje: string): void {
  console.log(mensaje);
  // No hay sentencia return, o solo `return;` para salida temprana
}

// TypeScript infiere void si no hay return explícito que devuelva valor
function saludar() { // Inferido como (): void
  console.log("Hola Mundo");
}

// Función que SÍ devuelve un valor que PUEDE ser undefined
function encontrarUsuario(id: number): { nombre: string } | undefined {
  if (id === 1) {
    return { nombre: "Alice" };
  }
  // Si no se encuentra, devuelve undefined explícitamente como valor significativo
  return undefined;
}
const usuario = encontrarUsuario(2);
if (usuario !== undefined) {
  console.log(usuario.nombre);
} else {
  console.log("Usuario no encontrado.");
}

// Función que devuelve undefined explícitamente (a menudo menos claro que void o T | null)
function devuelveUndefinedLiteral(): undefined {
  // Aunque válido, usar `undefined` como tipo de retorno explícito
  // suele ser menos común y claro que usar `void` para "sin retorno"
  // o `T | null` para "valor opcional".
  return undefined;
}
const resultadoUnd = devuelveUndefinedLiteral();
console.log(resultadoUnd); // Output: undefined

// Uso del resultado de una función void (desaconsejado y generalmente inútil)
const resultadoVoid = mostrarMensaje("Probando void");
console.log(resultadoVoid); // Output: undefined. Aunque se asigna, `void` indica que no deberíamos usar este valor.
```


[↑ Volver al Índice](#toc-container)


### Consideraciones Importantes

-   **Claridad del Código**: Usar `void` mejora significativamente la legibilidad del código al comunicar de forma explícita la intención del autor de la función: "esta función no está hecha para devolver un valor consumible". Esto evita ambigüedades sobre si la ausencia de un `return` fue intencional o un simple olvido.
-   **Intención vs. Implementación**: En JavaScript, una función sin `return` devuelve `undefined`. TypeScript respeta esto en tiempo de ejecución, pero el tipo `void` añade una capa semántica en tiempo de compilación para guiar al desarrollador sobre cómo *debería* usarse la función.
-   **Compatibilidad con JavaScript**: La permisividad de TypeScript al permitir `return undefined;` en funciones `void` existe principalmente por razones de compatibilidad con código JavaScript existente que podría ser migrado a TypeScript. Sin embargo, en código TypeScript nuevo y idiomático, se debe evitar esta práctica en favor de `return;` o ninguna sentencia `return`.
-   **Alternativas a `undefined` como Valor de Retorno**: Si una función necesita indicar explícitamente la ausencia de un resultado válido (en lugar de simplemente no tener un resultado), devolver `undefined` directamente a menudo es menos preferible que otras alternativas más semánticas y robustas:
    -   **`null`**: Tradicionalmente usado en muchos lenguajes para indicar "ausencia intencional de un objeto" o "no encontrado". `function buscar(): T | null`.
    -   **Lanzar un Error**: Si la ausencia del valor representa una condición excepcional o un error. `function obtenerConfig(): Config { throw new Error("..."); }`.
    -   **Tipos Unión Específicos**: Devolver un objeto con un estado, como `{ encontrado: false } | { encontrado: true, valor: T }`.
    -   **Tipos Resultado (Result Types)**: Patrón común (a menudo implementado con librerías) que encapsula éxito (`Ok<T>`) o fallo (`Err<E>`).


[↑ Volver al Índice](#toc-container)


### Buenas Prácticas

{% hint style="success" %}
Tipa explícitamente como `void` las funciones cuyo propósito principal son los efectos secundarios y no devuelven intencionadamente un valor, incluso si TypeScript podría inferirlo. Esto refuerza la intención del código.
{% endhint %}

-   Utiliza `return;` (sin valor) para realizar salidas tempranas de funciones `void` cuando la lógica condicional lo requiera.
-   Activa la opción de compilador `--noImplicitReturns` en `tsconfig.json`. Esto ayuda a asegurar que todas las rutas de código en funciones que *se supone* que devuelven un valor (es decir, no tipadas como `void` o `any`) realmente lo hagan, previniendo errores por olvido.
-   Cuando una función *necesita* indicar la ausencia de un valor como resultado *válido*, prefiere `T | null` sobre `T | undefined`, ya que `null` suele ser semánticamente más claro para "ausencia intencional", mientras que `undefined` a menudo se asocia con estados no inicializados o errores.


[↑ Volver al Índice](#toc-container)


### Malas Prácticas

{% hint style="warning" %}
Declarar una función como `void` y luego asignar su resultado a una variable con la intención de usar ese valor (`undefined`). Esto contradice directamente la semántica de `void` y probablemente indica un malentendido o un error lógico.
{% endhint %}

{% hint style="danger" %}
Usar `undefined` como tipo de retorno explícito (`function fn(): undefined`) cuando la verdadera intención es indicar que la función no tiene un valor de retorno significativo. En este caso, siempre se debe usar `void`.
{% endhint %}

-   Retornar `undefined` explícitamente (`return undefined;`) desde una función tipada como `void`. Aunque técnicamente permitido, es confuso y va en contra de las convenciones de TypeScript.
-   Depender del valor `undefined` devuelto implícitamente por una función `void` en el código que la llama.


[↑ Volver al Índice](#toc-container)



---

## Uso en funciones sin retorno explícito

TypeScript juega un papel importante en cómo se manejan las funciones que no tienen una sentencia `return` explícita que devuelva un valor, o que solo usan `return;` sin un valor.

### Inferencia de Tipo `void`

Cuando defines una función en TypeScript y no incluyes ninguna sentencia `return` que devuelva un valor, o si solo usas `return;` para una salida temprana, el compilador de TypeScript es lo suficientemente inteligente como para **inferir automáticamente** que el tipo de retorno de esa función es `void`.

Este comportamiento es una mejora semántica respecto a JavaScript puro. Mientras que en JavaScript una función sin `return` simplemente devuelve el valor `undefined`, la inferencia de `void` en TypeScript añade una capa de intención: el compilador entiende que no se espera un valor de retorno útil de esta función.


[↑ Volver al Índice](#toc-container)


#### Ejemplos

```typescript
// TypeScript infiere el tipo de retorno como (): void
function procesarDatos(datos: any[]) {
  console.log("Iniciando procesamiento de datos...");
  // Simula algún trabajo con los datos
  datos.forEach((dato, index) => {
    console.log(`Elemento ${index}:`, dato);
  });
  console.log("Procesamiento completado.");
  // No hay sentencia 'return' que devuelva un valor
}

// Lo mismo aplica a las funciones flecha
const logEvento = (evento: string, timestamp: Date) => { // Infiere (evento: string, timestamp: Date) => void
  console.log(`[${timestamp.toISOString()}] Evento registrado: ${evento}`);
  // No hay 'return'
};

// Incluso con un return vacío, sigue siendo void
function validarEntrada(entrada: string | null): void { // Tipado explícito (buena práctica), pero inferiría void
  if (entrada === null || entrada.trim() === '') {
    console.error("Error: La entrada no puede estar vacía.");
    // Salida temprana sin devolver valor
    return;
  }
  // Continúa el procesamiento si la entrada es válida
  console.log("Entrada válida recibida:", entrada);
}

// Llamadas a las funciones
procesarDatos([1, 'test', true]);
logEvento('Inicio de sesión de usuario', new Date());
validarEntrada("Datos correctos");
validarEntrada(null); // Provoca el return temprano
```


[↑ Volver al Índice](#toc-container)


### Casos de Uso Comunes

Las funciones `void` son extremadamente comunes y fundamentales en la programación del día a día, especialmente cuando interactuamos con el "mundo exterior" o realizamos acciones que no producen un resultado directo calculable:

-   **Manejadores de Eventos (Event Handlers / Listeners)**: Funciones que se ejecutan en respuesta a interacciones del usuario (clics, escritura, movimientos del ratón) o eventos del sistema (carga de página, recepción de mensajes). Su propósito es *reaccionar* al evento, usualmente actualizando el estado de la aplicación, modificando la interfaz de usuario (UI), o llamando a otras funciones.
    ```typescript
    // Ejemplo: Manejador de clic para un botón
    const miBoton = document.getElementById('submitButton');
    if (miBoton) {
      miBoton.addEventListener('click', (event: MouseEvent): void => {
        event.preventDefault(); // Prevenir comportamiento por defecto del formulario
        console.log('Formulario enviado (simulado)');
        // Aquí iría la lógica para enviar datos, actualizar UI, etc.
        // No se espera un valor de retorno de un event listener.
      });
    }
    ```

-   **Logging, Monitorización y Telemetría**: Funciones diseñadas específicamente para registrar información (errores, eventos importantes, métricas de rendimiento) en la consola, archivos de log, o enviarla a servicios externos de análisis o monitorización. Su trabajo es registrar, no calcular un valor.
    ```typescript
    // Ejemplo: Función simple de logging
    function logError(message: string, severity: 'info' | 'warning' | 'error'): void {
      const timestamp = new Date().toISOString();
      console.error(`[${timestamp}] [${severity.toUpperCase()}] ${message}`);
      // Podría también enviar esto a un servicio externo:
      // sendToMonitoringService({ timestamp, severity, message });
    }
    ```

-   **Manipulación Directa del DOM**: Funciones que interactúan directamente con la estructura HTML de una página web, como añadir, eliminar o modificar elementos, cambiar estilos o atributos. Estas acciones modifican el estado de la página, pero no suelen devolver un valor.
    ```typescript
    // Ejemplo: Actualizar un elemento de texto en la UI
    function actualizarMensajeEstado(mensaje: string): void {
      const statusElement = document.getElementById('statusMessage');
      if (statusElement) {
        statusElement.textContent = mensaje;
        statusElement.style.color = mensaje.startsWith('Error') ? 'red' : 'green';
      }
    }
    ```

-   **Funciones de Configuración o Inicialización**: Código que se ejecuta al inicio de la aplicación o de un módulo para establecer configuraciones iniciales, conectar a servicios, registrar componentes, etc.
    ```typescript
    // Ejemplo: Inicializar una librería externa
    function inicializarLibreriaGraficos(): void {
      // Supongamos que hay una librería global 'SuperCharts'
      // SuperCharts.config({ theme: 'dark', responsive: true });
      console.log("Librería de gráficos inicializada.");
    }
    ```
-   **Funciones de Modificación de Estado (Mutaciones)**: En algunos patrones (aunque a menudo se prefieren funciones puras), puedes tener funciones cuyo único propósito es modificar directamente un objeto o estado compartido.
    ```typescript
    // Ejemplo (simple, patrones más complejos como Redux manejan esto diferente)
    let contadorGlobal = 0;
    function incrementarContador(): void {
        contadorGlobal++;
        console.log(`Contador ahora es: ${contadorGlobal}`);
    }
    ```


[↑ Volver al Índice](#toc-container)


### `async` y `Promise<void>`

La relación entre funciones asíncronas (`async`) y `void` sigue la misma lógica fundamental, pero con la capa adicional de las Promesas.

-   Una función declarada con `async` **siempre** devuelve una `Promise`. Esto es una regla del lenguaje JavaScript/TypeScript.
-   Si una función `async` realiza una operación asíncrona (como una llamada a API, una operación de E/S de archivos, un `setTimeout`) pero **no está destinada a devolver un valor útil** una vez que la operación asíncrona se complete, su tipo de retorno debe ser `Promise<void>`.
-   `Promise<void>` comunica claramente: "Esta función realiza trabajo asíncrono, y cuando termine, la Promesa se resolverá, pero no esperes obtener un valor de esa resolución".
-   El código que utilice `await` sobre una función que devuelve `Promise<void>` recibirá `undefined` cuando la promesa se resuelva. Sin embargo, al igual que con `void` síncrono, la firma `Promise<void>` indica que este `undefined` no tiene significado y no debería ser utilizado.


[↑ Volver al Índice](#toc-container)


#### Ejemplo `async/Promise<void>`

```typescript
// Ejemplo: Función asíncrona que guarda datos pero no devuelve nada
async function guardarConfiguracionUsuario(userId: number, config: object): Promise<void> {
  console.log(`Guardando configuración para el usuario ${userId}...`);
  try {
    // Simula una llamada a API asíncrona
    await fetch(`/api/users/${userId}/config`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(config),
    });
    console.log(`Configuración guardada exitosamente para el usuario ${userId}.`);
    // La promesa se resuelve sin valor cuando fetch termina
  } catch (error) {
    console.error(`Error al guardar configuración para ${userId}:`, error);
    // La promesa se rechazaría en caso de error
    // Podríamos querer lanzar el error aquí para que el llamador lo maneje
    // throw error; // Si hacemos esto, la promesa se rechaza. Si no, se resuelve (a void).
  }
}

// Uso de la función async void
async function procesoPrincipalUsuario(id: number): Promise<void> {
  const nuevaConfig = { theme: 'dark', notifications: true };
  await guardarConfiguracionUsuario(id, nuevaConfig);
  // No esperamos un valor de guardarConfiguracionUsuario
  console.log("Proceso principal del usuario completado después de guardar.");
}

procesoPrincipalUsuario(123);
```


[↑ Volver al Índice](#toc-container)


### Consideraciones Importantes

{% hint style="info" %}
**Opción `--noImplicitReturns`**: Reiteramos su importancia. Activada en `tsconfig.json`, esta opción te obliga a tener una sentencia `return` en todas las ramas posibles de una función que *se espera* que devuelva un valor (es decir, no tipada como `void` o `any`). Es una excelente red de seguridad contra errores lógicos donde olvidas devolver un valor en un `if/else` o `switch`. Las funciones que infieren `void` (porque no tienen `return`s que devuelvan valor) no activan esta regla, lo cual es el comportamiento deseado.
{% endhint %}

-   **Funciones Callback y `void`**: Al definir tipos para funciones que aceptan callbacks, el uso de `void` como tipo de retorno del callback es muy potente. Indica que *quien llama al callback* (la función de orden superior) **ignora** cualquier valor que el callback pueda devolver. Esto permite pasar callbacks que *sí* devuelven un valor a funciones que esperan un callback `void`, sin que TypeScript se queje.
    ```typescript
    // Función que espera un callback void
    function ejecutarCadaSegundo(callback: () => void): void {
      setInterval(() => {
        callback(); // Llamamos al callback, pero ignoramos cualquier posible valor devuelto
      }, 1000);
    }

    // Pasamos un callback que devuelve un número
    let contador = 0;
    ejecutarCadaSegundo(() => {
      contador++;
      console.log(`Tick número: ${contador}`);
      return contador; // Este valor es ignorado por ejecutarCadaSegundo
    });

    // Esto es útil para tipos como los de Array.prototype.forEach
    // forEach espera un callback de tipo (value: T, index: number, array: T[]) => void
    // Puedes pasarle una función que devuelva algo, pero forEach lo ignorará.
    const numeros = [1, 2, 3];
    numeros.forEach((num) => {
      console.log(num);
      return num * 2; // El valor devuelto (2, 4, 6) es ignorado por forEach
    });
    ```


[↑ Volver al Índice](#toc-container)


### Buenas Prácticas

-   **Tipado Explícito**: Aunque TypeScript infiere `void`, es una buena práctica tipar explícitamente como `void` (o `Promise<void>` para `async`) las funciones destinadas a realizar efectos secundarios sin devolver valor. Mejora la claridad y la intención del código.
-   **`return;` para Control de Flujo**: Usa `return;` sin valor para salidas tempranas en funciones `void` cuando sea necesario por lógica condicional (ej., validaciones de entrada).
-   **Habilitar `--noImplicitReturns`**: Configura esta opción en tu `tsconfig.json` para mejorar la robustez del código, asegurando que las funciones no-`void` siempre devuelvan algo.


[↑ Volver al Índice](#toc-container)


### Malas Prácticas

-   **`return` Accidental**: Escribir una función con la intención de que sea `void` pero accidentalmente incluir una sentencia `return algo;`. Si no has tipado explícitamente el retorno como `void`, TypeScript podría inferir un tipo diferente (`string`, `number`, etc.), ocultando tu intención original y potencialmente llevando a errores si alguien intenta usar ese valor inesperado.
-   **Usar `undefined` en lugar de `void`**: Diseñar funciones que devuelven `undefined` explícitamente (`return undefined;`) solo para indicar "nada que devolver". Esto es menos semántico y claro que usar `void`.
-   **Ignorar el Tipo `void` en Callbacks**: Asumir que un callback siempre devolverá `undefined` solo porque la función que lo recibe lo espera como `void`. El callback podría devolver un valor, y aunque sea ignorado por el llamador, entender esta distinción es importante para el diseño de APIs flexibles.
-   **Funciones `async` sin `Promise<void>`**: Olvidar el `Promise<>` en el tipo de retorno de una función `async` que no devuelve valor. Una función `async` *siempre* devuelve una Promise, por lo que el tipo correcto es `Promise<void>`, no simplemente `void`.


[↑ Volver al Índice](#toc-container)



</rewritten_file>



