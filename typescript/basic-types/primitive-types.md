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

TypeScript, al ser un superconjunto de JavaScript, no solo hereda sus tipos de datos primitivos, sino que les añade una capa de seguridad y expresividad mediante el sistema de tipos estáticos. Estos tipos (`string`, `number`, `boolean`, `null`, `undefined`, `symbol` y `bigint`) constituyen los bloques de construcción fundamentales para cualquier aplicación. Dominar sus matices, usos y limitaciones es esencial para escribir código TypeScript que sea no solo funcional, sino también robusto, mantenible y eficiente.

Esta sección profundiza en cada uno de los tipos primitivos, explorando su definición, utilidad, casos de uso prácticos, consideraciones importantes, buenas y malas prácticas, y errores comunes a evitar.

## Tipos Primitivos Fundamentales (`string`, `number`, `boolean`)

Estos tres tipos son omnipresentes en casi cualquier fragmento de código JavaScript o TypeScript. Representan texto, números y valores lógicos, respectivamente.

### `string`

#### Definición y Utilidad

El tipo `string` en TypeScript se utiliza para representar secuencias de caracteres, es decir, datos textuales. Al igual que en JavaScript, las cadenas pueden definirse utilizando comillas simples (`'...'`), comillas dobles (`"..."`) o plantillas literales (backticks `` `...` ``).

La elección entre comillas simples o dobles suele ser una cuestión de preferencia estilística o de las guías de estilo del proyecto. Sin embargo, las plantillas literales ofrecen funcionalidades adicionales significativas:

1.  **Interpolación de expresiones:** Permiten incrustar fácilmente variables y expresiones JavaScript directamente dentro de la cadena utilizando la sintaxis `${expresion}`. Esto mejora drásticamente la legibilidad en comparación con la concatenación tradicional usando el operador `+`.
2.  **Cadenas multilínea:** Permiten crear cadenas que abarcan varias líneas sin necesidad de caracteres de escape especiales como `\\n`.

#### Ejemplos Básicos

```typescript
// Uso de comillas dobles
let firstName: string = "Alice";

// Uso de comillas simples
let message: string = 'Welcome to the system!';

// Uso de plantillas literales (backticks)
let lastName: string = "Wonderland";
let userGreeting: string = `User: ${firstName} ${lastName}. Status: Active.`;
let multiLineText: string = `This is line one.
This is line two.
The value is ${Math.random()}.`;

console.log(userGreeting);
// Output esperado: "User: Alice Wonderland. Status: Active."
console.log(multiLineText);
// Output esperado:
// This is line one.
// This is line two.
// The value is [un número aleatorio].
```

#### Casos de Uso Reales y Recomendables

El tipo `string` es increíblemente versátil y se utiliza en innumerables escenarios:

1.  **Interfaz de Usuario (UI):** Mostrar nombres, descripciones, mensajes de error, etiquetas de botones, y cualquier texto visible para el usuario. Las plantillas literales son especialmente útiles aquí para formatear dinámicamente la información.
2.  **Entrada de Datos:** Capturar y procesar información de formularios web (nombres, direcciones, comentarios). La validación de estas cadenas (longitud, formato) es crucial.
3.  **Manipulación y Formateo de Texto:** Construir URLs dinámicas, generar informes de texto, parsear y formatear fechas (aunque bibliotecas específicas suelen ser mejores), crear mensajes de log detallados.
4.  **Comunicación con APIs:** Enviar y recibir datos en formatos como JSON, donde los valores de cadena son muy comunes para claves y valores.
5.  **Identificadores:** Aunque no siempre es ideal para claves primarias (donde `number` o `UUID` pueden ser más eficientes), se usan para slugs de URL amigables, claves de configuración, nombres de eventos, etc.
6.  **Internacionalización (i18n) y Localización (l10n):** Almacenar plantillas de mensajes que luego se rellenan con datos específicos del idioma y la región del usuario.

#### Consideraciones Importantes

1.  **Inmutabilidad:** Este es un concepto clave. Una vez que se crea una cadena en JavaScript/TypeScript, su contenido no puede ser modificado directamente. Métodos como `toUpperCase()`, `substring()`, `replace()`, `trim()`, etc., no alteran la cadena original; en su lugar, devuelven una *nueva* cadena con el resultado de la operación.
    - **Implicación en Rendimiento:** En escenarios con manipulación intensiva de cadenas dentro de bucles (por ejemplo, construyendo una cadena grande mediante concatenación repetida con `+`), la creación constante de nuevas cadenas intermedias puede generar sobrecarga y afectar el rendimiento. Para estos casos, considera usar un array de strings y `join('')` al final, o investigar alternativas como `StringBuilder` si trabajas en entornos específicos (como Node.js con bibliotecas).
2.  **Codificación de Caracteres (UTF-16):** JavaScript (y por ende TypeScript) utiliza internamente la codificación UTF-16. La mayoría de los caracteres comunes (ASCII, latinos) ocupan una unidad de código de 16 bits. Sin embargo, caracteres fuera del Plano Multilingüe Básico (BMP), como muchos emojis o símbolos específicos, requieren *dos* unidades de código de 16 bits (un par sustituto o *surrogate pair*).
    - **Problemas Comunes:**
        - `string.length`: Devuelve el número de unidades de código UTF-16, no necesariamente el número de caracteres visibles. `'😂'.length` devuelve `2`.
        - Acceso por índice (`str[i]`): Accede a la unidad de código en esa posición, no al carácter completo si es un par sustituto.
        - Métodos antiguos (`substring`, `slice`): Pueden cortar un par sustituto por la mitad si no se usan con cuidado.
    - **Soluciones Modernas:**
        - Iteración con `for...of`: Itera sobre los caracteres reales (puntos de código Unicode).
        - `Array.from(str)`: Crea un array donde cada elemento es un carácter real. `Array.from('😂').length` devuelve `1`.
        - Métodos conscientes de Unicode (`String.prototype.codePointAt()`, `String.fromCodePoint()`).
