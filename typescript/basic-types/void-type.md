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
- [Diferencias entre void y undefined en retornos](#diferencias-entre-void-y-undefined-en-retornos)
  - [Definición y Propósito](#definicion-y-proposito)
  - [Implicaciones en el Tipado](#implicaciones-en-el-tipado)
  - [Consideraciones Importantes](#consideraciones-importantes)
  - [Buenas Prácticas](#buenas-practicas)
  - [Malas Prácticas](#malas-practicas)
- [Uso en funciones sin retorno explícito](#uso-en-funciones-sin-retorno-explícito)
  - [Inferencia de Tipo void](#inferencia-de-tipo-void)
  - [Casos de Uso Comunes](#casos-de-uso-comunes)
  - [async y Promise](#async-y-promise)
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


### Implicaciones en el Tipado

La principal diferencia radica en la **intención** que comunican al compilador y a otros desarrolladores:

-   **`function miFuncion(): void { ... }`**: Esta firma declara inequívocamente: "Esta función realiza una acción, pero no esperes obtener un valor útil de ella. Si intentas usar su resultado, probablemente estés cometiendo un error".
    -   Aunque técnicamente podrías hacer `return undefined;` dentro de una función `void` (por compatibilidad histórica con JavaScript), **hacerlo es confuso y considerado una mala práctica en TypeScript**. Oscurece la intención de `void`.
    -   TypeScript *permite* asignar el resultado de una función `void` a una variable (ej: `const res = miFuncion();`), pero el tipo inferido para `res` será `void`, y su valor en tiempo de ejecución será `undefined`. Sin embargo, la propia declaración `void` **desaconseja fuertemente** este uso. El compilador no te impedirá hacerlo directamente, pero herramientas de linting más estrictas o revisiones de código deberían señalarlo.

-   **`function otraFuncion(): T | undefined { ... }`**: Esta firma indica que la función *puede* devolver un valor de tipo `T`, pero también *puede* devolver `undefined` como un resultado *significativo* y esperado en ciertos escenarios (por ejemplo, una función de búsqueda que devuelve el objeto encontrado o `undefined` si no lo encuentra). Aquí, `undefined` es un valor legítimo que el código invocante debe manejar.

[↑ Volver al Índice](#toc-container)


#### Ejemplos

```typescript
// Función explícitamente tipada como void: Realiza acción, no devuelve nada útil.
// Función explícitamente tipada como void
function mostrarMensaje(mensaje: string): void {
  console.log(mensaje);
  // No hay sentencia return, o solo `return;`
}

// TypeScript infiere void si no hay return
function saludar() { // Inferido como (): void
  console.log("Hola Mundo");
}

// Función que devuelve undefined implícitamente (estilo JavaScript)
function sinRetornoExplicitoJS() {
  // No hay return
}
const resultadoJS = sinRetornoExplicitoJS();
console.log(resultadoJS); // Output: undefined

// Función que devuelve undefined explícitamente
function devuelveUndefined(): undefined {
  return undefined;
}
const resultadoUnd = devuelveUndefined();
console.log(resultadoUnd); // Output: undefined

// Uso del resultado de una función void (desaconsejado)
const resultadoVoid = mostrarMensaje("Probando void");
console.log(resultadoVoid); // Output: undefined

// Alternativas a `undefined` explícito: Si una función necesita indicar la ausencia de un resultado válido, a menudo es mejor usar `null`, lanzar un error, o devolver un tipo unión como `T | null` o usar tipos de resultado (Result types) en lugar de devolver `undefined` explícitamente, ya que `undefined` suele asociarse más a errores o estados no inicializados.
```

[↑ Volver al Índice](#toc-container)


### Consideraciones Importantes

-   **Claridad**: Usar `void` mejora la legibilidad del código al dejar clara la intención de no devolver un valor. Evita ambigüedades sobre si la ausencia de un `return` fue intencional o un olvido.
-   **Compatibilidad con JavaScript**: La permisividad de TypeScript al permitir `return undefined;` en funciones `void` existe principalmente por razones de compatibilidad con código JavaScript existente. Sin embargo, en código TypeScript nuevo, es mejor evitarlo.
-   **Alternativas a `undefined` explícito**: Si una función necesita indicar la ausencia de un resultado válido, a menudo es mejor usar `null`, lanzar un error, o devolver un tipo unión como `T | null` o usar tipos de resultado (Result types) en lugar de devolver `undefined` explícitamente, ya que `undefined` suele asociarse más a errores o estados no inicializados.

[↑ Volver al Índice](#toc-container)


### Buenas Prácticas

{% hint style="success" %}
Tipar explícitamente como `void` las funciones destinadas a realizar efectos secundarios sin devolver valor, incluso si TypeScript podría inferirlo. Mejora la intención.
{% endhint %}

-   Utilizar `return;` para salidas tempranas en funciones `void` cuando sea necesario por lógica condicional.
-   Activar la opción `--noImplicitReturns` en `tsconfig.json` para asegurar que todas las funciones que deben devolver un valor lo hagan explícitamente en todas sus ramas.

[↑ Volver al Índice](#toc-container)


### Malas Prácticas

{% hint style="warning" %}
Declarar una función como `void` y luego asignarle su resultado a una variable para usarlo. Esto ignora la intención del tipado.
{% endhint %}

{% hint style="warning" %}
Usar `undefined` como tipo de retorno cuando la intención es simplemente indicar que no hay valor de retorno (usa `void` en ese caso).
{% endhint %}

-   Retornar `undefined` explícitamente de forma habitual como señal de "nada que devolver", en lugar de usar alternativas más expresivas como `null` o tipos unión.

[↑ Volver al Índice](#toc-container)


---

## Uso en funciones sin retorno explícito

TypeScript juega un papel importante en cómo se manejan las funciones que no tienen una sentencia `return` explícita o que solo usan `return;` sin un valor.

### Inferencia de Tipo `void`

Cuando una función en TypeScript no tiene ninguna sentencia `return` que devuelva un valor, o solo contiene `return;`, el compilador de TypeScript infiere automáticamente que su tipo de retorno es `void`. Esto difiere del comportamiento de JavaScript puro, donde dicha función simplemente devolvería `undefined`. La inferencia de `void` por parte de TypeScript añade una capa de seguridad y claridad semántica.

#### Ejemplos

```typescript
// TypeScript infiere (): void
function procesarDatos(datos: any[]) {
  console.log("Procesando...");
  datos.forEach(dato => console.log(dato));
  // No hay return
}

// Función flecha infiere void
const logEvento = (evento: string) => { // Infiere (evento: string) => void
  console.log(`Evento registrado: ${evento}`);
  // No hay return
};

// Función con return vacío también infiere void
function validarEntrada(entrada: string): void {
  if (!entrada) {
    console.error("Entrada vacía");
    return; // Salida temprana, infiere void
  }
  console.log("Entrada válida:", entrada);
}
```

[↑ Volver al Índice](#toc-container)


### Casos de Uso Comunes

Las funciones `void` son ubicuas en muchas aplicaciones, especialmente en tareas relacionadas con efectos secundarios:

-   **Manejadores de Eventos (Event Handlers)**: Funciones que responden a eventos del usuario (clics, teclas) o del sistema. Suelen modificar el estado de la aplicación o la UI, pero no devuelven un valor.
    ```typescript
    button.addEventListener('click', (): void => {
      console.log('Botón clickeado!');
      // Actualizar UI, etc.
    });
    ```
-   **Logging y Monitorización**: Funciones que registran información en la consola, en archivos o envían datos a servicios de monitorización.
    ```typescript
    function logError(error: Error, contexto: string): void {
      console.error(`[${contexto}]`, error);
      // enviarErrorAServicioExterno(error, contexto);
    }
    ```
-   **Manipulación del DOM**: Funciones que modifican directamente la estructura o estilos de la página web.
    ```typescript
    function actualizarContadorUI(nuevoValor: number): void {
      const contadorElement = document.getElementById('contador');
      if (contadorElement) {
        contadorElement.textContent = nuevoValor.toString();
      }
    }
    ```
-   **Funciones de Configuración Inicial**: Código que se ejecuta una vez para configurar partes de la aplicación.
    ```typescript
    function inicializarBaseDeDatos(): void {
      // Conectar, configurar tablas...
      console.log("Base de datos inicializada.");
    }
    ```

[↑ Volver al Índice](#toc-container)


### async y Promise<void>

Cuando trabajamos con funciones asíncronas (`async`), la interacción con `void` es ligeramente diferente pero sigue la misma filosofía.

-   Una función `async` **siempre** devuelve una `Promise`.
-   Si una función `async` está destinada a realizar una operación asíncrona pero no a devolver un valor útil una vez completada, su tipo de retorno debe ser `Promise<void>`.
-   Esto indica que la promesa se resolverá (eventualmente), pero no lo hará con un valor significativo. El código que espera (`await`) esta promesa obtendrá `undefined` cuando se resuelva, pero la firma `Promise<void>` comunica que este valor no debe ser utilizado.

#### Ejemplo `async/Promise<void>`

```typescript
async function procesoPrincipal(): Promise<void> {
  // Código del proceso principal
  console.log("Proceso principal completado.");
}

procesoPrincipal();
```

[↑ Volver al Índice](#toc-container)


### Consideraciones Importantes

{% hint style="info" %}
**Opción `--noImplicitReturns`**: Esta opción del compilador de TypeScript (`tsconfig.json`) es muy útil. Cuando está activada, fuerza a que todas las rutas de código en una función devuelvan un valor, *a menos que* la función esté explícitamente tipada como `void` (o `any`). Ayuda a prevenir errores donde se olvida un `return` en alguna rama lógica. Las funciones que infieren `void` porque no tienen `return`s no activan esta regla.
{% endhint %}

-   **Funciones Callback**: Al pasar funciones como callbacks, tiparlas correctamente con `void` es crucial si no se espera que devuelvan un valor. Si un callback *puede* devolver un valor, pero este no *debe* ser usado por quien lo llama, `void` sigue siendo apropiado en la firma del tipo que espera el callback.
    ```typescript
    function ejecutarCallback(callback: () => void): void {
      console.log("Ejecutando callback...");
      callback(); // No usamos el valor de retorno, incluso si lo hubiera
      console.log("Callback ejecutado.");
    }

    ejecutarCallback(() => {
      console.log("Dentro del callback");
      return 42; // Aunque retorna 42, ejecutarCallback lo ignora por el tipado void
    });
    ```

### Buenas Prácticas

-   Tipar explícitamente como `void` las funciones destinadas a realizar efectos secundarios sin devolver valor, incluso si TypeScript podría inferirlo. Mejora la intención.
-   Utilizar `return;` para salidas tempranas en funciones `void` cuando sea necesario por lógica condicional.
-   Activar la opción `--noImplicitReturns` en `tsconfig.json` para asegurar que todas las funciones que deben devolver un valor lo hagan explícitamente en todas sus ramas.

### Malas Prácticas

-   Escribir una función con la intención de que sea `void` pero accidentalmente incluir una sentencia `return` con un valor, sin haber tipado explícitamente el retorno como `void`. TypeScript puede inferir un tipo diferente y ocultar el error lógico.
-   Diseñar funciones que devuelven `undefined` explícitamente para indicar "nada que devolver" en lugar de usar `void`.
-   Ignorar el tipo `void` en callbacks y asumir que siempre devolverán `undefined`.

[↑ Volver al Índice](#toc-container)

