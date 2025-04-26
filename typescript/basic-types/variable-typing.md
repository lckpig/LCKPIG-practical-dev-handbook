<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/variable-typing)
<!-- MULTILANGUAJE MENU END -->

<!--
# Tipado en variables y constantes

- Declaración con `let`, `const` y su relación con los tipos
- Inferencia de tipos vs. anotaciones explícitas
-->

<details id="toc-container">
<summary>Índice de contenidos</summary>

<!-- no toc -->
- [Declaración con `let`, `const` y su relación con los tipos](#declaracion-con-let-const-y-su-relacion-con-los-tipos)
  - [Tabla Comparativa: `let` vs `const`](#tabla-comparativa-let-vs-const)
  - [Consideraciones Importantes sobre `const` con Objetos y Arrays](#consideraciones-importantes-sobre-const-con-objetos-y-arrays)
  - [Casos de Uso Reales](#casos-de-uso-reales)
  - [Buenas Prácticas](#buenas-practicas)
  - [Malas Prácticas](#malas-practicas)
- [Inferencia de tipos vs. anotaciones explícitas](#inferencia-de-tipos-vs-anotaciones-explicitas)
  - [Inferencia de Tipos](#inferencia-de-tipos)
  - [Ventajas de la Inferencia de Tipos](#ventajas-de-la-inferencia-de-tipos)
  - [Anotaciones Explícitas](#anotaciones-explicitas)
  - [Tabla Comparativa: Inferencia vs. Anotación Explícita](#tabla-comparativa-inferencia-vs-anotacion-explicita)
  - [Cuándo usar Anotaciones Explícitas](#cuando-usar-anotaciones-explicitas)
  - [Casos de Uso Reales (Inferencia vs. Anotación)](#casos-de-uso-reales-inferencia-vs-anotacion)
  - [Buenas Prácticas](#buenas-practicas-1)
  - [Malas Prácticas](#malas-practicas-1)
  - [Errores Comunes y Malentendidos](#errores-comunes-y-malentendidos)

</details>

# Tipado en variables y constantes

En TypeScript, la forma en que declaramos variables (`let`) y constantes (`const`) y cómo gestionamos sus tipos —ya sea mediante la inferencia automática del compilador o a través de anotaciones explícitas— son aspectos fundamentales. Estas decisiones no solo afectan la sintaxis, sino que tienen un impacto directo en la robustez, legibilidad y mantenibilidad del código. Entender las sutilezas de `let`, `const`, la inferencia y las anotaciones explícitas es crucial para escribir código TypeScript eficaz y seguro.

---

## Declaración con `let`, `const` y su relación con los tipos

TypeScript, al extender JavaScript, adopta `let` y `const` para la declaración de variables. Sin embargo, enriquece su significado dentro de su sistema de tipos estático. La elección entre `let` y `const` va más allá de la simple mutabilidad del *valor* asignado; influye decisivamente en cómo TypeScript *infiere* y *trata* el tipo asociado a esa variable o constante, afectando la precisión y la seguridad del tipado.

[↑ Volver al Índice](#toc-container)

### Tabla Comparativa: `let` vs `const`

La siguiente tabla resume las diferencias clave entre `let` y `const` en el contexto de TypeScript:

| Característica                    | `let`                                                               | `const`                                                                                              |
| :-------------------------------- | :------------------------------------------------------------------ | :--------------------------------------------------------------------------------------------------- |
| **Mutabilidad**                   | Permite la reasignación de la variable.                             | **No** permite la reasignación de la constante (la referencia es fija).                              |
| **Inferencia (Tipos Primitivos)** | Infiere un tipo "amplio" (ej. `number`, `string`, `boolean`).       | Infiere un tipo "literal estrecho" (ej. `3`, `"activo"`, `true`).                                    |
| **Inferencia (Objetos/Arrays)**   | Infiere tipos de propiedades/elementos (ej. `{ name: string }`).    | Infiere tipos de propiedades/elementos (ej. `{ name: string }`). **No** hace el contenido inmutable. |
| **Seguridad**                     | Menor seguridad contra reasignaciones accidentales.                 | Mayor seguridad, previene reasignaciones accidentales.                                               |
| **Intención**                     | Indica que el valor de la variable puede/necesita cambiar.          | Indica que la referencia a la variable es fija y no cambiará.                                        |
| **Caso de Uso Típico**            | Contadores, estados cambiantes, acumuladores, variables temporales. | Valores fijos, configuraciones, referencias a funciones, APIs inmutables.                            |

#### Ejemplos de Inferencia

```typescript
// Con let: Tipos amplios
let contadorVisitas = 0; // Infiere number
contadorVisitas = 10;    // OK

let estadoActual = "pendiente"; // Infiere string
estadoActual = "procesando"; // OK

// Con const: Tipos literales estrechos
const MAX_INTENTOS = 3; // Infiere el tipo literal 3
// MAX_INTENTOS = 4; // Error: No se puede reasignar una constante

const CODIGO_ERROR_POR_DEFECTO = "NOT_FOUND"; // Infiere el tipo literal "NOT_FOUND"
// CODIGO_ERROR_POR_DEFECTO = "UNAUTHORIZED"; // Error
```

**Relevancia de los Tipos Literales con `const`:** Los tipos literales inferidos por `const` son una herramienta poderosa. Permiten definir conjuntos muy específicos de valores permitidos, lo cual es fundamental para mejorar la seguridad y la expresividad del código, especialmente en combinación con tipos unión o para modelar estados específicos. Por ejemplo, `type Estado = "pendiente" | "procesando" | "completado"; const estadoInicial: Estado = "pendiente";`.

[↑ Volver al Índice](#toc-container)

### Consideraciones Importantes sobre `const` con Objetos y Arrays

Es fundamental entender una limitación clave de `const` cuando se aplica a objetos y arrays: `const` **solo garantiza la inmutabilidad de la asignación (la referencia) a la variable, no la inmutabilidad del contenido interno del objeto o array.**

Esto significa que, aunque no puedes reasignar la variable `const` para que apunte a un objeto o array diferente, sí puedes modificar las propiedades del objeto o añadir/eliminar/modificar elementos del array.

#### Ejemplo: Mutabilidad interna con `const`

```typescript
// Objeto con const: La referencia 'configuracion' es constante
const configuracion = { puerto: 8080, ssl: false };

// Esto provocaría un error:
// configuracion = { puerto: 3000, ssl: true }; // Error: Cannot assign to 'configuracion' because it is a constant.

// Pero esto es perfectamente válido y modifica el objeto original:
configuracion.puerto = 3001; // Modificando una propiedad
configuracion.ssl = true;    // Modificando otra propiedad
console.log(configuracion); // Salida: { puerto: 3001, ssl: true }

// Array con const: La referencia 'permisos' es constante
const permisos = ["leer", "escribir"];

// Esto provocaría un error:
// permisos = ["admin"]; // Error: Cannot assign to 'permisos' because it is a constant.

// Pero esto es válido y modifica el array original:
permisos.push("eliminar"); // Añadiendo un elemento
permisos[0] = "read";     // Modificando un elemento existente
console.log(permisos);    // Salida: [ 'read', 'escribir', 'eliminar' ]
```

{% hint style="warning" %}
**`const` no implica Inmutabilidad Profunda:** Si necesitas asegurar que ni las propiedades de un objeto ni los elementos de un array puedan ser modificados después de su creación (inmutabilidad profunda), `const` por sí solo no es suficiente. Deberás recurrir a mecanismos adicionales como:
*   El modificador `readonly` de TypeScript para propiedades de objetos y arrays (`readonly string[]` o `ReadonlyArray<string>`).
*   Tipos de utilidad como `Readonly<T>` para objetos.
*   El método `Object.freeze()` de JavaScript (aunque su efecto no es rastreado por el sistema de tipos de TypeScript de forma predeterminada).
*   Librerías de inmutabilidad como Immer o Immutable.js.
{% endhint %}

Comprender esta distinción es vital para evitar errores derivados de asumir una inmutabilidad total donde no existe.

[↑ Volver al Índice](#toc-container)

### Casos de Uso Reales

La elección entre `let` y `const` debe basarse en la necesidad real de reasignación:

*   **Cuándo usar `let` (Necesidad de Reasignación):**
    *   **Contadores e Iteradores:** En bucles `for` o `while` donde el valor de la variable cambia en cada iteración (`for (let i = 0; ...)`).
    *   **Acumuladores:** Variables que acumulan un valor a lo largo de un proceso (`let sumaTotal = 0; ... sumaTotal += valor;`).
    *   **Variables de Estado:** En componentes de interfaz de usuario (React, Vue, Angular, etc.) para representar estados que cambian con interacciones o eventos (`let isLoading = true; ... isLoading = false;`).
    *   **Variables Temporales:** Dentro de funciones para almacenar resultados intermedios que pueden ser modificados antes del retorno final (`let resultadoTemporal = operacionInicial(); if (condicion) resultadoTemporal = otroValor;`).
*   **Cuándo usar `const` (Referencia Fija):**
    *   **Valores Fijos y Constantes:** Definición de valores que no deben cambiar durante la ejecución (constantes matemáticas, configuraciones fijas, umbrales: `const PI = 3.14159;`, `const TIMEOUT_MS = 5000;`).
    *   **Referencias a Funciones:** Es la forma estándar y recomendada para declarar funciones mediante expresiones de función (`const calcularImpuesto = (monto: number): number => { ... };`). Esto previene la reasignación accidental de la función.
    *   **Referencias a Objetos/Arrays cuyo contenido puede cambiar:** Cuando necesitas una referencia estable a un objeto o array, pero su contenido interno es mutable (configuraciones dinámicas, listas de elementos que se modifican: `const opcionesUsuario = { tema: 'oscuro' };`, `const tareasPendientes = [];`).
    *   **Referencias a Elementos del DOM:** Cuando obtienes una referencia a un elemento HTML que no será reemplazado (`const botonEnviar = document.getElementById('submit-btn');`).

[↑ Volver al Índice](#toc-container)

### Buenas Prácticas

*   **Priorizar `const` por Defecto:** Adopta la costumbre de declarar todas las variables con `const` inicialmente. Cámbialo a `let` **solo** si identificas una necesidad clara y justificada de reasignar la variable.
    *   **Beneficios:** Fomenta la inmutabilidad (de la referencia), hace el código más predecible y fácil de razonar, previene errores por reasignaciones accidentales y comunica claramente la intención de que un valor no debe cambiar.

{% hint style="info" %}
Usar `const` como opción predeterminada no es solo una preferencia estilística, es una estrategia que mejora la robustez y la claridad del código. Señala explícitamente qué referencias se espera que permanezcan constantes.
{% endhint %}

*   **`let` con Propósito:** Utiliza `let` únicamente cuando la reasignación de la variable sea una parte intencional y necesaria de la lógica del programa. Si una variable declarada con `let` nunca se reasigna, es una señal de que debería ser `const`.

[↑ Volver al Índice](#toc-container)

### Malas Prácticas

*   **Abuso de `let`:** Declarar variables con `let` cuando nunca se reasignan. Esto introduce "ruido" semántico, sugiriendo una mutabilidad que no existe y dificultando la comprensión del flujo de datos y las intenciones del programador.
    ```typescript
    // Mala práctica: 'nombreUsuario' nunca cambia, debería ser const.
    let nombreUsuario = obtenerNombreDesdeAPI();
    console.log(`Bienvenido, ${nombreUsuario}`);

    // Buena práctica:
    const nombreUsuarioConst = obtenerNombreDesdeAPI();
    console.log(`Bienvenido, ${nombreUsuarioConst}`);
    ```
*   **Confiar en `const` para Inmutabilidad Profunda:** Asumir erróneamente que `const` previene modificaciones internas en objetos o arrays. Esto puede llevar a bugs difíciles de rastrear si otra parte del código modifica inesperadamente el contenido de una estructura de datos que se creía inmutable. Utiliza las técnicas mencionadas anteriormente (`readonly`, `Object.freeze`, etc.) si necesitas inmutabilidad real del contenido.

[↑ Volver al Índice](#toc-container)

---

## Inferencia de tipos vs. anotaciones explícitas

TypeScript brilla por su capacidad para *inferir* tipos automáticamente en muchos contextos, reduciendo la verbosidad del código. Sin embargo, también proporciona un mecanismo robusto para declarar tipos *explícitamente* mediante anotaciones (`:`), lo cual es esencial para la claridad, la seguridad y el diseño de APIs robustas. Equilibrar cuándo confiar en la inferencia y cuándo ser explícito es clave para un buen código TypeScript.

[↑ Volver al Índice](#toc-container)

### Inferencia de Tipos

La inferencia de tipos ocurre cuando TypeScript deduce el tipo de una variable o expresión basándose en el valor asignado o el contexto de uso, sin necesidad de una anotación explícita. Esto es especialmente común durante la inicialización de variables.

#### Ejemplo: Inferencia de tipos en acción

```typescript
// Inferencia con primitivos (usando let o const)
let cantidad = 100;          // TypeScript infiere 'number'
const mensaje = "Procesando..."; // TypeScript infiere el tipo literal "Procesando..."
let completado = false;      // TypeScript infiere 'boolean'

// Inferencia con objetos y arrays (estructura inferida)
const punto = { x: 10, y: 20 }; // Infiere { x: number; y: number; }
const nombres = ["Ana", "Luis", "Elena"]; // Infiere string[] (array de strings)

// Inferencia del tipo de retorno de funciones
function sumar(a: number, b: number) { // Los parámetros NECESITAN anotación
  return a + b; // TypeScript infiere que el tipo de retorno es 'number'
}
const resultadoSuma = sumar(5, 3); // TypeScript infiere 'number' para resultadoSuma

// Inferencia en desestructuración
const { x, y } = punto; // 'x' e 'y' infieren 'number'
const [primerNombre] = nombres; // 'primerNombre' infiere 'string'
```

La inferencia funciona bien para tipos simples y estructuras de datos inicializadas directamente, haciendo el código más limpio en esos casos.

[↑ Volver al Índice](#toc-container)

### Ventajas de la Inferencia de Tipos

*   **Concisión:** Reduce significativamente la cantidad de código redundante. `let nombre = "X";` es más directo que `let nombre: string = "X";`.
*   **Legibilidad (en casos simples):** Para inicializaciones obvias con valores primitivos o estructuras sencillas, la ausencia de anotaciones explícitas puede hacer el código menos recargado y más fácil de leer.
*   **Mantenimiento (Refactorización):** Si cambias el valor inicial de una variable inferida (p.ej., de `0` a `null`), TypeScript puede ajustar el tipo inferido (aunque esto requiere cuidado, especialmente con `strictNullChecks`).

[↑ Volver al Índice](#toc-container)

### Anotaciones Explícitas

Las anotaciones explícitas (`variable: Tipo`) se utilizan para declarar de forma inequívoca el tipo esperado para una variable, parámetro de función, propiedad de objeto o valor de retorno. Son indispensables en situaciones donde la inferencia es imposible, insuficiente o ambigua.

#### Ejemplo: Uso de anotaciones explícitas

```typescript
// 1. Declaración sin inicialización
let userId: number; // Necesario: TypeScript no puede inferir sin valor inicial
userId = 12345;
// userId = "id-abc"; // Error: Tipo 'string' no asignable a 'number'.

// 2. Funciones (parámetros y retorno) - ESENCIAL
function procesarPedido(idPedido: string, cantidad: number): boolean { // Anotaciones en params y retorno
  console.log(`Procesando pedido ${idPedido} con ${cantidad} unidades.`);
  // ... lógica ...
  return true; // Asegura que la función devuelve un booleano
}

// 3. Tipos complejos (uniones, intersecciones, etc.)
let respuestaApi: string | null; // Tipo unión: Explícito para claridad
respuestaApi = await fetchDatos();
if (typeof respuestaApi === 'string') { /* ... */ }

type Coordenadas = { lat: number; lon: number };
let ubicacionActual: Coordenadas; // Anotación con un type alias
ubicacionActual = { lat: 40.7128, lon: -74.0060 };

// 4. Clarificar intención o restringir la inferencia
//    La inferencia podría ser demasiado amplia (HTMLElement | null)
const miBoton: HTMLButtonElement | null = document.getElementById("miBoton");

// 5. Uso peligroso de 'any' (evitar siempre que sea posible)
let configuracionUsuario: any; // Desactiva la comprobación de tipos. ¡Último recurso!
configuracionUsuario = obtenerConfig();
// configuracionUsuario.propiedadInexistente.hacerAlgo(); // Error en tiempo de ejecución, no de compilación

// 6. Usar 'unknown' (alternativa segura a 'any')
let valorDesconocido: unknown;
valorDesconocido = obtenerValorExterno();
if (typeof valorDesconocido === 'string') {
  // Es seguro usarlo como string dentro de este bloque
  console.log(valorDesconocido.toUpperCase());
}
// console.log(valorDesconocido.toUpperCase()); // Error: 'valorDesconocido' es de tipo 'unknown'.
```

[↑ Volver al Índice](#toc-container)

### Tabla Comparativa: Inferencia vs. Anotación Explícita

| Aspecto           | Inferencia de Tipos                                                                                      | Anotación Explícita (`: Tipo`)                                                                                           |
| :---------------- | :------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------- |
| **Definición**    | El compilador deduce el tipo automáticamente.                                                            | El programador declara el tipo explícitamente.                                                                           |
| **Cuándo Ocurre** | Principalmente en inicialización de variables/constantes, retornos de función simples.                   | Declaraciones sin inicialización, parámetros/retorno de funciones, tipos complejos, claridad.                            |
| **Pros**          | Concisión, legibilidad (simple), menos código.                                                           | Claridad, seguridad, documentación, manejo de tipos complejos, diseño de APIs.                                           |
| **Contras**       | Puede ser ambiguo, puede inferir `any` (si no hay `noImplicitAny`), puede ser demasiado amplio/estrecho. | Más verboso, puede ser redundante en casos obvios.                                                                       |
| **Seguridad**     | Depende de la calidad de la inferencia y `tsconfig`.                                                     | Máxima seguridad si se usa correctamente.                                                                                |
| **Recomendación** | Usar para inicializaciones obvias y simples.                                                             | **Obligatorio** para firmas de función, variables no inicializadas, tipos complejos, y donde la claridad sea primordial. |

[↑ Volver al Índice](#toc-container)

### Cuándo usar Anotaciones Explícitas

Es crucial anotar tipos explícitamente en los siguientes escenarios:

1.  **Declaración sin Inicialización:** Si una variable se declara sin un valor inicial (`let nombre;`), TypeScript no puede inferir su tipo (a menudo infiere `any` implícitamente si `noImplicitAny` está desactivado). La anotación es **obligatoria** para garantizar la seguridad de tipos (`let nombre: string;`).
2.  **Parámetros de Función:** **SIEMPRE** debes anotar los tipos de los parámetros de las funciones (`function saludar(nombre: string) {...}`). Esto define el contrato de la función y es esencial para la verificación de tipos en las llamadas.
3.  **Valor de Retorno de Función:** Aunque TypeScript a menudo puede inferir el tipo de retorno, es una **muy buena práctica** anotarlo explícitamente (`function sumar(a: number, b: number): number {...}`). Esto mejora la legibilidad, actúa como documentación y asegura que la función devuelva el tipo esperado, detectando errores si la lógica interna cambia accidentalmente el tipo de retorno.
4.  **Tipos Complejos:** Cuando trabajas con tipos unión (`string | number`), tipos de intersección (`A & B`), genéricos (`Array<T>`), tuplas (`[string, number]`) o tipos condicionales, las anotaciones explícitas son necesarias para definir y utilizar correctamente estas estructuras.
5.  **Cuando la Inferencia no es Suficiente o Deseada:**
    *   Si TypeScript infiere un tipo demasiado amplio (p.ej., infiere `string | number` cuando tú sabes que solo será `number` en un contexto específico).
    *   Si infiere un tipo demasiado estrecho (p.ej., infiere un tipo literal cuando necesitas un tipo más general como `string`).
    *   En contextos ambiguos donde la intención debe ser clara.
6.  **Definición de APIs y Límites de Módulos:** Al definir la "forma" (shape) de los objetos que tu código espera recibir (p.ej., de una API externa) o que expone a otros módulos, usar `interface` o `type` con anotaciones explícitas es fundamental para establecer contratos claros.
7.  **Claridad y Documentación:** En algoritmos complejos, lógica de negocio crítica o código que será mantenido por otros, las anotaciones explícitas sirven como documentación auto-verificable, mejorando la comprensión y reduciendo errores.

#### Ejemplo: Clarificando la intención con anotaciones

```typescript
// Escenario: Función que puede devolver un objeto Usuario o null si no lo encuentra.
// La inferencia por sí sola podría ser problemática o ambigua.
type DatosUsuario = { id: number; nombre: string; email?: string }; // Definimos la forma explícitamente

let usuarioActual: DatosUsuario | null = null; // Anotación explícita del tipo unión

async function cargarUsuario(id: number): Promise<void> { // Anotación explícita del retorno (Promise<void>)
  try {
    const respuesta = await fetch(`/api/usuarios/${id}`);
    if (respuesta.ok) {
      // Asumimos que la API devuelve algo compatible con DatosUsuario
      const datos: DatosUsuario = await respuesta.json(); // Anotamos el tipo esperado de la API
      usuarioActual = datos;
    } else {
      usuarioActual = null; // Asignación explícita de null
    }
  } catch (error) {
    console.error("Error al cargar usuario:", error);
    usuarioActual = null; // Manejo de error, asignando null
  }
}
```

[↑ Volver al Índice](#toc-container)

### Casos de Uso Reales (Inferencia vs. Anotación)

*   **Favorecer Inferencia para:**
    *   Variables locales simples inicializadas directamente: `let contador = 0;`, `const mensajeExito = "Operación completada";`.
    *   Constantes con valores primitivos obvios: `const IVA = 0.21;`.
    *   Valores de retorno inferidos de funciones muy simples y obvias (aunque anotar sigue siendo buena práctica): `function esPositivo(n: number) { return n > 0; } // Infiere boolean`.
*   **Exigir Anotaciones Explícitas para:**
    *   **Firmas de funciones (parámetros y retorno):** Indispensable para la robustez.
    *   **Variables no inicializadas:** Obligatorio.
    *   **Propiedades de objetos en interfaces y tipos:** `interface Configuracion { timeout: number; reintentos?: number; }`.
    *   **Variables que pueden ser `null` o `undefined`:** `let resultado: ResultadoOperacion | null = null;`.
    *   **Tipos unión, intersección, genéricos:** `type ID = string | number;`, `let lista: Array<Producto>;`.
    *   **Datos de APIs externas:** Para definir la estructura esperada.
    *   **Código complejo o crítico:** Para mejorar la claridad y mantenibilidad.

[↑ Volver al Índice](#toc-container)

### Buenas Prácticas

*   **Confiar en la Inferencia Inteligente:** No añadas anotaciones explícitas donde TypeScript puede inferir el tipo de forma clara, correcta y no ambigua (p.ej., `let edad = 30;`). Evita el "ruido" innecesario. El objetivo es código claro y seguro, no verboso porque sí.
*   **Ser Explícito Donde Aporta Valor:** Usa anotaciones sin dudar donde la claridad, la seguridad o la definición de contratos lo requieran (firmas de función, tipos complejos, variables no inicializadas, límites de API).
*   **Activar Opciones Estrictas del Compilador (`tsconfig.json`):**
    *   `"noImplicitAny": true`: **FUNDAMENTAL**. Evita que TypeScript infiera `any` silenciosamente. Te obliga a ser explícito cuando la inferencia falla.
    *   `"strictNullChecks": true`: **MUY RECOMENDADO**. Trata `null` y `undefined` como tipos distintos y te obliga a manejarlos explícitamente (p.ej., con tipos unión `T | null` y comprobaciones), previniendo errores comunes en tiempo de ejecución.
    *   Otras opciones `strict` (`strictBindCallApply`, `strictFunctionTypes`, etc.) también incrementan la seguridad.

{% hint style="success" %}
Configurar `"strict": true` (que habilita `noImplicitAny`, `strictNullChecks` y otras) en tu `tsconfig.json` es la base para aprovechar al máximo la seguridad de tipos de TypeScript y escribir código más robusto.
{% endhint %}

*   **Evitar `any` Explícito Siempre que Sea Posible:** El tipo `any` es una "escotilla de escape" que desactiva la comprobación de tipos para esa variable. Debe ser el último recurso.
    *   **Alternativas más seguras a `any`:**
        *   **`unknown`:** Representa un valor de tipo desconocido. TypeScript te obliga a realizar comprobaciones de tipo (con `typeof`, `instanceof`, type guards) antes de poder operar con el valor. Es la alternativa preferida a `any` cuando realmente no conoces el tipo.
        *   **Tipos Genéricos:** Para escribir código reutilizable que funcione con diferentes tipos de forma segura.
        *   **Tipos Unión/Intersección:** Para modelar estructuras de datos más complejas o variables que pueden tener varios tipos definidos.

[↑ Volver al Índice](#toc-container)

### Malas Prácticas

*   **Anotaciones Redundantes:** Añadir anotaciones donde la inferencia es obvia y correcta (`let nombre: string = "Ana";`). Solo añade verbosidad sin aportar valor.
*   **Abuso de `any`:** Utilizar `any` indiscriminadamente para "silenciar" errores del compilador o por pereza. Esto socava completamente el propósito de usar TypeScript y esconde potenciales errores que aparecerán en tiempo de ejecución. Cada `any` debería justificarse y documentarse.

{% hint style="danger" %}
El uso excesivo de `any` es un "code smell" en TypeScript. Indica a menudo una falta de comprensión del sistema de tipos o una evasión de la tarea de tipar correctamente el código. Si te encuentras usando `any` frecuentemente, detente y considera si hay una forma más segura y explícita de modelar tus tipos.
{% endhint %}

*   **No Anotar Parámetros y Retornos de Función:** Dejar que los parámetros o retornos sean `any` implícitos (si `noImplicitAny` está desactivado) o inferidos incorrectamente es una fuente común de errores. Las firmas de función explícitas son cruciales.
*   **Uso Excesivo o Incorrecto de Type Assertions (`as`):** Las aserciones de tipo (`valor as Tipo`) le dicen al compilador "confía en mí, sé que este valor es de este tipo", **desactivando la comprobación para esa operación**. Son útiles en casos contados donde sabes más que el compilador, pero si te equivocas, tendrás errores en tiempo de ejecución. Prefiere siempre type guards (como `if (valor instanceof Tipo)`) o validaciones explícitas antes que aserciones.

[↑ Volver al Índice](#toc-container)

### Errores Comunes y Malentendidos

*   **Confundir `const` con Inmutabilidad Profunda:** Reiterado por su frecuencia. `const` solo protege la *asignación* de la variable, no el *contenido* de objetos/arrays.
*   **Problemas con `this`:** Aunque relacionado con JavaScript en general, la forma de declarar funciones (función normal vs. función flecha) interactúa con `let`/`const` y el contexto de `this`. Las funciones flecha (`=>`) suelen ser preferibles para métodos de clase o callbacks para preservar el `this` léxico esperado.
*   **Uso Incorrecto de Type Assertions (`as Tipo` o `<Tipo>valor`):** Usarlas para forzar tipos sin una verificación real. Es una forma de decirle al compilador que ignore un posible error, lo cual es peligroso.
    ```typescript
    // Peligroso si el elemento no existe o no es un input
    const valorInput = (document.getElementById('miInput') as HTMLInputElement).value;

    // Forma más segura usando Type Guard (instanceof)
    const elemento = document.getElementById('miInput');
    if (elemento instanceof HTMLInputElement) {
      // Dentro de este bloque, TypeScript sabe que 'elemento' es HTMLInputElement
      const valorInputSeguro = elemento.value;
      console.log(valorInputSeguro);
    } else {
      console.error("Elemento 'miInput' no encontrado o no es un input.");
    }
    ```
*   **Manejo Inadecuado de `null`/`undefined` (sin `strictNullChecks`):** Si `strictNullChecks` está desactivado (`false`) en `tsconfig.json`, `null` y `undefined` pueden asignarse implícitamente a cualquier tipo, ocultando errores de "null pointer" o "undefined property" que explotarán en ejecución. **Activar `strictNullChecks` es esencial** y obliga a manejar estos casos explícitamente.
    ```typescript
    // Con strictNullChecks: true (Recomendado)
    let nombre: string | null = obtenerNombreOPcional();
    if (nombre) { // Comprobación necesaria
      console.log(nombre.toUpperCase());
    }

    // Con strictNullChecks: false (Peligroso)
    // let nombre: string = obtenerNombreOPcional(); // Podría asignar null a string
    // console.log(nombre.toUpperCase()); // Error en ejecución si nombre es null
    ```


[↑ Volver al Índice](#toc-container)