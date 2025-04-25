<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/arrays-tuples)
<!-- MULTILANGUAJE MENU END -->

<!--
- Declaración de arrays (`number[]`, `Array<string>`)
- Uso de tuplas (`[string, number]`)
- Tuplas con etiquetas (`[id: number, nombre: string]`)
-->

# Arrays y Tuplas en TypeScript

## Declaración de arrays (`number[]`, `Array<string>`)

En TypeScript, los arrays permiten almacenar colecciones de elementos del mismo tipo, lo que facilita la manipulación y el acceso a datos homogéneos. Existen dos formas principales de declarar un array: utilizando la sintaxis `tipo[]` o la forma genérica `Array<tipo>`. Ambas son equivalentes en funcionalidad, pero pueden preferirse en distintos contextos según las convenciones del equipo o la claridad del código.

**Ejemplo básico:**
```typescript
// Declaración de un array de números
const numbers: number[] = [1, 2, 3, 4, 5];

// Declaración usando la sintaxis genérica
const names: Array<string> = ["Alice", "Bob", "Charlie"];
```

**Caso de uso real:**
Supongamos que necesitas almacenar una lista de identificadores de usuario obtenidos de una base de datos. Utilizar un array tipado garantiza que solo se almacenen valores válidos y previene errores en tiempo de compilación.

```typescript
// Lista de IDs de usuario
const userIds: number[] = fetchUserIds(); // fetchUserIds retorna number[]
```

**Consideraciones importantes:**
- Los arrays en TypeScript son homogéneos: todos los elementos deben ser del tipo declarado.
- Puedes utilizar métodos estándar de arrays de JavaScript, como `map`, `filter`, `reduce`, etc., con la ventaja de tener tipado estático.
- Si necesitas un array de elementos de distintos tipos, considera usar tuplas o uniones de tipos.

**Buenas prácticas:**
- Prefiere la sintaxis `tipo[]` para arrays simples, ya que es más concisa y legible.
- Utiliza `Array<tipo>` cuando trabajes con tipos genéricos o cuando el tipo sea complejo (por ejemplo, `Array<User | Admin>`).
- Declara siempre el tipo de los arrays explícitamente si el valor inicial no es suficiente para que TypeScript lo infiera correctamente.

**Malas prácticas:**
- No mezcles tipos dentro de un array tipado, ya que esto anula las ventajas del tipado estático.
- Evita el uso de `any[]`, ya que elimina la seguridad de tipos y puede introducir errores difíciles de detectar.

**Errores comunes:**
- Olvidar declarar el tipo del array y asumir que TypeScript lo inferirá correctamente en todos los casos.
- Modificar un array declarado como constante (`const`) intentando reasignarlo, en lugar de modificar sus elementos.

## Uso de tuplas (`[string, number]`)

Las tuplas en TypeScript permiten definir arrays de longitud fija donde cada elemento puede tener un tipo diferente. Son especialmente útiles cuando se necesita representar una estructura de datos ordenada y heterogénea, como pares clave-valor, coordenadas, o resultados de funciones que retornan múltiples valores.

**Ejemplo básico:**
```typescript
// Tupla que representa un par nombre-edad
const person: [string, number] = ["Alice", 30];
```

**Caso de uso real:**
Imagina una función que retorna tanto el resultado de una operación como un código de estado. Utilizar una tupla permite devolver ambos valores de forma tipada y estructurada.

```typescript
function divide(a: number, b: number): [number, string] {
  if (b === 0) {
    return [0, "Error: División por cero"];
  }
  return [a / b, "OK"];
}

const [resultado, estado] = divide(10, 2);
// resultado: 5, estado: "OK"
```

**Consideraciones importantes:**
- Las tuplas tienen una longitud fija y los tipos de cada posición están definidos.
- Puedes acceder a los elementos por índice, pero es recomendable desestructurar para mayor claridad.
- A partir de TypeScript 3.0, las tuplas pueden tener elementos opcionales y rest (`[string, ...number[]]`).

**Buenas prácticas:**
- Utiliza tuplas cuando el significado de cada posición sea claro y esté bien documentado.
- Desestructura las tuplas al recibirlas para mejorar la legibilidad del código.
- Si la estructura se vuelve compleja, considera usar un objeto con propiedades nombradas en lugar de una tupla.

**Malas prácticas:**
- No abuses de las tuplas para estructuras de datos complejas o de longitud variable.
- Evita acceder a los elementos de la tupla por índice sin desestructurar, ya que puede dificultar el mantenimiento.

**Errores comunes:**
- Intercambiar el orden de los elementos al asignar valores a la tupla.
- Intentar añadir más elementos de los definidos en la tupla.

## Tuplas con etiquetas (`[id: number, nombre: string]`)

TypeScript permite etiquetar los elementos de una tupla para mejorar la legibilidad y la autocompletación en los editores. Las etiquetas no afectan la ejecución, pero sí aportan claridad sobre el propósito de cada elemento.

**Ejemplo básico:**
```typescript
// Tupla con etiquetas para mayor claridad
const user: [id: number, name: string] = [1, "Alice"];
```

**Caso de uso real:**
Supón que necesitas retornar información estructurada sobre un recurso, como el identificador y el nombre de un producto. Las etiquetas ayudan a entender rápidamente el significado de cada posición.

```typescript
function getProductInfo(): [id: number, name: string] {
  // Simulación de obtención de datos
  return [101, "Laptop"];
}

const [productId, productName] = getProductInfo();
// productId: 101, productName: "Laptop"
```

**Consideraciones importantes:**
- Las etiquetas son solo para documentación y autocompletado; no influyen en la ejecución.
- Mejoran la experiencia de desarrollo, especialmente en equipos grandes o proyectos de larga duración.
- No sustituyen a los objetos cuando se requiere flexibilidad o extensibilidad.

**Buenas prácticas:**
- Etiqueta las tuplas cuando el significado de cada elemento no sea evidente por el contexto.
- Prefiere objetos si la estructura de datos puede crecer o cambiar en el futuro.

**Malas prácticas:**
- No utilices etiquetas como sustituto de una documentación adecuada.
- Evita tuplas etiquetadas para estructuras complejas o con más de tres elementos.

**Errores comunes:**
- Asumir que las etiquetas son obligatorias o que afectan el comportamiento en tiempo de ejecución.
- Olvidar actualizar las etiquetas si cambia la estructura de la tupla. 