3.  **Comparación de Cadenas:** La comparación (`<`, `>`, `<=`, `>=`) se basa en el valor numérico de las unidades de código UTF-16, no en el orden alfabético natural para todos los idiomas. Para una ordenación sensible al idioma, utiliza `String.prototype.localeCompare()`.

#### Buenas Prácticas

1.  **Usar Plantillas Literales (`` ` ``):** Preferirlas siempre para interpolación y cadenas multilínea debido a su legibilidad y conveniencia.
2.  **Consistencia en Comillas:** Elige un tipo de comillas (simples `'` o dobles `"`) para cadenas sin interpolación y sé consistente en todo el proyecto (utiliza linters como ESLint para enforcing).
3.  **Evitar Concatenación Excesiva en Bucles:** Usa `Array.prototype.join('')` para construir cadenas grandes a partir de múltiples partes de forma eficiente.
    ```typescript
    // Menos eficiente en bucles largos
    let result = "";
    for (let i = 0; i < 1000; i++) {
      result += `Item ${i}, `;
    }

    // Más eficiente
    const parts: string[] = [];
    for (let i = 0; i < 1000; i++) {
      parts.push(`Item ${i}`);
    }
    const efficientResult = parts.join(', ');
    ```
4.  **Manejar Correctamente Unicode:** Sé consciente de los pares sustitutos (emojis, etc.) y utiliza métodos modernos (`for...of`, `Array.from`, `localeCompare`) cuando trabajes con texto potencialmente complejo o multilingüe.
5.  **Validar y Sanitizar Entradas:** Nunca confíes en las cadenas que provienen de fuentes externas (usuarios, APIs). Valida su formato, longitud y sanitiza (escapa) caracteres especiales para prevenir vulnerabilidades como Cross-Site Scripting (XSS) si se van a renderizar en HTML.

#### Malas Prácticas

1.  **Usar el Constructor `new String("...")`:** Crear cadenas con `new String()` genera objetos `String`, no primitivos `string`. Esto causa comportamientos inesperados, especialmente con `typeof` (que devuelve `"object"`) y comparaciones estrictas (`===`), donde `new String("a") !== new String("a")` y `new String("a") !== "a"`. Usa siempre literales (`""`, `''`, `` `` ` ``).
2.  **Ignorar la Inmutabilidad:** Intentar modificar una cadena "in situ" o asumir que métodos como `replace` cambian la original. Esto lleva a errores lógicos donde los cambios esperados no ocurren.
3.  **Concatenación Ineficiente:** Usar `+` repetidamente dentro de bucles para construir cadenas grandes.
4.  **Manejo Incorrecto de Unicode:** Confiar ciegamente en `string.length` o acceso por índice cuando se trabaja con caracteres fuera del BMP.
5.  **Comparación Ingenua:** Usar `==` en lugar de `===` (aunque con strings primitivos la diferencia es menos problemática que con otros tipos, `===` es la práctica general recomendada). Usar comparadores `<` o `>` sin considerar las reglas de ordenación Unicode específicas del idioma si es necesario (`localeCompare`).
6.  **No Validar/Sanitizar Entradas:** Introducir riesgos de seguridad al usar cadenas externas directamente sin validación ni saneamiento.

---

### `number`

#### Definición y Utilidad

El tipo `number` en TypeScript y JavaScript se utiliza para representar valores numéricos, tanto enteros como de punto flotante. Sigue el estándar internacional IEEE 754 para la representación de números de punto flotante de doble precisión (64 bits). Esto significa que todos los números, ya sean `10`, `3.14`, `-0.5`, son del mismo tipo `number`.

Además de los números finitos, el tipo `number` incluye tres valores especiales:
- `Infinity`: Representa el infinito positivo (resultado de divisiones por cero o números que exceden el límite superior).
- `-Infinity`: Representa el infinito negativo.
- `NaN`: Acrónimo de "Not a Number" (No es un Número). Es el resultado de operaciones matemáticas inválidas o indefinidas, como `0 / 0`, `Math.sqrt(-1)`, o `parseInt("texto")`.

#### Ejemplos Básicos

```typescript
let integerValue: number = 42;
let floatValue: number = 3.14159;
let negativeValue: number = -100;
let exponentialValue: number = 2.5e6; // 2.5 * 10^6 = 2500000

let positiveInfinity: number = Infinity; // O también: Number.POSITIVE_INFINITY, 1 / 0
let negativeInfinity: number = -Infinity; // O también: Number.NEGATIVE_INFINITY, -1 / 0
let notANumber: number = NaN; // O también: Number.NaN, 0 / 0, parseInt("invalid")

console.log(typeof integerValue);       // "number"
console.log(typeof positiveInfinity);  // "number"
console.log(typeof notANumber);        // "number"
```

#### Casos de Uso Reales y Recomendables

1.  **Cálculos Matemáticos Generales:** Operaciones aritméticas básicas (suma, resta, multiplicación, división), porcentajes, promedios.
2.  **Contadores, Índices y Bucles:** Iterar un número específico de veces, acceder a elementos de arrays por su índice numérico.
3.  **Medidas Físicas y Científicas:** Representar magnitudes como temperatura, distancia, peso, velocidad (con conciencia de la precisión).
4.  **Coordenadas y Geometría:** Posiciones (x, y, z) en gráficos 2D/3D, cálculos de áreas, volúmenes.
5.  **Estadísticas y Probabilidades:** Cálculos de medias, desviaciones estándar, etc.
6.  **Identificadores Numéricos:** IDs autoincrementales en bases de datos (aunque `bigint` puede ser necesario para sistemas muy grandes).
7.  **Finanzas (con extrema precaución):** Útil para cálculos *simples* donde la precisión decimal exacta no es crítica o se maneja mediante redondeo controlado. **Para cálculos financieros serios, NUNCA usar `number` directamente.**

#### Consideraciones Importantes

1.  {% hint style="danger" %}
    **Precisión de Punto Flotante (IEEE 754):** ¡Esta es la consideración más crítica! Debido a que los números se almacenan en formato binario, muchos números decimales no pueden representarse con exactitud. El ejemplo clásico es `0.1 + 0.2`, que *no* es igual a `0.3` en JavaScript/TypeScript, sino a `0.30000000000000004`.
    - **Consecuencias:** Las comparaciones directas (`===`) entre resultados de cálculos con decimales son muy peligrosas. Operaciones financieras acumulativas pueden generar errores significativos.
    - **Soluciones:**
        - **Para Finanzas/Exactitud Decimal:**
            - **Bibliotecas:** Usar bibliotecas especializadas como `Decimal.js`, `Big.js` o `currency.js`. Estas implementan aritmética decimal de precisión arbitraria.
            - **Representación Entera:** Trabajar con la unidad más pequeña (ej., céntimos en lugar de euros). Almacena `1999` en lugar de `19.99`. Realiza todos los cálculos con enteros y solo convierte a formato decimal para mostrar al usuario.
        - **Para Comparaciones:** En lugar de `a === b`, comprueba si la diferencia absoluta es menor que una pequeña tolerancia (épsilon): `Math.abs(a - b) < Number.EPSILON`. `Number.EPSILON` es la diferencia entre 1 y el siguiente valor representable mayor que 1.
    {% endhint %}
2.  **`NaN` (Not a Number):**
    - Es "contagioso": Cualquier operación aritmética con `NaN` resulta en `NaN` (`NaN + 5` es `NaN`).
    - Es el único valor en JavaScript que no es igual a sí mismo: `NaN === NaN` es `false`.
    - **Comprobación Correcta:** Para verificar si un valor es `NaN`, **NO** uses `valor === NaN`. Utiliza la función `Number.isNaN(valor)`. Existe también `isNaN(valor)`, pero tiene un comportamiento diferente y a menudo indeseado (intenta convertir el argumento a número primero, por lo que `isNaN("hola")` es `true`), prefiere `Number.isNaN()`.
3.  **Límites Seguros de Enteros (`Number.MAX_SAFE_INTEGER`, `Number.MIN_SAFE_INTEGER`):**
    - JavaScript solo puede representar de forma segura y exacta enteros dentro del rango `[-(2^53 - 1), 2^53 - 1]`.
    - `Number.MAX_SAFE_INTEGER` es `9007199254740991`.
    - Operaciones con enteros fuera de este rango pueden producir resultados incorrectos debido a la pérdida de precisión.
    - **Solución:** Si necesitas trabajar con enteros más grandes, utiliza el tipo `bigint`.
4.  **División por Cero:** `1 / 0` resulta en `Infinity`, `-1 / 0` en `-Infinity`, y `0 / 0` en `NaN`.
5.  **Coerción de Tipos:** Funciones como `parseInt()` y `parseFloat()` pueden ser útiles, pero también fuentes de error si no se usan con cuidado.

#### Buenas Prácticas

1.  **Usar `Number.isNaN()`:** Siempre usa esta función para comprobar si un valor es `NaN`.
2.  **Manejar la Precisión Decimal:** Sé consciente del problema de IEEE 754. Utiliza bibliotecas (`Decimal.js`) o representación entera para cálculos financieros o que requieran exactitud decimal. Compara floats usando tolerancia (`Math.abs(a - b) < Number.EPSILON`).
3.  **Especificar Radix en `parseInt()`:** Cuando uses `parseInt(string, radix)`, especifica siempre la base (normalmente `10` para decimal). Si omites el radix, cadenas que empiezan con `0` podrían interpretarse como octales (comportamiento antiguo) o `0x` como hexadecimales, llevando a resultados inesperados. `parseInt("010")` sin radix puede dar `8`, mientras `parseInt("010", 10)` da `10`.
4.  **Validar Entradas Numéricas:** Antes de realizar cálculos, asegúrate de que las entradas son números válidos (y no `NaN`) y están dentro de los rangos esperados.
5.  **Considerar `bigint`:** Si existe la posibilidad de trabajar con enteros muy grandes (IDs, criptografía, etc.), usa `bigint` desde el principio.
6.  **Evitar la Magia de `typeof`:** No confíes en `typeof x === 'number'` para excluir `NaN` o `Infinity`. Si necesitas un número finito, usa `Number.isFinite(x)`.

#### Malas Prácticas

1.  **Ignorar Problemas de Precisión Decimal:** Usar `number` directamente para cálculos financieros críticos. Comparar floats con `===`.
2.  **Comprobar `NaN` con `=== NaN`:** `if (valor === NaN)` nunca funcionará.
3.  **Usar `isNaN()` Global sin Cuidado:** Preferir `Number.isNaN()` para evitar la coerción de tipos no deseada.
4.  **Omitir Radix en `parseInt()`:** Puede causar errores de interpretación de la base numérica.
5.  **Operar con Enteros Grandes sin `bigint`:** Riesgo de resultados incorrectos debido a la superación de `Number.MAX_SAFE_INTEGER`.
6.  **Usar el Constructor `new Number(...)`:** Similar a `new String()`, crea objetos `Number` en lugar de primitivos `number`, causando problemas con `typeof` y `===`. Usa literales numéricos.

---

### `boolean`

#### Definición y Utilidad

El tipo `boolean` es el tipo de datos lógico fundamental. Solo puede tener uno de dos valores: `true` o `false`. Su propósito principal es representar condiciones verdaderas o falsas, siendo la piedra angular para la toma de decisiones y el control del flujo de ejecución en la programación.

#### Ejemplos Básicos

```typescript
let isActive: boolean = true;
let hasErrors: boolean = false;
let isVisible: boolean = 10 > 5; // El resultado de una comparación es siempre boolean (true)
let canProceed: boolean = isActive && !hasErrors; // Combinación lógica (true)

function checkPermissions(userId: number): boolean {
  // Lógica para verificar permisos...
  return userId === 123; // Devuelve true si el ID es 123, false en caso contrario
}

let isAdmin: boolean = checkPermissions(123); // true

if (canProceed && isAdmin) {
  console.log("Processing...");
} else {
  console.log("Cannot proceed.");
}
```

#### Casos de Uso Reales y Recomendables

1.  **Control de Flujo Condicional:** Base de las sentencias `if`/`else`, `switch` (aunque menos común para booleanos puros), y el operador ternario (`condicion ? valorSiTrue : valorSiFalse`). Determinan qué bloques de código se ejecutan.
2.  **Condiciones de Bucles:** Controlar la continuación o terminación de bucles `while`, `do...while`, y `for` (en sus condiciones).
3.  **Flags (Banderas) de Estado:** Representar estados binarios simples: `isEnabled`, `isLoading`, `isValid`, `isChecked`, `isLoggedIn`, `showDetails`.
4.  **Resultados de Funciones de Validación/Comprobación:** Funciones que verifican si una condición se cumple (ej. `isValidEmail(email)`, `userExists(id)`, `hasPermission(role)`) típicamente devuelven `boolean`.
5.  **Configuración y Opciones:** Habilitar o deshabilitar características de una aplicación o componente.
6.  **Lógica Booleana Compleja:** Combinar múltiples condiciones usando operadores lógicos `&&` (AND), `||` (OR), y `!` (NOT) para tomar decisiones más elaboradas.

#### Consideraciones Importantes

1.  **Truthiness y Falsiness:** Este es un concepto crucial heredado de JavaScript. En contextos donde se espera un booleano (como en un `if`), JavaScript/TypeScript no exige estrictamente `true` o `false`. Ciertos valores se *evalúan* como falsos ("falsy") y todos los demás como verdaderos ("truthy").
    - **Valores Falsy:** `false`, `0` (cero numérico), `-0` (cero negativo), `""` (cadena vacía), `null`, `undefined`, `NaN`, `0n` (BigInt cero).
    - **Valores Truthy:** Cualquier otro valor, incluyendo `"0"`, `"false"`, `[]` (array vacío), `{}` (objeto vacío), `Infinity`, `-Infinity`, cualquier `string` no vacía, cualquier `number` diferente de cero, cualquier `bigint` diferente de cero.
    - **Implicaciones:** `if (value)` ejecutará el bloque si `value` es "truthy". Esto puede ser conveniente pero también fuente de errores si no se entiende bien. Por ejemplo, `if (userCount)` ejecutará el bloque si `userCount` es `1`, `-1`, o cualquier número excepto `0`, pero no si es `0`, lo cual podría ser lo deseado o no, dependiendo del contexto.
2.  {% hint style="danger" %}
    **Igualdad Estricta (`===`) vs. Abstracta (`==`):** ¡Usa siempre `===` y `!==`!
    - `===` (Igualdad Estricta): Comprueba igualdad sin realizar coerción de tipos. `false === 0` es `false`.
    - `==` (Igualdad Abstracta): Intenta convertir los operandos a un tipo común antes de comparar. Esto lleva a resultados confusos y a menudo erróneos: `false == 0` es `true`, `false == ""` es `true`, `null == undefined` es `true`. Evítala a toda costa.
    {% endhint %}
3.  **Operadores de Cortocircuito (`&&`, `||`):**
    - `&&` (AND lógico): Evalúa de izquierda a derecha. Si el operando izquierdo es "falsy", devuelve ese valor "falsy" *sin* evaluar el operando derecho. Si el izquierdo es "truthy", evalúa y devuelve el valor del operando derecho.
    - `||` (OR lógico): Evalúa de izquierda a derecha. Si el operando izquierdo es "truthy", devuelve ese valor "truthy" *sin* evaluar el operando derecho. Si el izquierdo es "falsy", evalúa y devuelve el valor del operando derecho.
    - Útil para valores por defecto (aunque `??` suele ser mejor) o ejecución condicional concisa.

#### Buenas Prácticas

1.  **Nombres Descriptivos:** Usa nombres de variables que suenen como preguntas o estados claros: `isActive`, `isValid`, `shouldUpdate`, `hasPermission`, `isLoading`.
2.  **Usar Igualdad Estricta (`===`, `!==`):** Fundamental para evitar errores de coerción de tipos.
3.  **Ser Explícito:** Aunque la "truthiness/falsiness" puede ser útil, prefiere comparaciones explícitas si mejora la claridad. En lugar de `if (items.length)`, es más claro `if (items.length > 0)` si quieres asegurarte de que hay elementos (y no solo que `length` no es `0`). En lugar de `if (!errorMessage)`, es más claro `if (errorMessage === "")` o `if (errorMessage === null)` dependiendo de lo que quieras comprobar exactamente.
4.  **Funciones Claras:** Las funciones que realizan comprobaciones deben devolver claramente `true` o `false`, no `null` o `undefined` para indicar "falso".
5.  **Evitar Doble Negación Innecesaria:** `if (!!value)` es redundante, `if (value)` tiene el mismo efecto lógico. `!!value` se usa a veces para *convertir* explícitamente un valor a su equivalente booleano primitivo (`true` o `false`), pero raramente es necesario dentro de un `if`.

#### Malas Prácticas

1.  **Usar Igualdad Abstracta (`==`, `!=`):** La fuente número uno de errores relacionados con booleanos y tipos.
2.  **Usar el Constructor `new Boolean(...)`:** Crea objetos `Boolean`, no primitivos. `typeof new Boolean(false)` es `"object"`, y los objetos son *siempre* "truthy" en contextos booleanos, incluso `new Boolean(false)`. ¡Esto es extremadamente confuso! `if (new Boolean(false))` se ejecutará. Usa siempre `true` y `false`.
3.  **Confiar Excesivamente en Truthiness/Falsiness:** Puede oscurecer la intención del código si no está claro qué condición exacta se está comprobando (ej. ¿`if (value)` comprueba si no es `null`, no es `undefined`, no es `0`, no es `""`?).
4.  **Devolver `null` o `undefined` en lugar de `false`:** Hace que el código que llama a la función sea más complejo, ya que tiene que comprobar múltiples valores "falsy".
5.  **Comparar Directamente con `true` o `false` (Redundante):**
    - `if (isValid === true)` es redundante, simplemente usa `if (isValid)`.
    - `if (hasErrors === false)` es redundante, simplemente usa `if (!hasErrors)`.

---

## Ausencia de Valor (`null` y `undefined`)

Estos dos tipos primitivos representan la ausencia de un valor significativo, pero tienen orígenes y usos semánticos distintos, aunque a menudo causan confusión. Comprender su diferencia es vital, especialmente cuando se trabaja con la opción `strictNullChecks` de TypeScript.

### Definición y Utilidad

- **`undefined`:**
    - **Semántica:** Representa una variable que ha sido declarada pero a la que *aún no se le ha asignado un valor*. Es el valor "por defecto" para variables no inicializadas, parámetros de función opcionales que no se proporcionan, propiedades de objeto inexistentes y el valor de retorno implícito de funciones que no tienen una sentencia `return`. Indica una ausencia de valor más bien "accidental" o por el sistema.
    - **Origen:** Generalmente asignado por el motor de JavaScript.
- **`null`:**
    - **Semántica:** Representa la *ausencia intencional* de cualquier valor de objeto (o valor en general). Se utiliza explícitamente por el programador para indicar que una variable, que normalmente podría contener un objeto o un valor, deliberadamente no lo tiene en este momento. Es una asignación consciente de "nada".
    - **Origen:** Asignado explícitamente por el código del programador.

#### Ejemplos Ilustrativos

```typescript
let notInitialized: string; // Declared but not assigned value
console.log(notInitialized); // Output: undefined
console.log(typeof notInitialized); // Output: "undefined"

let intentionallyEmpty: string | null = null; // Programmer explicitly set it to null
console.log(intentionallyEmpty); // Output: null
console.log(typeof intentionallyEmpty); // Output: "object" (¡Error histórico!)

function greet(name?: string) { // Optional parameter, defaults to undefined if not passed
  if (name === undefined) {
    console.log("Hello, guest!");
  } else {
    console.log(`Hello, ${name}!`);
  }
}

greet(); // Output: Hello, guest!
greet("Bob"); // Output: Hello, Bob!
// greet(null); // Error if 'strictNullChecks' is on and type isn't 'string | null'

const user = { id: 1, name: "Alice" };
console.log(user.name); // Output: "Alice"
// console.log(user.email); // Error in TS if 'email' isn't defined. In JS, it would be 'undefined'.

function doNothing() {
  // No return statement
}
let result = doNothing();
console.log(result); // Output: undefined
```

### Diferencias Clave Resumidas

#### Tabla Comparativa

| Característica      | `undefined`                                               | `null`                                          |
| :------------------ | :-------------------------------------------------------- | :---------------------------------------------- |
| **Significado**     | Valor aún no asignado; ausencia "accidental"              | Ausencia intencional de valor; "nada" explícito |
| **Origen Típico**   | Motor de JavaScript, omisión                              | Asignación explícita del programador            |
| **`typeof`**        | `"undefined"`                                             | `"object"` (¡Error histórico!)                  |
| **Conversión Num.** | `NaN`                                                     | `0`                                             |
| **Uso Común**       | Valor por defecto, params opcionales, props no existentes | Indicar ausencia explícita de objeto/valor      |
| **`==` vs `===`**   | `null == undefined` es `true` (¡Evitar!)                  | `null === undefined` es `false` (¡Usar!)        |

### Casos de Uso Reales y Recomendables

- **`undefined`:**
    - **Valores por Defecto:** Comprobar si se proporcionó un parámetro opcional a una función.
    - **Propiedades Opcionales:** Indicar que una propiedad en un objeto o interfaz puede no estar presente.
    - **Estado No Inicializado:** Representar el estado inicial de una variable antes de que reciba su valor real (aunque a menudo es mejor inicializar con `null` si se espera un objeto).
- **`null`:**
    - **Ausencia Explícita de Objeto:** Cuando una variable se espera que contenga una referencia a un objeto (ej. un nodo del DOM, datos de usuario, un recurso), pero actualmente no lo tiene (ej. `document.getElementById('nonExistent')` devuelve `null`).
    - **Resetear/Limpiar:** Asignar `null` a una variable para liberar la referencia a un objeto anterior (ayudando potencialmente al recolector de basura) o para indicar que ya no es válida.
    - **APIs y Serialización (JSON):** `null` es un tipo válido en JSON y se usa comúnmente en APIs para indicar explícitamente que un campo no tiene valor, lo cual es diferente a que el campo no exista (`undefined`).
    - **Patrón de Objeto Nulo (Null Object Pattern):** Aunque no es `null` primitivo, a veces se usa `null` como señal para usar un objeto sustituto con comportamiento por defecto.

### Consideraciones Importantes

1.  {% hint style="info" %}
    **`strictNullChecks` (tsconfig.json):** ¡Actívala siempre! Es una de las características más valiosas de TypeScript.
    - **Sin `strictNullChecks` (comportamiento por defecto antiguo):** `null` y `undefined` son asignables a *cualquier* tipo (`string`, `number`, etc.). Esto lleva a errores en tiempo de ejecución cuando intentas usar un método en `null` (ej. `miString.toUpperCase()` donde `miString` es `null`).
    - **Con `strictNullChecks` (recomendado):** `null` y `undefined` son tipos distintos y no puedes asignarlos a variables de otros tipos a menos que explícitamente lo permitas usando una **unión de tipos** (ej. `string | null`, `number | undefined`, `User | null`). Esto fuerza al programador a manejar explícitamente los casos donde un valor podría ser `null` o `undefined` *antes* de usarlo, previniendo errores comunes.
    {% endhint %}
2.  **El Error de `typeof null === "object"`:** Nunca olvides esta peculiaridad histórica. Si necesitas comprobar si algo es específicamente `null`, usa `valor === null`. Si necesitas comprobar si es un objeto *real* (no `null`), usa `typeof valor === "object" && valor !== null`.
3.  **Coerción a Booleano:** Tanto `null` como `undefined` son valores "falsy".
4.  **Coerción a Número:** `Number(null)` es `0`, mientras que `Number(undefined)` es `NaN`. Otra diferencia sutil pero potencialmente problemática.
5.  **Operadores Modernos para Manejar Ausencia:**
    - **`??` (Nullish Coalescing Operator / Operador de Fusión Nuloso):** Proporciona un valor por defecto *solo* si el operando izquierdo es `null` o `undefined`. Ignora otros valores "falsy" como `0` o `""`. Es generalmente preferible a `||` para valores por defecto.
      ```typescript
      let userInput = "";
      let setting = null;
      let count = 0;

      // Usando || (OR lógico) - Puede dar resultados no deseados
      let username = userInput || "guest"; // username será "guest" (porque "" es falsy)
      let config = setting || { default: true }; // config será { default: true }
      let displayCount = count || -1; // displayCount será -1 (porque 0 es falsy)

      // Usando ?? (Nullish Coalescing) - Más preciso
      let usernameStrict = userInput ?? "guest"; // usernameStrict será ""
      let configStrict = setting ?? { default: true }; // configStrict será { default: true }
      let displayCountStrict = count ?? -1; // displayCountStrict será 0
      ```
    - **`?.` (Optional Chaining / Encadenamiento Opcional):** Permite acceder a propiedades anidadas de un objeto sin errores si alguna parte de la cadena es `null` o `undefined`. Si encuentra `null` o `undefined` en cualquier punto, la expresión entera cortocircuita y devuelve `undefined`.
      ```typescript
      const user: { profile?: { address?: { street: string } } } | null = getUser();

      // Sin Optional Chaining (propenso a errores o código verboso)
      // const street = user && user.profile && user.profile.address && user.profile.address.street;

      // Con Optional Chaining (seguro y conciso)
      const street = user?.profile?.address?.street; // street será undefined si user, profile, o address es null/undefined

      // También funciona con llamadas a funciones opcionales
      user?.profile?.logActivity?.(); // Llama a logActivity solo si existe
      ```

### Buenas Prácticas

1.  **Activar `strictNullChecks`:** Imprescindible para un código TypeScript robusto.
2.  **Ser Explícito con Uniones:** Usa `tipo | null` o `tipo | undefined` cuando un valor pueda estar ausente legítimamente. Evita tipos implícitos `any` o no especificar tipos.
3.  **Usar `=== null` y `=== undefined`:** Para comprobaciones claras y explícitas. Evita `==`.
4.  **Preferir `??` sobre `||` para Valores por Defecto:** A menos que quieras tratar explícitamente `0` o `""` como casos para el valor por defecto.
5.  **Usar `?.` para Acceso Seguro a Propiedades:** Simplifica enormemente el código que maneja objetos potencialmente nulos o con propiedades opcionales.
6.  **Consistencia Semántica:** Usa `null` para indicar una ausencia *intencional* de valor (especialmente de objetos) y deja que `undefined` represente el estado no inicializado o ausente por defecto del sistema.
7.  **Evitar Devolver `null`/`undefined` Innecesariamente:** Si una función puede fallar, considera lanzar un error o devolver un tipo de resultado más explícito (como un objeto con estado y valor/error) en lugar de simplemente devolver `null` o `undefined`, lo cual puede ocultar la causa del fallo.

### Malas Prácticas

1.  **No Usar `strictNullChecks`:** Deja la puerta abierta a innumerables errores `TypeError: Cannot read property '...' of null/undefined`.
2.  **Usar `== null` o `== undefined`:** Código menos claro y propenso a errores debido a la coerción abstracta.
3.  **Confiar en `typeof valor === "object"` para Excluir `null`:** Recuerda que `typeof null` es `"object"`.
4.  **Usar `||` para Valores por Defecto con `0` o `""`:** Si `0` o `""` son valores válidos y significativos, `||` los sobrescribirá incorrectamente. Usa `??`.
5.  **Anidamiento Excesivo de Comprobaciones `&&`:** Antes de `?.`, era común ver `if (a && a.b && a.b.c)`. El encadenamiento opcional (`a?.b?.c`) es mucho más limpio.
6.  **Asignar `undefined` Explícitamente:** Aunque técnicamente posible (`let x = undefined;`), generalmente es menos claro que dejar que una variable no inicializada sea `undefined` por defecto o usar `null` si la intención es una ausencia explícita. Hay excepciones, pero `null` suele comunicar mejor la intención de "vacío".

---

## `bigint`

### Definición y Utilidad

Introducido en ECMAScript 2020 (ES11) y soportado en TypeScript, `bigint` es un tipo numérico primitivo diseñado para representar números enteros con precisión arbitraria. Resuelve la limitación fundamental del tipo `number`, que solo puede representar de forma segura enteros hasta `Number.MAX_SAFE_INTEGER` (2<sup>53</sup> - 1) debido a su representación IEEE 754 de 64 bits.

Los `bigint` se crean de dos maneras:
1.  Añadiendo una `n` al final de un literal numérico entero: `12345n`.
2.  Llamando a la función `BigInt()` con un número o una cadena como argumento: `BigInt(9007199254740992)`, `BigInt("12345678901234567890")`.

#### Ejemplos de Creación y Operación

```typescript
const limit = Number.MAX_SAFE_INTEGER;
console.log(limit); // 9007199254740991

const numProblem = limit + 1;
const numProblem2 = limit + 2;
console.log(numProblem === numProblem2); // Output: true (¡Error de precisión con 'number'!)

const bigIntOk = BigInt(limit) + 1n; // 9007199254740992n
const bigIntOk2 = BigInt(limit) + 2n; // 9007199254740993n
console.log(bigIntOk);
console.log(bigIntOk2);
console.log(bigIntOk === bigIntOk2); // Output: false (Correcto con 'bigint')

const veryLarge = 123456789012345678901234567890n;
const anotherLarge = BigInt("987654321098765432109876543210");
const sumLarge = veryLarge + anotherLarge;

console.log(sumLarge); // Imprime la suma exacta, un número muy grande
console.log(typeof sumLarge); // Output: "bigint"

// División entera (trunca decimales)
const division = 10n / 3n;
console.log(division); // Output: 3n (no 3.333...)
```

#### Casos de Uso Reales y Recomendables

1.  **Operaciones con Enteros Muy Grandes:**
    - **Criptografía:** Muchos algoritmos criptográficos operan con números enteros extremadamente grandes.
    - **Cálculos Científicos/Matemáticos:** Cuando se requiere precisión exacta con enteros que superan los límites de `number`.
    - **IDs Únicos de Alta Precisión:** Sistemas distribuidos a gran escala a menudo usan IDs de 64 bits o más (ej., IDs de Twitter Snowflake, IDs de bases de datos con `BIGINT` como tipo de columna). `number` no puede almacenar estos IDs sin pérdida de precisión.
2.  **Interoperabilidad con Sistemas Externos:**
    - **Bases de Datos:** Interactuar con columnas de tipo `BIGINT` u otros tipos de enteros grandes.
    - **APIs:** Consumir o enviar datos a APIs que utilizan enteros de 64 bits o más (común en lenguajes como Java, C++, Go).
    - **Timestamp de Alta Precisión:** Representar timestamps con precisión de nanosegundos o microsegundos como un único entero grande.
3.  **Evitar Pérdida de Precisión:** Cualquier escenario donde las operaciones con enteros puedan superar `Number.MAX_SAFE_INTEGER` y se requiera exactitud.

### Consideraciones Importantes

1.  {% hint style="danger" %}
    **Incompatibilidad de Tipos Estricta:** No puedes mezclar operandos `number` y `bigint` directamente en la mayoría de las operaciones aritméticas (`+`, `-`, `*`, `/`, `%`, `**`, etc.) ni en comparaciones (`<`, `>`, `<=`, `>=`). Hacerlo provocará un `TypeError` en tiempo de ejecución y un error en tiempo de compilación en TypeScript.
    - **Solución:** Debes convertir explícitamente uno de los tipos al otro *antes* de la operación.
      ```typescript
      let big: bigint = 100n;
      let num: number = 5;

      // INCORRECTO:
      // let mixedSum = big + num; // TypeError
      // let mixedComp = big > num; // TypeError

      // CORRECTO (convertir number a bigint):
      let sumAsBigInt: bigint = big + BigInt(num); // 105n

      // CORRECTO (convertir bigint a number - ¡CUIDADO CON LA PRECISIÓN!)
      // Solo seguro si 'big' está dentro del rango seguro de 'number'
      if (big >= BigInt(Number.MIN_SAFE_INTEGER) && big <= BigInt(Number.MAX_SAFE_INTEGER)) {
          let sumAsNumber: number = Number(big) + num; // 105
          let comparisonAsNumbers: boolean = Number(big) > num; // true
      } else {
          console.warn("BigInt too large to safely convert to Number for operation");
          // Podrías optar por convertir 'num' a BigInt en su lugar
          let comparisonAsBigInts: boolean = big > BigInt(num); // true
      }
      ```
    - **Excepción:** Los operadores de comparación `==` y `===` *sí* funcionan entre `number` y `bigint`, pero siempre devuelven `false` porque son tipos diferentes (`10n === 10` es `false`). `==` también devuelve `false` en la mayoría de los casos prácticos.
    {% endhint %}
2.  **Solo Enteros:** `bigint` está diseñado exclusivamente para números enteros.
    - No puedes crear un `bigint` con decimales (`12.3n` es un error de sintaxis).
    - La función `BigInt()` truncará cualquier parte decimal si se le pasa un `number` (`BigInt(5.9)` es `5n`).
    - La división (`/`) entre dos `bigint` siempre realiza una división entera, truncando el resultado hacia cero (`7n / 2n` es `3n`, `-7n / 2n` es `-3n`). No hay residuo ni parte fraccionaria.
3.  **Compatibilidad del Entorno:** `bigint` es una característica relativamente moderna (ES2020). Asegúrate de que tu entorno de ejecución (navegador de destino, versión de Node.js) lo soporte. TypeScript puede transpilar la sintaxis si tu `target` en `tsconfig.json` es anterior (ej. `es2016`), pero requerirá polyfills o puede tener limitaciones de rendimiento o funcionalidad dependiendo de la configuración de transpilación. Revisa la configuración de tu compilador y las tablas de compatibilidad (como MDN o Can I use).
4.  **Rendimiento:** Las operaciones con `bigint` son generalmente más lentas que las operaciones equivalentes con `number` para números que caben dentro del rango seguro de `number`. Esto se debe a que `bigint` requiere asignación de memoria dinámica y algoritmos más complejos para manejar la precisión arbitraria. Úsalo solo cuando sea estrictamente necesario por razones de precisión o tamaño.
5.  **Serialización (JSON):** `JSON.stringify()` no soporta `bigint` directamente y lanzará un `TypeError`. Debes convertir los `bigint` a `string` (o `number` si es seguro) antes de serializar, y convertirlos de nuevo al deserializar.
    ```typescript
    const data = { id: 123n, value: "example" };
    // const jsonString = JSON.stringify(data); // TypeError

    // Solución: Convertir a string (o usar una biblioteca con soporte)
    const jsonStringSafe = JSON.stringify(data, (key, value) =>
      typeof value === 'bigint' ? value.toString() + 'n' : value // Añadir 'n' para identificar al deserializar
    );
    console.log(jsonStringSafe); // {"id":"123n","value":"example"}

    const parsedData = JSON.parse(jsonStringSafe, (key, value) => {
      if (typeof value === 'string' && /^-?\d+n$/.test(value)) {
          return BigInt(value.slice(0, -1));
      }
      return value;
    });
    console.log(parsedData.id === 123n); // true
    ```

### Buenas Prácticas

1.  **Usar `bigint` Específicamente:** Empléalo solo cuando necesites representar enteros fuera del rango seguro de `number` (`> Number.MAX_SAFE_INTEGER` o `< Number.MIN_SAFE_INTEGER`) o cuando interactúes con sistemas que usen enteros de 64 bits o más.
2.  **Conversión Explícita y Segura:** Siempre convierte explícitamente entre `number` y `bigint` usando `BigInt()` y `Number()`. Sé extremadamente cuidadoso al convertir `bigint` a `number` y verifica si el valor está dentro del rango seguro para evitar pérdida de precisión.
3.  **Sufijo `n`:** Utiliza siempre el sufijo `n` para los literales `bigint` (`10n`, `0n`, `-5n`). Mejora la claridad y evita errores.
4.  **Manejo de Serialización:** Implementa una lógica personalizada (usando el segundo argumento de `JSON.stringify` y el reviver de `JSON.parse`) o usa bibliotecas que manejen la serialización/deserialización de `bigint` si trabajas con JSON.

### Malas Prácticas

1.  **Usar `bigint` Innecesariamente:** Aplicarlo a números que caben perfectamente en `number` puede impactar negativamente el rendimiento sin aportar beneficios.
2.  **Mezclar Tipos Implícitamente:** Intentar realizar operaciones aritméticas o comparaciones directas entre `number` y `bigint` sin conversión explícita.
3.  **Esperar Decimales en la División:** Asumir que `5n / 2n` dará `2.5n`. La división de `bigint` es siempre entera.
4.  **Conversión Ciega de `bigint` a `number`:** Convertir `bigint` grandes a `number` sin verificar si exceden los límites seguros, lo que lleva a pérdida de precisión.
5.  **Ignorar la Serialización JSON:** No manejar la conversión a/desde string al trabajar con `JSON.stringify` y `JSON.parse`, lo que resultará en errores.

