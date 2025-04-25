<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/primitive-types)
<!-- MULTILANGUAJE MENU END -->

<!--
# Tipos primitivos en TypeScript

- `string`, `number`, `boolean`, `null`, `undefined`
- Diferencias entre `null` y `undefined`
- Uso de `bigint` para operaciones con grandes números
-->
  
# Tipos primitivos en TypeScript

## `string`, `number`, `boolean`, `null`, `undefined`

Los tipos primitivos en TypeScript son la base de la tipificación y el control de errores en el lenguaje. Comprender su naturaleza, diferencias y aplicaciones es esencial para escribir código seguro, legible y mantenible.

### ¿Qué es un tipo primitivo?

Un tipo primitivo es un valor simple e inmutable que no deriva de ningún objeto. En TypeScript, los principales tipos primitivos son:

- `string`: texto y cadenas de caracteres.
- `number`: números enteros y decimales.
- `boolean`: valores lógicos (`true` o `false`).
- `null`: ausencia intencionada de valor.
- `undefined`: variable declarada pero no inicializada o sin valor asignado.

A continuación se detalla cada uno de estos tipos, sus características y consideraciones clave.

---

### Desglose y explicación de los tipos primitivos

#### `string`

Representa secuencias de caracteres Unicode. Es el tipo fundamental para almacenar y manipular texto, nombres, rutas, mensajes, descripciones y cualquier dato textual. Permite operaciones como concatenación, búsqueda, reemplazo y formateo de cadenas.

```typescript
let productName: string = "Laptop Pro 15"; // Nombre de producto
let welcomeMessage: string = `Bienvenido, ${userName}!`;
```

- Los literales de texto pueden declararse con comillas simples, dobles o backticks (template literals).
- Los métodos de cadena (`toUpperCase`, `includes`, `replace`, etc.) permiten manipular el contenido de forma segura.
- Es recomendable validar y sanear las cadenas recibidas de fuentes externas para evitar inyecciones o errores de formato.

#### `number`

Incluye tanto enteros como decimales (punto flotante). Es el tipo base para cálculos matemáticos, operaciones financieras, estadísticas y cualquier dato cuantitativo.

```typescript
let price: number = 199.99; // Precio de un producto
let quantity: number = 3; // Cantidad de unidades
let total: number = price * quantity;
```

- No existe distinción entre enteros y decimales: ambos son `number`.
- Los límites de precisión están definidos por IEEE 754 (pueden producirse errores de redondeo en operaciones con decimales).
- Para valores extremadamente grandes, considerar el uso de `bigint` (ver sección correspondiente).

#### `boolean`

Representa valores lógicos: `true` o `false`. Es esencial para el control de flujo, validaciones, flags y lógica condicional.

```typescript
let isAvailable: boolean = true; // Disponibilidad de un producto
let hasDiscount: boolean = false; // Indica si hay descuento
```

- Utilizar `boolean` para expresar estados binarios o condiciones.
- Evitar el uso de valores numéricos o cadenas para representar lógica booleana, ya que puede inducir a errores y dificultar la lectura.

#### `null`

Indica la ausencia intencionada de valor. Se utiliza para señalar que una variable o propiedad no tiene valor asignado de forma deliberada.

```typescript
let lastPurchaseDate: string | null = null; // No hay compras previas
```

- Es útil para inicializar variables que se completarán posteriormente o para limpiar referencias.
- Su uso debe ser explícito y documentado, evitando ambigüedades con `undefined`.

#### `undefined`

Señala que una variable ha sido declarada pero no inicializada, o que una función no retorna valor explícitamente.

```typescript
let pendingOrder: string | undefined; // Pedido pendiente aún no asignado

function findUser(id: number): string | undefined {
  // ... lógica de búsqueda ...
  // Si no se encuentra, retorna undefined
}
```

- Es el valor por defecto de variables no inicializadas.
- Suele indicar estados transitorios o la ausencia de retorno en funciones.
- Es recomendable evitar el uso de `undefined` como valor asignado manualmente, salvo en casos muy justificados.

