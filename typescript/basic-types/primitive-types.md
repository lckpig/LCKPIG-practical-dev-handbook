<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/primitive-types)
<!-- MULTILANGUAJE MENU END -->

<!--
# Tipos primitivos en TypeScript

- `string`, `number`, `boolean`, `null`, `undefined`
- Diferencias entre `null` y `undefined`
- Uso de `bigint` para operaciones con grandes números
-->

<details id="toc-container">
<summary>Índice de contenidos</summary>

<!-- no toc -->
- [Los Tipos Primitivos Fundamentales: `string`, `number`, `boolean`](#los-tipos-primitivos-fundamentales-string-number-boolean)
  - [`string`](#string)
  - [`number`](#number)
  - [`boolean`](#boolean)
  - [Buenas Prácticas con `string`, `number`, `boolean`](#buenas-practicas-con-string-number-boolean)
  - [Malas Prácticas](#malas-practicas)
- [Ausencia de Valor: `null` y `undefined`](#ausencia-de-valor-null-y-undefined)
  - [`undefined`](#undefined)
  - [`null`](#null)
  - [Diferencias Clave y Cuándo Usar Cada Uno](#diferencias-clave-y-cuando-usar-cada-uno)
  - [Buenas Prácticas con `null` y `undefined`](#buenas-practicas-con-null-y-undefined)
  - [Malas Prácticas](#malas-practicas-1)
- [Manejando Números Grandes: `bigint`](#manejando-numeros-grandes-bigint)
  - [`bigint`](#bigint)
  - [Consideraciones Importantes sobre `bigint`](#consideraciones-importantes-sobre-bigint)
  - [Buenas Prácticas con `bigint`](#buenas-practicas-con-bigint)
  - [Malas Prácticas](#malas-practicas-2)

</details>

# Tipos primitivos en TypeScript

TypeScript, al ser un superconjunto de JavaScript, hereda sus tipos de datos primitivos fundamentales. Estos tipos son la base sobre la cual se construyen estructuras de datos más complejas. Comprenderlos a fondo es esencial para escribir código TypeScript robusto y eficiente.

## Los Tipos Primitivos Fundamentales: `string`, `number`, `boolean`

Estos tres tipos son probablemente los más utilizados en cualquier aplicación. Representan texto, números y valores de verdad, respectivamente.

### `string`

El tipo `string` se utiliza para representar datos textuales. TypeScript, al igual que JavaScript, utiliza comillas simples (`'`), comillas dobles (`"`) o plantillas literales (backticks `` ` ``) para delimitar las cadenas de texto.

#### Ejemplo: Declaración de Strings

```typescript
let nombre: string = "Alice";
let saludo: string = 'Hola, ' + nombre;
let edad: number = 30;
// Las plantillas literales permiten la interpolación de variables y expresiones
let mensajeCompleto: string = `Hola, me llamo ${nombre} y tengo ${edad} años.`;

console.log(nombre); // Salida: Alice
console.log(saludo); // Salida: Hola, Alice
console.log(mensajeCompleto); // Salida: Hola, me llamo Alice y tengo 30 años.
```

[↑ Volver al Índice](#toc-container)


### `number`

El tipo `number` representa tanto números enteros como de punto flotante. TypeScript utiliza el estándar IEEE 754 para representar números, lo que implica ciertas particularidades en cuanto a precisión y límites.

#### Ejemplo: Operaciones Numéricas

```typescript
let cantidad: number = 10;
let precioUnitario: number = 49.99;
let total: number = cantidad * precioUnitario; // Multiplicación
let descuento: number = 0.1; // 10% de descuento
let precioFinal: number = total * (1 - descuento);

console.log(total);         // Salida: 499.9
console.log(precioFinal);   // Salida: 449.91
```

[↑ Volver al Índice](#toc-container)


### `boolean`

El tipo `boolean` representa un valor lógico y solo puede tener dos valores: `true` o `false`. Es fundamental para la lógica condicional y el control de flujo.

#### Ejemplo: Uso de Booleanos en Condicionales

```typescript
let esMayorDeEdad: boolean = edad >= 18; // edad fue definida como 30 previamente
let tienePermiso: boolean = false;

if (esMayorDeEdad && tienePermiso) {
  console.log("Puede conducir.");
} else if (esMayorDeEdad) {
  console.log("Es mayor de edad, pero no tiene permiso para conducir.");
} else {
  console.log("No es mayor de edad.");
}
// Salida: Es mayor de edad, pero no tiene permiso para conducir.
```

[↑ Volver al Índice](#toc-container)


### Buenas Prácticas con `string`, `number`, `boolean`

*   **Consistencia en Comillas**: Elige un tipo de comillas (simples o dobles) y sé consistente en todo tu proyecto para mejorar la legibilidad. Las plantillas literales son preferibles cuando necesitas interpolación, cadenas multilínea o trabajar con internacionalización (i18n) facilitando la inserción de variables en textos traducidos.
*   **Evitar `Number` vs `number`**: Usa siempre el tipo primitivo `number` en lugar del objeto `Number`. Lo mismo aplica para `string` vs `String` y `boolean` vs `Boolean`. Los tipos primitivos son más eficientes y evitan comportamientos inesperados.
*   **Claridad en Booleanos**: Nombra las variables booleanas de forma descriptiva, preferiblemente como preguntas o afirmaciones (ej. `isActive`, `hasPermission`, `isValid`).

[↑ Volver al Índice](#toc-container)


### Malas Prácticas

*   **Comparaciones Imprecisas**: Ten cuidado al comparar números de punto flotante directamente debido a posibles problemas de precisión. En lugar de `a === b`, considera `Math.abs(a - b) < umbralPequeño`.

{% hint style="warning" %}
La comparación directa de números de punto flotante (`===`) puede fallar debido a cómo se representan internamente. Utilizar un umbral pequeño (`epsilon`) para las comparaciones es una práctica más segura para evitar errores inesperados.
{% endhint %}

*   **Confiar Ciegamente en la Coerción de Tipos**: JavaScript realiza coerción automática de tipos en ciertas operaciones (ej. `1 == '1'`). TypeScript ayuda a prevenir esto, pero es bueno ser explícito y evitar depender de la coerción implícita.

[↑ Volver al Índice](#toc-container)


---

## Ausencia de Valor: `null` y `undefined`

Estos dos tipos representan la ausencia de un valor, pero tienen diferencias sutiles en su semántica y uso.

### `undefined`

Una variable a la que no se le ha asignado un valor tiene el valor `undefined`. También es el valor que retornan las funciones que no tienen una sentencia `return` explícita o que retornan `void`. Se considera generalmente como la ausencia de valor "no intencional" o "aún no asignado".

### `null`

`null` representa la ausencia intencional de un valor de objeto. Cuando asignas `null` a una variable, estás indicando explícitamente que esa variable no referencia a ningún objeto en ese momento.

### Diferencias Clave y Cuándo Usar Cada Uno

Aunque ambos representan la ausencia de valor, sus usos y significados difieren:

| Característica  | `undefined`                                 | `null`                                               |
| --------------- | ------------------------------------------- | ---------------------------------------------------- |
| **Significado** | Valor no asignado (aún) o inexistente.      | Valor explícitamente asignado como "sin objeto".     |
| **Asignación**  | Automática para variables no inicializadas. | Debe ser asignada explícitamente por el programador. |
| **`typeof`**    | `"undefined"`                               | `"object"` (peculiaridad histórica)                  |
| **Uso Común**   | Parámetros opcionales, props no pasados.    | Retorno de APIs (objeto o nada), resetear objetos.   |
| **Contexto**    | Ausencia "por defecto" o no intencional.    | Ausencia intencional y significativa.                |

*   **Intencionalidad**: `null` es asignado intencionalmente por el programador para indicar "sin valor". `undefined` suele ser el valor por defecto de variables no inicializadas o parámetros de función omitidos.

{% hint style="warning" %}
El hecho de que `typeof null` devuelva `"object"` es una peculiaridad histórica de JavaScript que persiste por razones de compatibilidad. No significa que `null` sea un objeto real. Ten esto en cuenta al verificar tipos.
{% endhint %}

*   **Tipo**: `typeof null` devuelve `"object"` (un error histórico en JavaScript). `typeof undefined` devuelve `"undefined"`.

*   **Uso Común**:
    *   Usa `undefined` para verificar si una variable ha sido inicializada o si un parámetro opcional fue proporcionado.
    *   Usa `null` cuando quieras indicar explícitamente que una variable que normalmente contendría un objeto no tiene valor actualmente (por ejemplo, en APIs que devuelven un objeto o `null`).

#### Ejemplo: Diferencias entre `null` y `undefined`

```typescript
let valorNoDefinido: undefined; // Automáticamente es undefined
let valorNulo: null = null;     // Asignación explícita de null
let objetoUsuario: { nombre: string } | null = null; // Puede ser un objeto o null

console.log(valorNoDefinido); // Salida: undefined
console.log(typeof valorNoDefinido); // Salida: undefined

console.log(valorNulo); // Salida: null
console.log(typeof valorNulo); // Salida: object (¡Cuidado con esto!)

// Caso de uso: función con parámetro opcional
function saludar(nombre?: string) {
  if (nombre === undefined) {
    console.log("Hola, invitado!");
  } else {
    console.log(`Hola, ${nombre}!`);
  }
}

saludar(); // Salida: Hola, invitado!
saludar("Bob"); // Salida: Hola, Bob!

// Caso de uso: API que puede devolver un usuario o nada
function encontrarUsuario(id: number): { nombre: string } | null {
  if (id === 1) {
    return { nombre: "Alice" };
  }
  return null; // Indica explícitamente que no se encontró
}

objetoUsuario = encontrarUsuario(1);
console.log(objetoUsuario); // Salida: { nombre: 'Alice' }
objetoUsuario = encontrarUsuario(2);
console.log(objetoUsuario); // Salida: null
```

[↑ Volver al Índice](#toc-container)


### Buenas Prácticas con `null` y `undefined`

*   **Habilitar `strictNullChecks`**: Esta opción del compilador de TypeScript (`tsconfig.json`) fuerza a manejar explícitamente los casos donde una variable puede ser `null` o `undefined`, previniendo errores comunes en tiempo de ejecución. Es una de las características más valiosas de TypeScript.

{% hint style="info" %}
Activar `strictNullChecks` es **altamente recomendable** en cualquier proyecto TypeScript. Aunque requiere un manejo más explícito de nulos y indefinidos, reduce drásticamente los errores en tiempo de ejecución relacionados con valores nulos (`null pointer exceptions`).
{% endhint %}

*   **Preferir `undefined` para Valores Opcionales**: En parámetros de función o propiedades de objeto opcionales, `undefined` es generalmente más idiomático en TypeScript/JavaScript.
*   **Ser Explícito con `null`**: Usa `null` cuando quieras indicar de forma clara y deliberada la ausencia de un objeto.

[↑ Volver al Índice](#toc-container)


### Malas Prácticas

*   **Usar `null` Indiscriminadamente**: Evita usar `null` donde `undefined` sería más apropiado (ej. para indicar simplemente que una variable no ha sido inicializada).
*   **Ignorar `strictNullChecks`**: Deshabilitar esta opción o ignorar sus advertencias puede llevar a errores de `Cannot read property 'x' of null/undefined` en producción.

{% hint style="danger" %}
Ignorar o desactivar `strictNullChecks` introduce un riesgo significativo de errores en tiempo de ejecución que son difíciles de detectar durante el desarrollo. Es una mala práctica que compromete la seguridad de tipos que TypeScript proporciona.
{% endhint %}

[↑ Volver al Índice](#toc-container)


---

## Manejando Números Grandes: `bigint`

JavaScript (y por ende TypeScript) tiene limitaciones en la precisión de los números enteros grandes debido al uso del formato `number` (IEEE 754 de 64 bits). El tipo `number` puede representar de forma segura enteros hasta \(2^{53} - 1\) (accesible mediante `Number.MAX_SAFE_INTEGER`). Para operar con enteros más grandes, se introdujo el tipo `bigint`.

### `bigint`

Los `bigint` se crean añadiendo una `n` al final de un literal entero o llamando a la función `BigInt()`.

#### Ejemplo: Uso de `bigint`

```typescript
const numeroMuyGrande: bigint = 9007199254740991n; // Límite seguro para number
const otroNumeroGrande: bigint = 9007199254740992n; // Más allá del límite seguro
const sumaGrande: bigint = numeroMuyGrande + otroNumeroGrande;

console.log(numeroMuyGrande);     // Salida: 9007199254740991n
console.log(otroNumeroGrande);    // Salida: 9007199254740992n
console.log(sumaGrande);          // Salida: 18014398509481983n

// Comparación con number (puede perder precisión)
const numNormal: number = 9007199254740992;
console.log(numNormal); // Salida: 9007199254740992 (puede parecer correcto, pero las operaciones pueden fallar)

// const errorMixto = numeroMuyGrande + 10; // Error: No se pueden mezclar bigint y number directamente
const conversionCorrecta: bigint = numeroMuyGrande + BigInt(10); // Correcto: Convertir number a bigint
console.log(conversionCorrecta);  // Salida: 9007199254740991n + 10n = 9007199254741001n

// También funciona con números negativos
const negativoGrande: bigint = -12345678901234567890n;
console.log(negativoGrande);      // Salida: -12345678901234567890n
```

[↑ Volver al Índice](#toc-container)


### Consideraciones Importantes sobre `bigint`

*   **No Mezclar Tipos**: No puedes realizar operaciones aritméticas mezclando `bigint` y `number` directamente. Debes convertir explícitamente uno de los tipos.
*   **Sin Decimales**: `bigint` solo puede representar enteros. Operaciones como la división truncarán cualquier parte decimal (`5n / 2n` resulta en `2n`).
*   **Compatibilidad**: `bigint` es una característica relativamente nueva (ES2020). Asegúrate de que tu entorno de ejecución (navegador, Node.js) o tu objetivo de compilación (`target` en `tsconfig.json`) sea compatible si necesitas usarlo. Para entornos más antiguos, podrías necesitar polyfills o librerías externas.

{% hint style="warning" %}
Antes de usar `bigint`, verifica la compatibilidad con los navegadores o versiones de Node.js objetivo. Si necesitas soportar entornos más antiguos, considera usar librerías como `jsbi` o ajusta tu `target` de compilación si es posible, aunque esto puede implicar polyfills.
{% endhint %}

*   **Uso Específico**: Utiliza `bigint` solo cuando necesites trabajar con enteros que excedan `Number.MAX_SAFE_INTEGER`. Para la mayoría de los casos numéricos, `number` es suficiente y más performante.

[↑ Volver al Índice](#toc-container)


### Buenas Prácticas con `bigint`

*   **Conversión Explícita**: Siempre convierte `number` a `bigint` (usando `BigInt()`) antes de operar con otros `bigint`.
*   **Verificar Compatibilidad**: Asegúrate de la compatibilidad del entorno antes de usar `bigint` extensivamente.
*   **Usar Cuando Sea Necesario**: Resérvalo para casos donde la precisión con enteros muy grandes es crítica (criptografía, identificadores únicos de 64 bits, etc.).

[↑ Volver al Índice](#toc-container)


### Malas Prácticas

*   **Mezclar Tipos Implícitamente**: Intentar sumar, restar, etc., `bigint` y `number` sin conversión explícita generará errores.
*   **Usarlo para Decimales**: `bigint` no es adecuado para números con partes fraccionarias.

{% hint style="warning" %}
Si intentas usar `bigint` para representar números con decimales, perderás la parte fraccionaria. Para números decimales de alta precisión, considera librerías específicas como `decimal.js`.
{% endhint %}

*   **Ignorar Límites de `number`**: No usar `bigint` cuando se opera cerca o más allá de `Number.MAX_SAFE_INTEGER` puede llevar a errores de precisión sutiles y difíciles de depurar con el tipo `number`.

[↑ Volver al Índice](#toc-container)

