<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/primitive-types)
<!-- MULTILANGUAJE MENU END -->

<!--
# Tipos primitivos en TypeScript

- `string`, `number`, `boolean`, `null`, `undefined`
- Diferencias entre `null` y `undefined`
- Uso de `bigint` para operaciones con grandes números
-->

# Tipos Primitivos en TypeScript

TypeScript, al ser un superconjunto de JavaScript, hereda sus tipos de datos primitivos. Estos tipos son la base sobre la cual se construyen estructuras de datos más complejas. Entenderlos a fondo es crucial para escribir código robusto y eficiente.

## Tipos Primitivos Básicos (`string`, `number`, `boolean`)

Estos son los tipos más fundamentales y comunes que encontrarás en el desarrollo diario.

### `string`

#### Definición y Utilidad

El tipo `string` se utiliza para representar datos textuales. Cualquier secuencia de caracteres encerrada entre comillas simples (`'`), dobles (`"`) o backticks (`` ` ``) se considera una cadena de texto.

```typescript
let nombre: string = "Juan Pérez";
let mensaje: string = 'Bienvenido al sistema';
let plantilla: string = `El usuario ${nombre} ha iniciado sesión.`; // Template literal
```

Los *template literals* (usando backticks) son especialmente útiles para crear cadenas multilínea o para interpolar variables y expresiones directamente dentro de la cadena, mejorando la legibilidad.

#### Casos de Uso Reales y Recomendables

- **Manipulación de Texto:** Concatenar nombres, formatear direcciones, construir mensajes de log.
- **Entrada/Salida:** Mostrar mensajes al usuario, leer datos de formularios web, interactuar con APIs que devuelven texto.
- **Identificadores Únicos:** Aunque existen tipos más específicos, a veces se usan strings para IDs simples (aunque UUIDs o tipos numéricos suelen ser preferibles para claves primarias en bases de datos).
- **Internacionalización (i18n):** Almacenar y mostrar texto en diferentes idiomas.

#### Consideraciones Importantes

- **Inmutabilidad:** Las cadenas en JavaScript/TypeScript son inmutables. Cualquier operación que parezca modificar una cadena (como `toUpperCase()`, `substring()`, `replace()`) en realidad crea y devuelve una nueva cadena. Esto tiene implicaciones en el rendimiento si se realizan muchas manipulaciones en bucles intensivos. En tales casos, considera usar arrays de caracteres o constructores de strings si es necesario.
- **Codificación:** JavaScript utiliza internamente UTF-16. Esto puede llevar a comportamientos inesperados al trabajar con caracteres que requieren más de 16 bits (como algunos emojis), especialmente con métodos como `length` o al iterar sobre la cadena. Métodos modernos como `Array.from(str)` o el bucle `for...of` manejan mejor estos casos.

#### Buenas Prácticas

- Utilizar *template literals* (backticks) para interpolación y cadenas multilínea por su claridad.
- Ser consistente en el uso de comillas (simples o dobles) a lo largo del proyecto.
- Evitar la concatenación excesiva de strings en bucles (`+`), prefiriendo métodos como `Array.join('')` o template literals si es posible.

#### Malas Prácticas

- Usar el constructor `new String("texto")`. Esto crea un objeto `String`, no un primitivo `string`, lo cual puede llevar a errores sutiles y comparaciones inesperadas (`new String("a") !== new String("a")`). Siempre usa literales.
- Confiar ciegamente en `string.length` para caracteres multibyte (emojis, etc.).

---

### `number`

#### Definición y Utilidad

El tipo `number` representa tanto números enteros como de punto flotante. TypeScript (y JavaScript) utiliza el estándar IEEE 754 para representar números de punto flotante de doble precisión (64 bits).

```typescript
let edad: number = 30;
let precio: number = 99.95;
let temperatura: number = -5.5;
let infinitoPositivo: number = Infinity;
let noEsUnNumero: number = NaN; // Not a Number
```

`Infinity`, `-Infinity` y `NaN` son valores especiales del tipo `number`.

#### Casos de Uso Reales y Recomendables

- **Cálculos Matemáticos:** Operaciones aritméticas, financieras (con precauciones), científicas.
- **Contadores e Índices:** Bucles `for`, acceso a elementos de arrays.
- **Medidas y Cantidades:** Representar edad, peso, altura, stock, etc.
- **Coordenadas:** Posiciones en gráficos, mapas.

#### Consideraciones Importantes

{% hint style="warning" %}
**Precisión de Punto Flotante (IEEE 754):** Debido a la representación binaria, las operaciones con números decimales pueden producir resultados inesperados. Por ejemplo, `0.1 + 0.2` no es exactamente `0.3`. Para cálculos financieros o donde la precisión decimal exacta es crítica, **no uses el tipo `number` directamente**. Considera usar bibliotecas como `Decimal.js` o `Big.js`, o representa los valores en la unidad más pequeña (ej. céntimos en lugar de euros) usando enteros.
{% endhint %}

- **`NaN`:** Es un valor numérico especial que resulta de operaciones matemáticas indefinidas o incorrectas (ej. `Math.sqrt(-1)`, `parseInt("hola")`). `NaN` es el único valor en JavaScript que no es igual a sí mismo (`NaN === NaN` es `false`). Para comprobar si un valor es `NaN`, usa la función `Number.isNaN()`.
- **Límites Seguros:** JavaScript tiene límites para los enteros que pueden ser representados de forma segura (`Number.MAX_SAFE_INTEGER` y `Number.MIN_SAFE_INTEGER`). Operaciones fuera de este rango pueden perder precisión. Para números muy grandes, usa `bigint`.

#### Buenas Prácticas

- Usar `Number.isNaN()` para verificar si un valor es `NaN`.
- Ser consciente de los problemas de precisión con decimales y usar soluciones adecuadas (bibliotecas, representación en enteros) cuando sea necesario.
- Validar las entradas numéricas para evitar `NaN` inesperados.
- Usar `parseInt(string, radix)` o `parseFloat(string)` con precaución, especificando siempre la base (radix) para `parseInt` (generalmente 10).

#### Malas Prácticas

- Realizar comparaciones directas (`===`) con números de punto flotante que resultan de cálculos. Es mejor comprobar si la diferencia absoluta entre ellos es menor que un pequeño épsilon (tolerancia). `Math.abs(a - b) < Number.EPSILON`.
- Usar `number` para valores monetarios sin una estrategia para manejar la precisión decimal.
- Olvidar especificar la base (radix) en `parseInt()`, lo que puede llevar a interpretaciones octales inesperadas si la cadena empieza con `0`.

---

### `boolean`

#### Definición y Utilidad

El tipo `boolean` representa valores lógicos y solo puede tener dos valores: `true` o `false`. Es fundamental para el control de flujo (condicionales, bucles).

```typescript
let esValido: boolean = true;
let estaActivo: boolean = false;
let tienePermisos: boolean = checkPermissions(); // Función que devuelve boolean

if (esValido && tienePermisos) {
  console.log("Acceso concedido.");
} else {
  console.log("Acceso denegado.");
}
```

#### Casos de Uso Reales y Recomendables

- **Control de Flujo:** Decisiones en `if/else`, `switch`, condiciones en bucles `while`, `for`.
- **Flags (Banderas):** Indicar estados binarios (activado/desactivado, visible/oculto, completado/pendiente).
- **Resultados de Comparaciones:** Las operaciones de comparación (`===`, `!==`, `<`, `>`, `<=`, `>=`) siempre devuelven un booleano.
- **Validaciones:** Indicar si una entrada de usuario cumple ciertos criterios.

#### Consideraciones Importantes

- **Truthiness y Falsiness:** En JavaScript/TypeScript, ciertos valores se evalúan como `false` en contextos booleanos (condicionales, operador `!`). Estos son los valores "falsy": `false`, `0`, `""` (cadena vacía), `null`, `undefined`, `NaN`. Todos los demás valores se consideran "truthy". Comprender esto es vital, ya que `if (valor)` no siempre significa que `valor` sea estrictamente `true`.
- **Comparación Estricta:** Usa siempre el operador de igualdad estricta (`===`) y desigualdad estricta (`!==`) en lugar de los operadores de igualdad abstracta (`==`, `!=`). Los operadores abstractos realizan coerción de tipos, lo que puede llevar a errores difíciles de depurar (`0 == false` es `true`, pero `0 === false` es `false`).

#### Buenas Prácticas

- Usar nombres de variables booleanas descriptivos que indiquen una pregunta o un estado (ej. `isActive`, `isValid`, `hasPermission`).
- Utilizar la igualdad estricta (`===`, `!==`).
- Ser explícito en las comparaciones cuando la claridad lo requiera, en lugar de depender únicamente de la "truthiness" o "falsiness" si puede generar confusión. Por ejemplo, en lugar de `if (count)`, usar `if (count > 0)` si `count` es un número y quieres verificar si es positivo.

#### Malas Prácticas

- Usar los operadores de igualdad abstracta (`==`, `!=`).
- Usar el constructor `new Boolean(true)`. Esto crea un objeto `Boolean`, no un primitivo `boolean`, y los objetos siempre son "truthy", incluso `new Boolean(false)`.
- Devolver `null` o `undefined` en lugar de `false` desde funciones que lógicamente deberían devolver un booleano.

---

## Ausencia de Valor (`null` y `undefined`)

Estos dos tipos representan la ausencia de un valor, pero tienen sutiles diferencias semánticas y de uso.

### Definición y Utilidad

- **`undefined`:** Significa que una variable ha sido declarada pero aún no se le ha asignado un valor. También es el valor devuelto por funciones que no tienen una sentencia `return` explícita o que retornan `void`. Es la "ausencia de valor" por defecto del sistema.
- **`null`:** Representa la "ausencia intencional" de un valor de objeto. Se asigna explícitamente por el programador para indicar que una variable debería contener un objeto (o algún valor), pero actualmente no lo hace.

```typescript
let noAsignada: string; // Valor inicial: undefined
let valorNulo: string | null = null; // Asignación intencional de ausencia

function saludar(nombre?: string) { // Parámetro opcional, puede ser undefined
  if (nombre === undefined) {
    console.log("Hola, anónimo!");
  } else {
    console.log(`Hola, ${nombre}!`);
  }
}

saludar(); // Imprime "Hola, anónimo!"
// saludar(null); // Error si el tipo no incluye null explícitamente
```

### Diferencias Clave entre `null` y `undefined`

#### Semántica
- `undefined`: Valor no asignado, ausencia "accidental" o por defecto.
- `null`: Ausencia intencional de valor, asignada por el programador.

#### Tipo (`typeof`)
- `typeof undefined` devuelve `"undefined"`.
- `typeof null` devuelve `"object"`. ¡Este es un error histórico de JavaScript que persiste por compatibilidad! Es una fuente común de confusión.

#### Uso Común
- El sistema usa `undefined` (parámetros opcionales, variables no inicializadas, funciones sin `return`).
- Los programadores suelen usar `null` para indicar explícitamente que una variable que normalmente contendría un objeto está vacía.

#### Comparación
- `null == undefined` es `true` (igualdad abstracta, ¡evitar!).
- `null === undefined` es `false` (igualdad estricta, ¡preferir!).

### Casos de Uso Reales y Recomendables

- **`undefined`:**
    - Verificar si un parámetro opcional fue proporcionado.
    - Comprobar si una propiedad de objeto existe (aunque `in` o `hasOwnProperty` son más robustos).
    - Estado inicial de variables antes de asignarles un valor real.
- **`null`:**
    - Indicar que una variable que se espera contenga un objeto (ej. un nodo DOM, un registro de base de datos) no lo tiene actualmente.
    - Resetear o limpiar el valor de una variable de objeto.
    - Usado a menudo en APIs (ej. JSON) para representar explícitamente un valor faltante.

### Consideraciones Importantes

{% hint style="info" %}
**`strictNullChecks`:** La opción `strictNullChecks` en la configuración de TypeScript (`tsconfig.json`) es fundamental. Cuando está activada (recomendado), `null` y `undefined` no son asignables a otros tipos (como `string` o `number`) a menos que explícitamente los incluyas en una unión de tipos (ej. `string | null`). Esto previene una gran cantidad de errores relacionados con valores nulos o indefinidos.
{% endhint %}

- **Error `typeof null === "object"`:** Siempre ten presente este comportamiento anómalo al usar `typeof`. Para verificar si algo es `null`, usa la comparación estricta: `valor === null`.

### Buenas Prácticas

- **Activar `strictNullChecks`:** Es una de las características más valiosas de TypeScript.
- **Ser Explícito:** Usa uniones de tipos (`tipo | null` o `tipo | undefined`) cuando una variable pueda legítimamente carecer de valor.
- **Usar `=== null` y `=== undefined`:** Para comprobaciones explícitas.
- **Usar `??` (Nullish Coalescing Operator):** Para proporcionar valores por defecto de forma segura solo para `null` o `undefined`, sin afectar a otros valores "falsy" como `0` o `""`. `let valorFinal = valorRecibido ?? "predeterminado";`
- **Usar `?.` (Optional Chaining):** Para acceder a propiedades de objetos que podrían ser `null` o `undefined` de forma segura. `let nombre = usuario?.perfil?.nombre;`

### Malas Prácticas

- Usar `== null` o `== undefined` (igualdad abstracta) para comprobar ambos a la vez. Es menos explícito y propenso a errores que `===`.
- No activar `strictNullChecks`.
- Devolver `null` o `undefined` innecesariamente desde funciones cuando se podrían usar tipos más específicos o lanzar errores.
- Confiar en `typeof valor === "object"` para verificar si `valor` es un objeto real (podría ser `null`).

---

## `bigint`

### Definición y Utilidad

Introducido en ES2020, el tipo `bigint` permite representar números enteros de precisión arbitraria, superando las limitaciones de `Number.MAX_SAFE_INTEGER`. Se crea añadiendo una `n` al final de un literal numérico o usando la función `BigInt()`.

```typescript
// Error: número demasiado grande para 'number'
// let numeroGrandeNumber = 90071992547409919007199254740991;

let numeroGrande: bigint = 90071992547409919007199254740991n;
let otroGrande: bigint = BigInt("123456789012345678901234567890");
let sumaGrande: bigint = numeroGrande + otroGrande;

console.log(sumaGrande); // Imprime un número muy grande
```

### Casos de Uso Reales y Recomendables

- **Operaciones con Enteros Muy Grandes:** Criptografía, cálculos científicos que requieren alta precisión entera, identificadores únicos muy largos (ej. IDs de bases de datos distribuidas, snowflakes).
- **Interoperabilidad:** Interactuar con sistemas o APIs que usan enteros de 64 bits o más (ej. bases de datos con `BIGINT`, APIs financieras).
- **Evitar Pérdida de Precisión:** Cuando las operaciones con `number` exceden `Number.MAX_SAFE_INTEGER`.

### Consideraciones Importantes

{% hint style="danger" %}
**Incompatibilidad de Tipos:** No puedes mezclar directamente operandos `number` y `bigint` en operaciones aritméticas. Debes convertir explícitamente uno de los tipos.
```typescript
let a: bigint = 10n;
let b: number = 5;
// let sumaMixta = a + b; // Error de TypeScript!
let sumaMixtaCorrecta: bigint = a + BigInt(b); // OK
let productoMixtoCorrecto: number = Number(a) * b; // OK, pero puede perder precisión si 'a' es muy grande
```
{% endhint %}

- **Solo Enteros:** `bigint` solo puede representar números enteros. Intentar crear un `bigint` con decimales lanzará un error. Las divisiones entre `bigint` truncan el resultado hacia cero (como la división entera en otros lenguajes). `5n / 2n` resulta en `2n`.
- **Compatibilidad:** Asegúrate de que tu entorno de ejecución (navegador, Node.js) soporte `bigint` (generalmente versiones recientes). TypeScript lo transpilará si tu `target` en `tsconfig.json` es lo suficientemente bajo, pero puede requerir polyfills o tener limitaciones.
- **Rendimiento:** Las operaciones con `bigint` pueden ser más lentas que con `number` para números pequeños, ya que requieren más memoria y cálculos más complejos. Úsalo solo cuando sea necesario.

### Buenas Prácticas

- Usar `bigint` solo cuando se necesite manejar enteros que excedan `Number.MAX_SAFE_INTEGER`.
- Realizar conversiones explícitas (`BigInt()`, `Number()`) al mezclar `bigint` y `number`, siendo consciente de la posible pérdida de precisión al convertir `bigint` a `number`.
- Añadir siempre la `n` al final de los literales `bigint` para claridad y corrección.

### Malas Prácticas

- Usar `bigint` para números pequeños donde `number` es suficiente (impacto en rendimiento).
- Intentar realizar operaciones mixtas sin conversión explícita.
- Esperar que la división de `bigint` produzca decimales.
- Convertir `bigint` muy grandes a `number` sin considerar la posible pérdida de precisión.