{% hint style="info" %}
Diferenciar claramente entre `null` y `undefined` permite modelar estados y flujos de datos de forma precisa y robusta, facilitando la depuración y el mantenimiento del código.
{% endhint %}

---

### Ejemplo avanzado de declaración y uso conjunto

```typescript
// Declaración explícita de tipos primitivos
let firstName: string = "Ada"; // Nombre de usuario
let balance: number = 1250.75; // Saldo en cuenta bancaria
let isVerified: boolean = false; // Estado de verificación
let lastLogin: null = null; // Último acceso no registrado
let sessionToken: undefined = undefined; // Token aún no generado

// Uso en funciones
function greet(user: string): string {
  return `Welcome, ${user}!`;
}

function calculateDiscount(price: number, isMember: boolean): number {
  return isMember ? price * 0.9 : price;
}
```

---

### Casos de uso reales y recomendables

- **Gestión de estados simples**: Utilizar `boolean` para flags de activación/desactivación, validaciones o permisos.
- **Procesamiento de datos externos**: Al consumir APIs, declarar explícitamente los tipos esperados para evitar errores de conversión.
- **Modelado de entidades**: Definir propiedades básicas de modelos de datos (`string` para nombres, `number` para cantidades, `boolean` para estados, etc.).
- **Inicialización controlada**: Usar `null` o `undefined` para indicar la ausencia de datos en formularios, respuestas de red o procesos asíncronos.

---

### Consideraciones importantes y advertencias

- Los tipos primitivos son inmutables: cualquier operación que los modifique genera un nuevo valor.
- La asignación de un primitivo a otra variable crea una copia, no una referencia.
- Aunque se pueden invocar métodos sobre primitivos (`'abc'.toUpperCase()`), internamente se crea un objeto temporal, lo que puede llevar a confusiones si se intenta extenderlos o modificar su prototipo.
- El uso de `null` y `undefined` debe ser intencionado y nunca resultado de descuido; su presencia suele indicar estados excepcionales o transitorios.

{% hint style="warning" %}
No declarar explícitamente el tipo de una variable puede llevar a inferencias erróneas, especialmente cuando se reciben datos de fuentes externas o dinámicas.
{% endhint %}

---

### Buenas prácticas

- Declarar siempre el tipo explícito cuando la inferencia no es obvia o puede inducir a error.
- Utilizar `string`, `number` y `boolean` para modelar datos simples y evitar el uso de tipos genéricos como `any`.
- Documentar el significado de los valores nulos o indefinidos en el contexto de la aplicación.
- Validar siempre los datos externos antes de asignarlos a variables tipadas como primitivas.

---

### Malas prácticas frecuentes

- Usar `any` o `Object` en lugar de tipos primitivos, perdiendo así la seguridad de tipos.
- Mezclar valores nulos e indefinidos sin una justificación clara, lo que puede dificultar la depuración.
- Asumir que los valores primitivos nunca serán `null` o `undefined` en flujos asíncronos o integraciones externas.

---

### Errores comunes y trampas habituales

- Olvidar inicializar variables, lo que puede llevar a errores en tiempo de ejecución al operar sobre `undefined`.
- Realizar comparaciones no estrictas (`==`) entre tipos primitivos, lo que puede producir resultados inesperados debido a la coerción de tipos.
- Suponer que los métodos de los envoltorios (`String`, `Number`, `Boolean`) están disponibles en todos los entornos o que su comportamiento es idéntico al de los primitivos.

{% hint style="danger" %}
La manipulación incorrecta de tipos primitivos puede provocar errores silenciosos y difíciles de rastrear, especialmente en aplicaciones grandes o con múltiples fuentes de datos.
{% endhint %}

---

## Diferencias entre `null` y `undefined`

Aunque ambos representan la ausencia de valor, `null` y `undefined` tienen diferencias conceptuales y prácticas importantes en TypeScript y JavaScript.

