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
  - [`let`: Variables Mutables y Tipos Amplios](#let-variables-mutables-y-tipos-amplios)
  - [`const`: Constantes y Tipos Literales Estrechos](#const-constantes-y-tipos-literales-estrechos)
  - [Consideraciones Importantes](#consideraciones-importantes)
  - [Casos de Uso Reales](#casos-de-uso-reales)
  - [Buenas Prácticas](#buenas-practicas)
  - [Malas Prácticas](#malas-practicas)
- [Inferencia de tipos vs. anotaciones explícitas](#inferencia-de-tipos-vs-anotaciones-explicitas)
  - [Inferencia de Tipos](#inferencia-de-tipos)
  - [Ventajas de la Inferencia de Tipos](#ventajas-de-la-inferencia-de-tipos)
  - [Anotaciones Explícitas](#anotaciones-explicitas)
  - [Cuándo usar Anotaciones Explícitas](#cuando-usar-anotaciones-explicitas)
  - [Casos de Uso Reales](#casos-de-uso-reales-1)
  - [Buenas Prácticas](#buenas-practicas-1)
  - [Malas Prácticas](#malas-practicas-1)
  - [Errores Comunes](#errores-comunes)

</details>

# Tipado en variables y constantes

En TypeScript, la forma en que declaramos variables y constantes (`let` vs `const`) y cómo especificamos sus tipos (inferencia vs anotación explícita) son decisiones fundamentales que impactan directamente la robustez, legibilidad y mantenibilidad de nuestro código. Comprender estas opciones y sus implicaciones es crucial para aprovechar al máximo las capacidades de TypeScript.

---

## Declaración con `let`, `const` y su relación con los tipos

TypeScript, al ser un superconjunto de JavaScript, hereda las palabras clave `let` y `const` para la declaración de variables. Sin embargo, les añade una capa de significado relacionada con el sistema de tipos. La elección entre una y otra no solo afecta la mutabilidad del *valor* asignado, sino también cómo TypeScript *infiere* y *trata* el tipo de esa variable.

### `let`: Variables Mutables y Tipos Amplios

Cuando declaramos una variable usando `let` y la inicializamos con un valor primitivo (como un número, una cadena o un booleano) sin especificar explícitamente su tipo, TypeScript adopta una postura conservadora e infiere un tipo "amplio" (widened type). Asume que, dado que la variable *puede* cambiar su valor en el futuro (porque se declaró con `let`), podría asignársele *cualquier otro* valor del mismo tipo primitivo.

#### Ejemplo: Inferencia de tipo con `let`

```typescript
let contadorVisitas = 0; // TypeScript infiere 'number'
// Podemos reasignar cualquier otro número
contadorVisitas = 10;
contadorVisitas = contadorVisitas + 1;

let estadoActual = "pendiente"; // TypeScript infiere 'string'
// Podemos reasignar cualquier otra cadena
estadoActual = "procesando";
estadoActual = "completado";

let mostrarDetalles = false; // TypeScript infiere 'boolean'
// Podemos reasignar true o false
mostrarDetalles = true;
```
**Relevancia:** Esta flexibilidad es útil para variables que, por naturaleza, necesitan cambiar su valor durante la ejecución del programa, como contadores, acumuladores, o indicadores de estado que evolucionan.

[↑ Volver al Índice](#toc-container)


### `const`: Constantes y Tipos Literales Estrechos

En contraste, cuando usamos `const`, le estamos diciendo a TypeScript que el valor asignado a esta variable será *constante* y *nunca* cambiará después de la inicialización. Aprovechando esta garantía de inmutabilidad, TypeScript infiere el tipo más específico posible: un *tipo literal* (literal type). En lugar de inferir simplemente `number`, inferirá el número exacto; en lugar de `string`, el valor exacto de la cadena.

#### Ejemplo: Inferencia de tipo con `const`

```typescript
const MAX_INTENTOS = 3; // TypeScript infiere el tipo literal 3
// MAX_INTENTOS = 4; // Error: Cannot assign to 'MAX_INTENTOS' because it is a constant.

const CODIGO_ERROR_POR_DEFECTO = "NOT_FOUND"; // TypeScript infiere el tipo literal "NOT_FOUND"
// CODIGO_ERROR_POR_DEFECTO = "UNAUTHORIZED"; // Error

const MODO_DEBUG = false; // TypeScript infiere el tipo literal false
// MODO_DEBUG = true; // Error
```
**Relevancia:** Los tipos literales son extremadamente útiles porque permiten crear conjuntos muy específicos de valores permitidos, lo que mejora la seguridad y la expresividad del código, especialmente cuando se combinan con tipos unión.

[↑ Volver al Índice](#toc-container)


### Consideraciones Importantes

- **Mutabilidad vs. Tipado:** La diferencia clave es la mutabilidad. `let` implica potencial cambio y lleva a tipos más generales. `const` garantiza inmutabilidad de la asignación y permite tipos literales precisos.
- **Seguridad y Predictibilidad:** Usar `const` por defecto incrementa la seguridad al prevenir reasignaciones accidentales. Hace el código más predecible, ya que sabes que el valor asociado a un identificador `const` no cambiará.
- **`const` con Objetos y Arrays:** ¡Este es un punto crucial y fuente común de confusión! `const` aplicado a un objeto o array **solo hace inmutable la referencia (la asignación) a ese objeto/array, no su contenido interno**. Puedes seguir modificando las propiedades del objeto o los elementos del array.

#### Ejemplo: Mutabilidad de objetos y arrays con `const`

```typescript
// La referencia 'configuracion' es constante
const configuracion = { puerto: 8080, ssl: false };

// Esto daría error:
// configuracion = { puerto: 3000, ssl: true }; // Error: Cannot assign to 'configuracion'...

// Pero esto es perfectamente válido:
configuracion.puerto = 3001; // Modificando una propiedad interna
configuracion.ssl = true;
console.log(configuracion); // { puerto: 3001, ssl: true }

// La referencia 'permisos' es constante
const permisos = ["leer", "escribir"];

// Esto daría error:
// permisos = ["admin"]; // Error: Cannot assign to 'permisos'...

// Pero esto es válido:
permisos.push("eliminar"); // Modificando el contenido del array
permisos[0] = "read"; // Modificando un elemento existente
console.log(permisos); // [ 'read', 'escribir', 'eliminar' ]
```

{% hint style="warning" %}
**No confundas `const` con Inmutabilidad Profunda:** Si necesitas garantizar que ni las propiedades de un objeto ni los elementos de un array puedan ser modificados, `const` por sí solo no es suficiente. Necesitarás usar tipos `readonly` de TypeScript o utilidades como `Object.freeze()`.
{% endhint %}


[↑ Volver al Índice](#toc-container)


### Casos de Uso Reales

- **`let` para Estado Cambiante:**
    - Contadores en bucles: `for (let i = 0; i < 10; i++) { ... }`
    - Acumuladores: `let sumaTotal = 0; items.forEach(item => sumaTotal += item.valor);`
    - Estado en componentes UI (React, Vue, Angular): Variables que guardan si un modal está abierto, el valor de un input, etc. `let isLoading = true; ... isLoading = false;`
    - Variables temporales dentro de funciones: `let resultadoTemporal = operacionCompleja();`
- **`const` para Valores Fijos:**
    - Constantes matemáticas o de configuración: `const SEGUNDOS_EN_MINUTO = 60;`
    - Referencias a elementos del DOM que no cambian: `const botonEnviar = document.getElementById('submit-btn');`
    - Funciones (las expresiones de función asignadas a `const` son la norma): `const calcularImpuesto = (monto: number): number => { ... };`
    - Objetos de configuración que no deben ser reasignados: `const API_CONFIG = { baseUrl: '/api', timeout: 5000 };`

[↑ Volver al Índice](#toc-container)


### Buenas Prácticas

- **Prioriza `const`:** Haz de `const` tu opción por defecto. Declara todo con `const` inicialmente y cámbialo a `let` solo si te das cuenta de que necesitas reasignar la variable. Esto fomenta la inmutabilidad y hace el código más fácil de razonar.

{% hint style="info" %}
Usar `const` por defecto no solo previene errores de reasignación accidental, sino que también señaliza claramente a otros desarrolladores (y a ti mismo en el futuro) que un valor particular no debería cambiar.
{% endhint %}

- **`let` solo cuando sea necesario:** Usa `let` exclusivamente para variables cuyo valor *necesita* cambiar explícitamente durante su ciclo de vida.

[↑ Volver al Índice](#toc-container)


### Malas Prácticas

- **Abuso de `let`:** Usar `let` para variables que nunca se reasignan. Esto introduce ruido y sugiere una mutabilidad innecesaria, haciendo más difícil entender el flujo de datos.
  ```typescript
  // Mal: 'nombreUsuario' nunca se reasigna, debería ser const
  let nombreUsuario = obtenerNombre();
  console.log(`Bienvenido, ${nombreUsuario}`);
  ```
- **Asumir inmutabilidad profunda con `const`:** Depender de `const` para prevenir cambios internos en objetos o arrays. Esto puede llevar a errores si otra parte del código modifica el contenido inesperadamente.


[↑ Volver al Índice](#toc-container)


---

## Inferencia de tipos vs. anotaciones explícitas

Una de las características más potentes de TypeScript es su capacidad para *inferir* tipos. Sin embargo, también nos permite ser *explícitos* mediante anotaciones cuando sea necesario o beneficioso.

### Inferencia de Tipos

En muchos casos, especialmente durante la inicialización de variables, TypeScript puede deducir el tipo de una variable basándose en el valor que se le asigna. Esto hace que el código sea más conciso sin sacrificar la seguridad de tipos.

#### Ejemplo: Inferencia de tipos

```typescript
let cantidad = 100; // TypeScript infiere 'number' correctamente
let mensaje = "Procesando..."; // TypeScript infiere 'string'
let completado = false; // TypeScript infiere 'boolean'

// La inferencia funciona bien con objetos y arrays simples
const punto = { x: 10, y: 20 }; // Infiere { x: number; y: number; }
const nombres = ["Ana", "Luis", "Elena"]; // Infiere string[]

// También infiere el tipo de retorno de funciones simples
function sumar(a: number, b: number) { // Infiere que retorna 'number'
  return a + b;
}
const resultadoSuma = sumar(5, 3); // Infiere 'number' para resultadoSuma
```

[↑ Volver al Índice](#toc-container)


### Ventajas de la Inferencia de Tipos

- **Concisión:** Reduce la cantidad de código a escribir y leer. `let nombre = "X";` es más corto que `let nombre: string = "X";`.
- **Legibilidad (en casos simples):** Para inicializaciones obvias, la inferencia evita el ruido visual de las anotaciones explícitas.
- **Mantenimiento:** Si el valor inicial cambia de una manera que implica un cambio de tipo (dentro de lo razonable), TypeScript a menudo ajustará la inferencia.

[↑ Volver al Índice](#toc-container)


### Anotaciones Explícitas

Hay situaciones donde la inferencia no es posible, no es deseable, o donde ser explícito mejora la claridad y la intención del código. Usamos la sintaxis de dos puntos (`:`) seguida del tipo.

#### Ejemplo: Anotaciones explícitas

```typescript
let userId: number; // Variable declarada pero no inicializada. Anotación necesaria.
userId = 12345;
// userId = "id-abc"; // Error: Type 'string' is not assignable to type 'number'.

let respuestaApi: string | null; // Tipo unión: puede ser string o null. Mejor ser explícito.
respuestaApi = await fetchDatos();
if (respuestaApi) { /* ... */ }

let configuracionUsuario: any; // ¡Peligro! Anotación explícita de 'any'. Evitar si es posible.
configuracionUsuario = obtenerConfig(); // Desactiva la comprobación de tipos.

// Anotaciones en parámetros de función (ESENCIAL)
function procesarPedido(idPedido: string, cantidad: number): boolean {
  console.log(`Procesando pedido ${idPedido} con ${cantidad} unidades.`);
  // ... lógica de procesamiento ...
  return true; // Valor de retorno explícitamente indicado como boolean
}

// Anotación para tipos más complejos o específicos
type Coordenadas = { lat: number; lon: number };
let ubicacionActual: Coordenadas;
ubicacionActual = { lat: 40.7128, lon: -74.0060 };

// Forzar un tipo más específico que la inferencia
const elemento = document.getElementById("miBoton"); // Infiere HTMLElement | null
const miBoton: HTMLButtonElement | null = document.getElementById("miBoton") as HTMLButtonElement | null; // Más específico
// O usando type assertion (con cuidado):
const miBotonAsegurado = document.getElementById("miBoton") as HTMLButtonElement;
```

[↑ Volver al Índice](#toc-container)


### Cuándo usar Anotaciones Explícitas

1.  **Declaración sin Inicialización:** Si una variable no se inicializa en su declaración, TypeScript no puede inferir su tipo (a menudo resulta en `any` implícito). La anotación es obligatoria para la seguridad de tipos.
2.  **Funciones (Parámetros y Retorno):** **Siempre** se deben anotar los tipos de los parámetros de las funciones. Anotar el tipo de retorno es una muy buena práctica para claridad y para asegurar que la función cumple su contrato.
3.  **Tipos Complejos (Unión, Intersección, Genéricos):** Cuando una variable puede tener múltiples tipos, o se usan tipos más avanzados, la anotación explícita es crucial para la comprensión.
4.  **Cuando la Inferencia no es Suficiente o Correcta:** A veces, TypeScript infiere un tipo demasiado amplio (ej. `string | number` cuando solo queremos `number` inicialmente pero permitimos `string` después) o simplemente incorrecto en contextos complejos. La anotación clarifica la intención.
5.  **Límites de Objetos y APIs:** Al definir la forma de objetos que vienen de una API externa o que forman parte de la API pública de un módulo, las anotaciones explícitas (a menudo mediante `interface` o `type`) son esenciales.
6.  **Claridad y Documentación:** En algoritmos complejos o lógica de negocio crítica, las anotaciones sirven como documentación auto-verificable.

#### Ejemplo: Aclarando la intención con anotaciones

```typescript
// Escenario: Recibimos datos de una API que pueden ser un objeto o null si no se encuentra
type DatosUsuario = { id: number; nombre: string };
let usuarioActual: DatosUsuario | null = null; // Inferencia sola podría ser solo 'null'

async function cargarUsuario(id: number) {
  const datos = await fetch(`/api/usuarios/${id}`);
  if (datos.ok) {
    usuarioActual = await datos.json(); // Asignamos el objeto DatosUsuario
  } else {
    usuarioActual = null; // Asignamos null explícitamente
  }
}
```

[↑ Volver al Índice](#toc-container)


### Casos de Uso Reales

- **Inferencia:**
    - Variables locales simples con inicialización directa: `let i = 0;`, `const msg = "Hecho";`.
    - Constantes obvias: `const IVA = 0.21;`.
    - Tipos de retorno simples inferidos: `function esMayor(a: number, b: number) { return a > b; } // Infiere boolean`.
- **Anotaciones Explícitas:**
    - Definición de la forma de los datos de una API: `interface Producto { id: string; nombre: string; precio: number; } let productoCargado: Producto | null;`
    - Parámetros y retorno de funciones reutilizables o complejas.
    - Variables que comienzan como `null` o `undefined` pero luego tendrán un tipo específico.
    - Uso de tipos unión para representar diferentes estados o valores posibles: `type EstadoCarga = "idle" | "loading" | "success" | "error"; let estado: EstadoCarga = "idle";`

[↑ Volver al Índice](#toc-container)


### Buenas Prácticas

- **Confía en la inferencia para lo simple:** No anotes explícitamente tipos que TypeScript puede inferir fácil y correctamente (ej. `let edad = 30;`). Mantiene el código limpio.
- **Sé explícito donde importa:** Usa anotaciones para firmas de funciones, tipos complejos, variables no inicializadas y para clarificar la intención en lógica compleja.
- **Activa `noImplicitAny`:** Esta opción del compilador (`tsconfig.json`) es **fundamental**. Obliga a anotar explícitamente cualquier cosa que TypeScript no pueda inferir, previniendo el peligroso tipo `any` implícito.

{% hint style="success" %}
Configurar `"noImplicitAny": true` en tu `tsconfig.json` es una de las mejores prácticas para asegurar un uso robusto de TypeScript.
{% endhint %}

- **Evita `any` explícito:** El tipo `any` desactiva la comprobación de tipos. Úsalo como último recurso (ej. integración con JS antiguo, tipos complejos temporalmente desconocidos). Prefiere `unknown` si necesitas un tipo seguro para valores de tipo desconocido, ya que `unknown` te obliga a verificar el tipo antes de usarlo.

[↑ Volver al Índice](#toc-container)


### Malas Prácticas

- **Anotaciones redundantes:** `let nombre: string = "Ana";`. TypeScript infiere `string` perfectamente. Esto es solo ruido.
- **Abuso de `any`:** Usar `any` para "solucionar" rápidamente errores de tipo. Esto invalida los beneficios de TypeScript y esconde posibles bugs.

{% hint style="danger" %}
Cada uso de `any` debe ser considerado cuidadosamente. Representa una "puerta trasera" en el sistema de tipos y debe evitarse siempre que sea posible. Si necesitas flexibilidad, considera tipos unión, genéricos o `unknown`.
{% endhint %}

- **No anotar parámetros de función:** Dejar que los parámetros sean `any` implícitos es una receta para errores en tiempo de ejecución que TypeScript podría haber detectado en tiempo de compilación.

[↑ Volver al Índice](#toc-container)


### Errores Comunes

- **Confundir `const` con inmutabilidad profunda:** Ya mencionado, pero es un error muy frecuente. Recordar que `const` solo protege la asignación de la variable.
- **Errores con `this` en funciones y `let`/`const`:** Aunque no directamente de tipado, la forma de declarar funciones (especialmente métodos en clases) puede interactuar con `let`/`const` y el contexto de `this`. Usar funciones flecha (`=>`) a menudo ayuda a preservar el contexto esperado.
- **Uso incorrecto de Type Assertions (`as`):** Usar `as Tipo` para forzar un tipo cuando no estás seguro de que el valor realmente sea de ese tipo. Las aserciones le dicen al compilador "confía en mí, sé lo que hago", pero si te equivocas, tendrás errores en tiempo de ejecución. Es más seguro usar validaciones de tipo o type guards.
  ```typescript
  // Peligroso si inputElement no es realmente un HTMLInputElement
  const valorInput = (document.getElementById('miInput') as HTMLInputElement).value;

  // Más seguro
  const inputElement = document.getElementById('miInput');
  if (inputElement instanceof HTMLInputElement) {
    const valorInputSeguro = inputElement.value; // Ahora es seguro acceder a .value
  }
  ```
- **Manejo inadecuado de `null`/`undefined` sin `strictNullChecks`:** Si `strictNullChecks` está desactivado, `null` y `undefined` pueden asignarse a cualquier tipo, ocultando errores potenciales. Activar esta opción y manejar explícitamente los casos `null`/`undefined` (con tipos unión `T | null` y comprobaciones) es mucho más seguro.

[↑ Volver al Índice](#toc-container)