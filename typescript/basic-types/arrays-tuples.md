<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/arrays-tuples)
<!-- MULTILANGUAJE MENU END -->

<!--
# Arrays y Tuplas en TypeScript
- Declaración de arrays (`number[]`, `Array<string>`)
- Uso de tuplas (`[string, number]`)
- Tuplas con etiquetas (`[id: number, nombre: string]`)
-->

<details>
<summary>Índice de contenidos</summary>

<!-- no toc -->
- [Declaración de Arrays (`number[]`, `Array<string>`)](#declaracion-de-arrays-number-arraystring)
  - [Definición y Sintaxis](#definicion-y-sintaxis)
  - [Casos de Uso Reales y Recomendables](#casos-de-uso-reales-y-recomendables)
  - [Consideraciones Importantes](#consideraciones-importantes)
  - [Buenas Prácticas](#buenas-practicas)
  - [Malas Prácticas](#malas-practicas)
  - [Errores Comunes y Trampas](#errores-comunes-y-trampas)
- [Uso de Tuplas (`[string, number]`)](#uso-de-tuplas-string-number)
  - [Definición y Sintaxis](#definicion-y-sintaxis-1)
  - [Casos de Uso Reales y Recomendables](#casos-de-uso-reales-y-recomendables-1)
  - [Consideraciones Importantes](#consideraciones-importantes-1)
  - [Buenas Prácticas](#buenas-practicas-1)
  - [Malas Prácticas](#malas-practicas-1)
  - [Errores Comunes y Trampas](#errores-comunes-y-trampas-1)
- [Tuplas con Etiquetas (`[id: number, nombre: string]`)](#tuplas-con-etiquetas-id-number-nombre-string)
  - [Definición y Sintaxis](#definicion-y-sintaxis-2)
  - [Ventajas y Casos de Uso](#ventajas-y-casos-de-uso)
</details>

# Arrays y Tuplas en TypeScript

TypeScript, al ser un superconjunto de JavaScript, hereda sus estructuras de datos fundamentales pero las potencia con un sistema de tipos estático robusto. Entre las colecciones de datos más utilizadas se encuentran los **arrays** y las **tuplas**. Aunque ambos permiten almacenar múltiples valores, su propósito, estructura y comportamiento difieren significativamente. Comprender estas diferencias es crucial para modelar datos de forma precisa, aprovechar la seguridad de tipos que ofrece TypeScript y escribir código más limpio, predecible y fácil de mantener, especialmente en aplicaciones complejas donde la integridad de los datos es primordial.

---

## Declaración de Arrays (`number[]`, `Array<string>`)

Los arrays en TypeScript, al igual que en JavaScript, representan colecciones ordenadas de elementos a los que se puede acceder mediante un índice numérico (base 0). La característica distintiva que aporta TypeScript es la capacidad de **especificar el tipo de datos que el array puede contener**. Esta restricción de tipo se verifica en tiempo de compilación, lo que ayuda a prevenir una clase entera de errores relacionados con la manipulación de datos de tipos inesperados. Los arrays son ideales para listas donde todos los elementos comparten el mismo tipo y cuyo tamaño puede variar dinámicamente durante la ejecución del programa.

### Definición y Sintaxis

TypeScript ofrece dos sintaxis principales y equivalentes para declarar arrays:

1.  **Sintaxis de Corchetes (`T[]`)**: Esta es la forma más común y concisa. Se especifica el tipo de los elementos (`T`) seguido de corchetes vacíos (`[]`).
    *   `let numeros: number[];` // Declara un array que solo contendrá números.
    *   `let nombres: string[];` // Declara un array que solo contendrá cadenas de texto.
2.  **Sintaxis Genérica (`Array<T>`)**: Utiliza el tipo genérico `Array` proporcionado por TypeScript, especificando el tipo de los elementos (`T`) entre ángulos (`<>`).
    *   `let lecturas: Array<number>;` // Equivalente a `number[]`.
    *   `let identificadores: Array<string>;` // Equivalente a `string[]`.

La elección entre `T[]` y `Array<T>` es principalmente una cuestión de preferencia estilística, aunque la sintaxis `T[]` es más prevalente en la comunidad TypeScript.

#### Ejemplos Detallados

```typescript
// ===== Declaración e Inicialización Básica =====

// Usando T[] (preferido por concisión)
let puntuacionesExamen: number[] = [85, 92, 78, 99]; // Array de números inicializado
let nombresParticipantes: string[] = ["Ana", "Luis", "Elena"]; // Array de strings
let tareasCompletadas: boolean[] = [true, false, true, true]; // Array de booleanos

// Usando Array<T>
let medicionesSensor: Array<number> = [12.5, 13.1, 12.8]; // Array de números
let categoriasProducto: Array<string> = ["Electrónica", "Ropa", "Hogar"]; // Array de strings

// ===== Arrays con Tipos Unión =====
// Permite elementos de tipos específicos mezclados
let identificadoresMixtos: (string | number)[] = ["ID-123", 456, "ID-789", 101];
// let errorMixto: (string | number)[] = [true]; // Error: Type 'boolean' is not assignable to type 'string | number'.

// ===== Arrays de Objetos =====
// Muy común para listas de datos estructurados
interface Usuario {
  id: number;
  nombre: string;
  activo: boolean;
}

let listaUsuarios: Usuario[] = [
  { id: 1, nombre: "Carlos", activo: true },
  { id: 2, nombre: "Marta", activo: false },
  { id: 3, nombre: "Javier", activo: true }
];

// Acceso a propiedades
console.log(listaUsuarios[0].nombre); // Salida: Carlos

// ===== Arrays Multidimensionales =====
// Arrays que contienen otros arrays
let matrizNumeros: number[][] = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

console.log(matrizNumeros[1][1]); // Salida: 5 (accede al segundo array, luego al segundo elemento)

// ===== El tipo 'any' (A EVITAR) =====
// Permite cualquier tipo, perdiendo la seguridad de tipos
let datosSinTipo: any[] = [1, "hola", true, { clave: "valor" }, null];
// No hay errores en tiempo de compilación, pero sí potenciales errores en ejecución
// datosSinTipo[0].toUpperCase(); // Error en tiempo de ejecución: toUpperCase is not a function
```

### Casos de Uso Reales y Recomendables

Los arrays son una herramienta versátil con aplicaciones en numerosos escenarios de desarrollo:

*   **Gestión de Listas de Entidades:** Almacenar y manipular colecciones de objetos similares, como una lista de usuarios registrados, productos en un carrito de compras, comentarios en una publicación, etc. La restricción de tipo asegura que todos los objetos de la lista comparten una estructura común (definida por una interfaz o clase).
*   **Resultados de Consultas a Bases de Datos o APIs:** Las respuestas de APIs REST o consultas a bases de datos frecuentemente devuelven listas de registros. Tipar estos arrays (`Producto[]`, `Pedido[]`) permite trabajar con los datos de forma segura y predecible.
*   **Almacenamiento de Datos de Formularios Dinámicos:** En formularios donde el usuario puede añadir múltiples entradas del mismo tipo (ej. añadir varios números de teléfono, direcciones de correo), un array es ideal para almacenar estos valores.
*   **Procesamiento de Datos en Lote:** Realizar operaciones sobre conjuntos de datos, como calcular el promedio de una lista de puntuaciones (`number[]`), filtrar una lista de correos electrónicos (`string[]`) o transformar una lista de objetos.
*   **Implementación de Estructuras de Datos:** Los arrays son la base para implementar otras estructuras de datos como pilas (stacks), colas (queues) o listas enlazadas (aunque estas últimas a menudo se implementan de forma diferente para optimizar inserciones/eliminaciones).

### Consideraciones Importantes

*   **Seguridad de Tipos vs. Flexibilidad:** La principal ventaja es la seguridad de tipos. TypeScript verifica que solo se añadan elementos del tipo especificado. Si necesitas flexibilidad para almacenar diferentes tipos, considera los **tipos unión** (`(string | number)[]`) que ofrecen flexibilidad controlada, en lugar de recurrir a `any[]` que deshabilita la verificación de tipos.
*   **Rendimiento de Operaciones:** Los arrays en JavaScript/TypeScript son dinámicos. Operaciones como `push` (añadir al final) y `pop` (quitar del final) son generalmente rápidas (O(1) amortizado). Sin embargo, insertar o eliminar elementos al principio o en medio (`unshift`, `splice`) puede ser costoso (O(n)) en arrays grandes, ya que requiere reorganizar los índices de los elementos posteriores. Para escenarios con muchas inserciones/eliminaciones en medio, otras estructuras como listas enlazadas podrían ser más eficientes, aunque menos comunes en el día a día de JavaScript/TypeScript.
*   **Inmutabilidad y `readonly`:** Por defecto, los arrays son mutables (pueden modificarse después de su creación). Si necesitas garantizar que un array no sea modificado, TypeScript ofrece el modificador `readonly`:
    ```typescript
    let numerosFijos: readonly number[] = [10, 20, 30];
    // numerosFijos.push(40); // Error: Property 'push' does not exist on type 'readonly number[]'.
    // numerosFijos[0] = 5;   // Error: Index signature in type 'readonly number[]' only permits reading.

    // También funciona con la sintaxis genérica
    let cadenasFijas: ReadonlyArray<string> = ["a", "b", "c"];
    ```
    Usar `readonly` es una excelente práctica en programación funcional o cuando se pasan arrays a funciones que no deben alterarlos, previniendo efectos secundarios inesperados. Ten en cuenta que `readonly` solo previene la reasignación o modificación directa del array; si el array contiene objetos, las propiedades de esos objetos *sí* pueden ser modificadas (es una inmutabilidad superficial).
*   **Comparación de Arrays:** Comparar arrays con `==` o `===` verifica la igualdad de referencia (si apuntan al mismo objeto en memoria), no la igualdad de contenido. Para comparar el contenido, necesitas iterar y comparar cada elemento o usar librerías externas.
    ```typescript
    let arr1 = [1, 2];
    let arr2 = [1, 2];
    let arr3 = arr1;
    console.log(arr1 === arr2); // false (diferentes objetos en memoria)
    console.log(arr1 === arr3); // true (misma referencia)
    ```

### Buenas Prácticas

*   **Especificar el tipo explícitamente:** Siempre define el tipo de los elementos (`number[]`, `Usuario[]`) para maximizar la seguridad y claridad. Evita que TypeScript infiera `any[]`.
*   **Preferir `T[]`:** Usa la sintaxis `T[]` por ser más concisa y común, a menos que las convenciones del equipo dicten lo contrario.
*   **Utilizar `readonly` cuando la inmutabilidad sea deseada:** Aplica `readonly T[]` o `ReadonlyArray<T>` para arrays que no deben cambiar después de su creación, mejorando la predictibilidad del código.
*   **Inicializar arrays al declarar:** Evita declarar arrays sin inicializar (`let miArray: number[];`) si es posible, ya que tendrán el valor `undefined` hasta que se les asigne un array, lo cual puede llevar a errores si se intentan usar antes de tiempo. Prefiere `let miArray: number[] = [];`.
*   **Usar métodos funcionales para transformaciones:** Prefiere métodos como `map`, `filter`, `reduce` para crear nuevos arrays basados en el original en lugar de modificarlo directamente, especialmente si buscas un estilo de programación más funcional o necesitas inmutabilidad.

### Malas Prácticas

*   **Abuso de `any[]`:** Usar `any[]` indiscriminadamente anula la principal ventaja de TypeScript. Si necesitas tipos mixtos, usa tipos unión (`(string | number | boolean)[]`) o define interfaces/tipos adecuados si la estructura es más compleja.
*   **Mutación inesperada de arrays pasados como argumentos:** Si una función recibe un array y lo modifica sin que el llamador lo espere, puede causar efectos secundarios difíciles de depurar. Considera usar `readonly` en los parámetros de la función o clonar el array (`[...miArray]`, `miArray.slice()`) antes de modificarlo dentro de la función.
*   **Ordenar números sin función de comparación:** Depender del `sort()` por defecto para números (`[1, 10, 2].sort()`) produce un orden incorrecto (`[1, 10, 2]`) porque ordena lexicográficamente (como strings). Proporciona siempre una función de comparación: `numeros.sort((a, b) => a - b);`.
*   **Ignorar que métodos como `map`, `filter`, `slice` devuelven *nuevos* arrays:** Esperar que `miArray.filter(...)` modifique `miArray` es un error común. Estos métodos crean y devuelven un nuevo array con los resultados. Asigna el resultado a una nueva variable o a la misma si deseas sobrescribirla: `miArrayFiltrado = miArray.filter(...)`.
*   **Crear arrays dispersos (sparse arrays) innecesariamente:** Aunque JavaScript permite arrays con "huecos" (`let a = []; a[0] = 1; a[100] = 100;`), suelen ser menos eficientes y más propensos a errores que los arrays densos. En TypeScript, es mejor evitarlos si no hay una razón muy específica.

### Errores Comunes y Trampas

*   **Acceso a Índices Fuera de Límites (`undefined`):** TypeScript **no** previene en tiempo de compilación el acceso a un índice que no existe en el array. `miArray[indiceInvalido]` devolverá `undefined` en tiempo de ejecución. Esto puede causar errores si el código posterior espera un valor válido. Siempre verifica los límites o maneja la posibilidad de `undefined`, especialmente con índices calculados dinámicamente.
    ```typescript
    let nombres = ["Ana", "Luis"];
    console.log(nombres[2]); // undefined (sin error de compilación)
    // console.log(nombres[2].toUpperCase()); // TypeError en ejecución!
    ```
	 
{% hint style="warning" %}
Activar la opción `noUncheckedIndexedAccess` en `tsconfig.json` puede ayudar. Con esta opción, el acceso a un índice de array o tipo de objeto se considera potencialmente `undefined` (`string | undefined`), forzando su manejo explícito.
{% endhint %}

*   **Mutación por Referencia:** Cuando asignas un array a otra variable (`let copia = original;`), ambas variables apuntan al *mismo* array en memoria. Modificar `copia` también afectará a `original` y viceversa. Para crear una copia independiente (superficial), usa el operador spread (`[...original]`) o `slice()` (`original.slice()`).
    ```typescript
    let original = [1, 2];
    let referencia = original;
    let copiaSuperficial = [...original];

    referencia.push(3);
    console.log(original); // [1, 2, 3] (modificado!)
    console.log(referencia); // [1, 2, 3]

    copiaSuperficial.push(4);
    console.log(original); // [1, 2, 3] (no afectado por la copia)
    console.log(copiaSuperficial); // [1, 2, 3, 4]
    ```
*   **Confusión con métodos mutadores vs. no mutadores:** No recordar qué métodos modifican el array original (`push`, `pop`, `splice`, `sort`, `reverse`, `fill`, `copyWithin`) y cuáles devuelven uno nuevo (`map`, `filter`, `slice`, `concat`, `reduce`) es una fuente común de bugs. Consulta la documentación si tienes dudas.

---

## Uso de Tuplas (`[string, number]`)

Las tuplas son una adición de TypeScript que no existe directamente en JavaScript (aunque se compilan a arrays de JavaScript). Representan un **array con un número fijo de elementos, donde el tipo de cada elemento está estrictamente definido según su posición (índice)**. Son útiles para representar estructuras de datos donde el orden y el tipo de cada elemento son significativos y conocidos de antemano, como un par clave-valor, coordenadas o los valores devueltos por una función.

### Definición y Sintaxis

Las tuplas se definen usando corchetes `[]` y listando los tipos de cada elemento en el orden exacto que deben tener.

```typescript
// Una tupla para representar un punto 2D (coordenadas x, y)
let punto2D: [number, number];
punto2D = [10, 20]; // Correcto
// punto2D = [10]; // Error: Type '[number]' is not assignable to type '[number, number]'. Source has 1 element(s) but target requires 2.
// punto2D = [10, 20, 30]; // Error: Type '[number, number, number]' is not assignable to type '[number, number]'. Source has 3 element(s) but target allows only 2.
// punto2D = ["10", 20]; // Error: Type 'string' is not assignable to type 'number'.

// Una tupla para representar información básica de un usuario: ID (number) y Nombre (string)
let infoUsuario: [number, string];
infoUsuario = [123, "Alice"];

// Acceso a elementos por índice (con seguridad de tipos específica para esa posición)
let coordX: number = punto2D[0]; // TypeScript sabe que el elemento en el índice 0 es 'number'
let nombreUsuario: string = infoUsuario[1]; // TypeScript sabe que el elemento en el índice 1 es 'string'

// console.log(punto2D[2]); // Error: Tuple type '[number, number]' of length '2' has no element at index '2'.
```

#### Características Avanzadas de Sintaxis

*   **Elementos Opcionales (`?`):** Puedes marcar elementos como opcionales añadiendo `?` después de su tipo. Todos los elementos opcionales deben ir después de los elementos requeridos.
    ```typescript
    let configuracion: [string, number, boolean?]; // El tercer elemento es opcional
    configuracion = ["host", 8080];       // Correcto
    configuracion = ["host", 8080, true]; // Correcto
    ```
*   **Elementos Rest (`...T[]`):** Puedes definir una tupla que tenga un número mínimo de elementos iniciales con tipos específicos, seguido de un número variable de elementos restantes de un tipo concreto. El elemento rest debe ser de tipo array (`T[]`) y debe ser el último elemento de la tupla.
    ```typescript
    // Una función que devuelve el nombre del evento y una lista de participantes
    type EventoResultado = [string, ...string[]];
    let concierto: EventoResultado = ["Concierto Rock", "Ana", "Luis", "Eva"];
    let charla: EventoResultado = ["Charla Técnica"]; // Correcto, cero participantes

    // console.log(concierto[0]); // "Concierto Rock" (string)
    // console.log(concierto[1]); // "Ana" (string)
    // console.log(concierto.slice(1)); // ["Ana", "Luis", "Eva"] (string[])
    ```

### Casos de Uso Reales y Recomendables

*   **Pares Clave-Valor Definidos:** Cuando necesitas representar un par donde el tipo de la clave y el tipo del valor son fijos y distintos (ej., `[string, number]` para una propiedad y su valor numérico). Útil en estructuras de datos internas o configuraciones simples.
*   **Coordenadas y Puntos Geométricos:** Representar puntos en 2D (`[number, number]`), 3D (`[number, number, number]`), o incluso coordenadas geográficas (`[number, number]` para latitud y longitud). El orden es significativo.
*   **Devolución Múltiple de Funciones:** Aunque a menudo es preferible devolver un objeto con propiedades nombradas para mayor claridad, las tuplas pueden ser una forma concisa para funciones que devuelven un conjunto pequeño y fijo de valores con tipos distintos.
    ```typescript
    function dividirEntero(dividendo: number, divisor: number): [cociente: number, resto: number] | undefined {
      if (divisor === 0) {
        return undefined; // Manejo de error
      }
      const cociente = Math.floor(dividendo / divisor);
      const resto = dividendo % divisor;
      return [cociente, resto]; // Devuelve una tupla
    }

    const resultado = dividirEntero(10, 3);
    if (resultado) {
      const [coc, res] = resultado; // Desestructuración para claridad
      console.log(`Cociente: ${coc}, Resto: ${res}`); // Cociente: 3, Resto: 1
    }
    ```
*   **Estado en Hooks de React:** El ejemplo canónico es el hook `useState` de React, que devuelve una tupla: `[valorEstado, funcionActualizadora]`. La estructura fija y los tipos distintos para cada posición hacen de la tupla una elección ideal aquí.
    ```typescript
    // Ejemplo conceptual similar a React useState
    function miUseState<S>(initialState: S): [S, (newState: S) => void] {
      let state = initialState;
      const setState = (newState: S) => {
        state = newState;
        // Lógica de re-renderizado...
      };
      return [state, setState];
    }

    const [contador, setContador] = miUseState(0); // contador es number, setContador es (newState: number) => void
    ```
*   **Representación de Registros Fijos:** Para datos con una estructura muy estable y un número pequeño de campos donde el orden es intrínseco (ej., representar una fila de un CSV simple con columnas `[string, number, boolean]`).

### Consideraciones Importantes

*   **Longitud Fija (Intención vs. Realidad Histórica):** El propósito principal de las tuplas es tener una longitud fija definida por los tipos especificados. Sin embargo, debido a que se compilan a arrays de JavaScript, métodos como `push`, `pop`, `splice` *podrían* (especialmente en versiones antiguas de TS o configuraciones laxas) modificar la longitud en tiempo de ejecución. **Esto es una mala práctica y viola el contrato de la tupla**. Las versiones modernas de TypeScript y configuraciones estrictas tienden a prevenir o advertir sobre esto, especialmente si se usan con `readonly`.
  
{% hint style="danger" %}
Evita usar métodos mutadores de array (`push`, `pop`, `splice`, etc.) en tuplas. Trátalas como estructuras de tamaño fijo. Si necesitas una colección de tamaño variable, usa un array (`T[]`).
{% endhint %}

*   **Acceso por Índice vs. Legibilidad:** El acceso siempre se realiza mediante índices numéricos (`miTupla[0]`, `miTupla[1]`). Esto puede reducir la legibilidad si no está inmediatamente claro qué representa cada índice.
*   **Desestructuración para Claridad:** La **desestructuración** es la forma preferida y más legible de extraer los valores de una tupla, asignando nombres significativos a cada elemento.
    ```typescript
    let config: [string, number] = ["localhost", 3000];
    // Menos legible:
    // const host = config[0];
    // const port = config[1];

    // Más legible con desestructuración:
    const [host, port] = config;
    console.log(`Servidor en ${host}:${port}`);
    ```
*   **Tuplas vs. Objetos:** Para estructuras con más de unos pocos elementos, o donde el orden no es tan intrínsecamente significativo como el nombre de la propiedad, un **objeto con propiedades nombradas** (`{ id: number; nombre: string; activo: boolean; }`) suele ser mucho más claro y mantenible que una tupla larga (`[number, string, boolean]`). Los objetos permiten el acceso por nombre (`objeto.propiedad`), lo cual es auto-documentado.

### Buenas Prácticas

*   **Usar tuplas para colecciones heterogéneas de longitud fija y orden significativo:** Son perfectas cuando tienes un número conocido de elementos (generalmente pocos) con tipos específicos en posiciones concretas.
*   **Preferir la desestructuración para acceder a los elementos:** Mejora drásticamente la legibilidad al dar nombres a los valores extraídos.
*   **Utilizar `readonly` para tuplas inmutables:** Si la tupla representa un valor que no debe cambiar (como un punto o una configuración fija), declárala como `readonly [Tipo1, Tipo2]`.
    ```typescript
    let puntoFijo: readonly [number, number] = [10, 5];
    // puntoFijo[0] = 15; // Error
    // puntoFijo.push(20); // Error
    ```
*   **Considerar objetos para claridad en estructuras complejas:** No fuerces el uso de tuplas si un objeto con propiedades nombradas haría el código más comprensible y mantenible, especialmente con 3 o más elementos.

### Malas Prácticas

*   **Usar tuplas para listas homogéneas o de tamaño variable:** Si todos los elementos son del mismo tipo y/o la longitud puede cambiar, un array (`T[]`) es la herramienta adecuada.
*   **Crear tuplas excesivamente largas:** Tuplas con muchos elementos (`[string, number, boolean, string, Date, ...]`) se vuelven muy difíciles de manejar y propensas a errores por confusión de índices. Usa interfaces u objetos.
*   **Modificar tuplas con `push`, `pop`, `splice`:** Viola el propósito fundamental de la tupla (longitud y tipos fijos por posición). Evítalo a toda costa.
*   **Acceder a elementos solo por índice numérico sin contexto claro:** Hace el código difícil de entender. Si no usas desestructuración, asegúrate de que el contexto (nombre de variable, comentarios) aclare qué representa cada índice.

### Errores Comunes y Trampas

*   **Confusión entre Tuplas y Arrays:** Recordar la diferencia clave: los arrays son listas homogéneas (mismo tipo) de tamaño variable, mientras que las tuplas son estructuras heterogéneas (diferentes tipos por posición) de tamaño fijo.
*   **Errores Lógicos de Índice:** Aunque TypeScript detecta el acceso a índices fuera de los límites *definidos* de la tupla, no puede prevenir errores lógicos donde usas el índice incorrecto (ej., `miTupla[0]` esperando el valor que está en `miTupla[1]`). La desestructuración ayuda a mitigar esto.
*   **Comportamiento de `push` en Versiones Anteriores:** Históricamente, `push` en una tupla podía permitir añadir elementos de *cualquier* tipo presente en la definición de la tupla, no necesariamente respetando el tipo de una posición específica o la longitud fija. Las versiones modernas de TypeScript son más estrictas, pero es bueno conocer este comportamiento si se trabaja con código antiguo.
*   **Tipado Estructural:** TypeScript usa tipado estructural. Un array que *casualmente* tiene los mismos tipos en las posiciones correctas y la longitud adecuada puede ser asignable a un tipo tupla, y viceversa en algunos casos, lo que a veces puede sorprender si no se entiende este concepto.
    ```typescript
    let miArray: (string | number)[] = ["hola", 123];
    let miTupla: [string, number] = ["hola", 123];

    miTupla = miArray; // Error: Type '(string | number)[]' is not assignable to type '[string, number]'. Target requires 2 element(s) but source may have more.

    // Pero ¡cuidado!:
    let otroArray = ["mundo", 456]; // TypeScript infiere (string | number)[]
    miTupla = otroArray as [string, number]; // Posible con type assertion (¡peligroso si la estructura no coincide!)

    let tuplaMasLarga: [string, number, boolean] = ["a", 1, true];
    let arrayDesdeTupla: (string | number | boolean)[] = tuplaMasLarga; // Asignación válida de tupla a array compatible
    ```

---

## Tuplas con Etiquetas (`[id: number, nombre: string]`)

Para mejorar la legibilidad del código al trabajar con tuplas, TypeScript permite añadir **etiquetas** a los elementos de la tupla. Estas etiquetas actúan puramente como **documentación en tiempo de desarrollo**; no tienen ningún impacto en el código JavaScript compilado ni cambian la forma en que se accede a los elementos (que sigue siendo exclusivamente por índice numérico).

### Definición y Sintaxis

Las etiquetas se añaden anteponiendo un nombre descriptivo seguido de dos puntos (`:`) antes del tipo de cada elemento en la definición de la tupla.

```typescript
// Tupla sin etiquetas (menos descriptiva)
let productoInfoSimple: [string, number, number];
productoInfoSimple = ["SKU-123", 29.99, 150]; // ¿Qué es cada número?

// Tupla con etiquetas (más descriptiva)
let productoInfoEtiquetada: [codigo: string, precio: number, stock: number];
productoInfoEtiquetada = ["SKU-456", 45.50, 75];

// El acceso sigue siendo numérico
let elCodigo: string = productoInfoEtiquetada[0];    // "SKU-456"
let elPrecio: number = productoInfoEtiquetada[1];    // 45.50
let elStock: number = productoInfoEtiquetada[2];     // 75

// ¡Intentar acceder por etiqueta NO funciona!
// let precioIncorrecto = productoInfoEtiquetada.precio; // Error: Property 'precio' does not exist on type '[codigo: string, precio: number, stock: number]'.
```

### Ventajas y Casos de Uso

*   **Mejora Exponencial de la Legibilidad:** El principal beneficio es la claridad. Al ver la definición del tipo `[codigo: string, precio: number, stock: number]`, es inmediatamente obvio qué representa cada posición, eliminando la ambigüedad de `[string, number, number]`.
*   **Autodocumentación:** Las etiquetas sirven como documentación integrada directamente en el tipo. Los editores de código modernos (como VS Code) utilizan estas etiquetas para mostrar información útil en tooltips e IntelliSense al trabajar con variables de ese tipo tupla.
    ```typescript
    function procesarProducto(producto: [codigo: string, precio: number, stock: number]) {
        // Al pasar el cursor sobre producto[1] en un editor compatible,
        // mostrará algo como "(parameter) producto: [..., precio: number, ...]"
        const costeTotal = producto[1] * producto[2];
        console.log(`Coste total inventario para ${producto[0]}: ${costeTotal}`);
    }
    procesarProducto(productoInfoEtiquetada);
    ```
*   **Claridad en Tipos Complejos:** En definiciones de tipos que involucran tuplas anidadas o combinadas con otros tipos, las etiquetas son cruciales para mantener la comprensión de la estructura de datos.
*   **Mejor Comunicación en Equipos:** Facilitan que otros desarrolladores (o tu yo futuro) entiendan rápidamente la estructura y el propósito de los datos representados por la tupla.

### Consideraciones Importantes

*   **Naturaleza Puramente Documental:** Es fundamental recordar que las etiquetas **no existen en tiempo de ejecución**. Son una ayuda exclusiva para el desarrollador durante la fase de codificación y análisis. No modifican el comportamiento ni permiten nuevas formas de acceso.
*   **Acceso Siempre por Índice:** El acceso a los elementos de una tupla etiquetada sigue siendo, y siempre será, mediante índices numéricos (`tupla[0]`, `tupla[1]`, etc.).
*   **Mantenimiento de Etiquetas:** Si la estructura de la tupla cambia (por ejemplo, se reordenan elementos o cambia el significado de una posición), es vital actualizar las etiquetas correspondientes para evitar documentación engañosa.

### Buenas Prácticas

*   **Usar etiquetas siempre que aporten claridad:** Especialmente para tuplas con más de dos elementos o donde el significado de cada posición no sea trivialmente obvio por el contexto (como en `[number, number]` para coordenadas, que puede ser suficientemente claro).
*   **Elegir nombres de etiqueta descriptivos y concisos:** Sigue las mismas convenciones que para nombres de variables (claridad, camelCase si aplica).
*   **Combinar con desestructuración:** Aunque las etiquetas no se usan *directamente* en la sintaxis de desestructuración (`let [c, p, s] = miTuplaEtiquetada;`), definir la tupla con etiquetas mejora enormemente la comprensión de qué son `c`, `p` y `s`.

### Malas Prácticas

*   **Usar etiquetas genéricas o poco claras:** Nombres como `val1`, `item2`, `dato` (`[val1: string, item2: number, dato: number]`) aportan poco valor sobre `[string, number, number]`.
*   **Asumir que las etiquetas permiten acceso por nombre:** Confiar en que `tupla.etiqueta` funcionará es un error conceptual. Si necesitas acceso por nombre, la estructura de datos correcta es un objeto (`{ etiqueta: tipo; }`).
*   **Dejar etiquetas desactualizadas:** Si la estructura de la tupla evoluciona, no actualizar las etiquetas crea confusión y reduce la confianza en la documentación del tipo.

### Errores Comunes y Trampas

*   **Intento de Acceso por Etiqueta:** El error más frecuente es intentar usar la etiqueta como si fuera una propiedad de objeto (`miTupla.miEtiqueta`). Recordar: siempre `miTupla[indiceNumerico]`.
*   **Olvido de su Naturaleza en Tiempo de Desarrollo:** No tener presente que las etiquetas desaparecen en el JavaScript compilado puede llevar a confusiones si se inspecciona el código transpilado o se depura en el navegador sin sourcemaps.

{% hint style="info" %}
En resumen, los **arrays (`T[]`)** son para listas homogéneas de tamaño variable, mientras que las **tuplas (`[T1, T2, ...]`)** son para estructuras heterogéneas de tamaño fijo y orden significativo. Las **etiquetas** en las tuplas son una excelente herramienta para mejorar la legibilidad y documentación en tiempo de desarrollo, sin afectar el comportamiento en tiempo de ejecución. Elegir la estructura correcta y usarla adecuadamente es clave para escribir código TypeScript robusto y mantenible.
{% endhint %}