- `undefined` indica que una variable ha sido declarada pero no inicializada, o que una función no retorna valor explícitamente.
- `null` es un valor asignado intencionadamente para señalar la ausencia deliberada de un valor.

### Comparativa entre `null` y `undefined`

A continuación se presenta una tabla que resume las diferencias clave entre ambos:

### Diferencias clave entre `null` y `undefined`

| Característica        | `null`                          | `undefined`                             |
| --------------------- | ------------------------------- | --------------------------------------- |
| Asignación explícita  | Sí                              | No (por defecto)                        |
| Tipo                  | object                          | undefined                               |
| Uso principal         | Ausencia intencionada de valor  | Variable no inicializada, retorno vacío |
| Comparación estricta  | `null === undefined` es `false` | `undefined === null` es `false`         |
| Conversión a booleano | `false`                         | `false`                                 |

Es fundamental comprender estas diferencias para evitar errores lógicos y comportamientos inesperados en el código.

#### Ejemplo de uso y diferencias

```typescript
// Diferencia entre null y undefined
let valueA: string | null = null; // Ausencia intencionada de valor
let valueB: string | undefined;   // No inicializada, es undefined por defecto

function getUserName(): string | undefined {
  // No retorna valor explícitamente, devuelve undefined
}
```

### Casos de uso y advertencias

- Utilizar `null` cuando se desee indicar explícitamente que un valor está ausente o no disponible.
- Permitir `undefined` solo cuando la variable puede no haber sido inicializada o cuando una función puede no retornar valor.
- Evitar comparar ambos valores con el operador `==`, ya que puede llevar a confusiones. Preferir siempre el uso de `===` para comparaciones estrictas.

{% hint style="warning" %}
Confundir `null` y `undefined` puede provocar errores difíciles de detectar, especialmente en validaciones y lógica condicional.
{% endhint %}

---

## Uso de `bigint` para operaciones con grandes números

El tipo `bigint` permite trabajar con números enteros de precisión arbitraria, superando las limitaciones del tipo `number` en JavaScript y TypeScript. Es especialmente útil en contextos donde se requiere manejar valores numéricos extremadamente grandes, como en criptografía, cálculos financieros o manipulación de identificadores únicos.

### Consideraciones y limitaciones de `bigint`

- Los valores `bigint` se crean añadiendo la letra `n` al final del número o mediante la función `BigInt()`.
- No es posible mezclar operaciones entre `number` y `bigint` directamente; se debe convertir explícitamente entre ambos tipos si es necesario.
- Algunas APIs y librerías pueden no soportar `bigint`, por lo que es importante validar la compatibilidad antes de su uso.

#### Ejemplo de uso de `bigint`

```typescript
// Operaciones con grandes números usando bigint
type LargeId = bigint;

const maxSafe: number = Number.MAX_SAFE_INTEGER; // Valor máximo seguro para number
const beyondSafe: LargeId = 9007199254740993n; // Valor superior al máximo seguro

const sum: LargeId = beyondSafe + 1000n; // Suma de bigints
// console.log(sum); // 9007199254741993n
```

### Casos de uso recomendados

- Generación y manipulación de identificadores únicos de gran tamaño.
- Cálculos financieros que requieren precisión absoluta y no toleran errores de redondeo.
- Algoritmos criptográficos y operaciones matemáticas avanzadas.

### Buenas prácticas y advertencias

- Validar siempre la compatibilidad de `bigint` con el entorno de ejecución y las dependencias utilizadas.
- No mezclar operaciones entre `number` y `bigint` sin conversión explícita.
- Documentar claramente el uso de `bigint` en el código para evitar confusiones y errores de mantenimiento.

{% hint style="danger" %}
El uso incorrecto de `bigint` puede provocar errores de tipo y fallos en tiempo de ejecución si no se gestiona adecuadamente la conversión entre tipos.
{% endhint %} 