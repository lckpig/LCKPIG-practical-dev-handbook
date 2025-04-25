<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/arrays-tuples)
<!-- MULTILANGUAJE MENU END -->

<!--
# Arrays y Tuplas en TypeScript
- Declaración de arrays (`number[]`, `Array<string>`)
- Uso de tuplas (`[string, number]`)
- Tuplas con etiquetas (`[id: number, nombre: string]`)
-->

# Arrays y Tuplas en TypeScript

TypeScript, como extensión de JavaScript, ofrece tipos de datos más robustos y específicos. Entre ellos, los arrays y las tuplas son fundamentales para manejar colecciones de datos de manera estructurada y segura. Entender sus diferencias y aplicaciones es clave para escribir código TypeScript eficiente y mantenible.

---

## Declaración de Arrays (`number[]`, `Array<string>`)

Los arrays en TypeScript, al igual que en JavaScript, son colecciones ordenadas de elementos. La principal ventaja de TypeScript es que nos permite definir el tipo de elementos que contendrá el array, proporcionando seguridad de tipos en tiempo de compilación.

### Definición y Sintaxis

Existen dos sintaxis principales para declarar arrays en TypeScript:

1.  **Usando corchetes `[]`:** Se especifica el tipo de los elementos seguido de `[]`.
2.  **Usando el tipo genérico `Array<T>`:** Se utiliza el tipo genérico `Array` con el tipo de los elementos entre `<>`.

Ambas sintaxis son funcionalmente equivalentes, aunque la sintaxis `[]` suele ser más común y preferida por su brevedad.

#### Ejemplos

```typescript
// Usando corchetes []
let numbersList: number[]; // Declara un array de números
let namesList: string[];   // Declara un array de strings
let activeFlags: boolean[]; // Declara un array de booleanos

// Inicialización
numbersList = [1, 2, 3, 4, 5];
namesList = ["Alice", "Bob", "Charlie"];
activeFlags = [true, false, true];

// Usando el tipo genérico Array<T>
let temperatureReadings: Array<number>; // Declara un array de números
let productCodes: Array<string>;     // Declara un array de strings

// Inicialización
temperatureReadings = [22.5, 23.1, 21.8];
productCodes = ["P001", "P002", "P003"];

// También se pueden declarar e inicializar en una sola línea
let mixedData: (string | number)[] = ["hello", 100, "world", 200]; // Array con tipos unión
let anyValues: any[] = [1, "test", true, { id: 1 }]; // Array de tipo 'any' (evitar si es posible)
```

### Casos de Uso Reales y Recomendables

-   **Listas de elementos homogéneos:** Almacenar listas de usuarios, productos, puntuaciones, etc., donde todos los elementos son del mismo tipo.
-   **Datos tabulares:** Representar filas o columnas de una tabla donde cada elemento corresponde al mismo tipo de dato.
-   **Colecciones dinámicas:** Cuando necesitas añadir o eliminar elementos de una lista cuyo tamaño puede variar durante la ejecución.
-   **Resultados de APIs:** Manejar respuestas de APIs que devuelven listas de objetos con una estructura consistente.

### Consideraciones Importantes

-   **Seguridad de tipos:** Definir el tipo de los elementos previene errores comunes, como intentar añadir un `string` a un array de `number`. El compilador de TypeScript detectará estos errores.
-   **Flexibilidad vs. Seguridad:** Usar `any[]` permite almacenar cualquier tipo de dato, pero anula las ventajas de la seguridad de tipos de TypeScript. Es preferible usar tipos unión (`(string | number)[]`) si necesitas flexibilidad controlada.
-   **Rendimiento:** Los arrays en JavaScript (y por extensión en TypeScript) son objetos dinámicos. Aunque optimizados, operaciones como inserciones o eliminaciones en medio del array pueden tener implicaciones de rendimiento en arrays muy grandes.
-   **Inmutabilidad:** TypeScript no impone inmutabilidad por defecto. Si necesitas arrays inmutables, puedes usar el tipo `readonly T[]` o `ReadonlyArray<T>`.

```typescript
let immutableNumbers: readonly number[] = [1, 2, 3];
// immutableNumbers.push(4); // Error: Property 'push' does not exist on type 'readonly number[]'.
// immutableNumbers[0] = 0;   // Error: Index signature in type 'readonly number[]' only permits reading.
```

### Buenas Prácticas

-   **Especificar siempre el tipo:** Define explícitamente el tipo de los elementos del array para aprovechar la seguridad de tipos.
-   **Preferir `T[]` sobre `Array<T>`:** La sintaxis `T[]` es más concisa y ampliamente utilizada.
-   **Usar `readonly` para arrays inmutables:** Cuando el contenido del array no debe cambiar después de su creación, utiliza `readonly` para prevenir modificaciones accidentales.
-   **Evitar `any[]`:** Usa tipos específicos o tipos unión siempre que sea posible para mantener la seguridad de tipos.
-   **Inicializar los arrays:** Declara e inicializa los arrays siempre que sea posible para evitar valores `undefined`.

### Malas Prácticas

-   **Usar `any[]` sin necesidad:** Sacrifica la seguridad de tipos innecesariamente. Si realmente necesitas tipos mixtos, considera tipos unión o interfaces/tipos más específicos.
-   **Modificar arrays pasados como argumentos si se esperan inmutables:** Si una función recibe un array que no debería modificar, usa `readonly` en la firma de la función o clona el array antes de modificarlo.
-   **Confiar ciegamente en `Array.prototype.sort()` sin función de comparación:** El ordenamiento por defecto convierte los elementos a strings, lo que puede dar resultados inesperados con números (ej. `[1, 10, 2]` se ordenaría como `[1, 10, 2]`). Proporciona siempre una función de comparación para números.
-   **No considerar el tipo de retorno de métodos:** Métodos como `map`, `filter`, `slice` devuelven *nuevos* arrays, no modifican el original (a diferencia de `push`, `splice`, `sort`).

### Errores Comunes y Trampas

-   **Acceder a índices fuera de los límites:** TypeScript no previene el acceso a índices inexistentes en tiempo de compilación. Acceder a `miArray[miArray.length]` o un índice negativo devolverá `undefined` en tiempo de ejecución, lo cual puede causar errores si no se maneja adecuadamente.
-   **Mutación inesperada:** Olvidar que métodos como `push`, `pop`, `splice`, `sort`, `reverse` modifican el array original puede llevar a efectos secundarios no deseados.
-   **Comparación de arrays:** Comparar arrays directamente con `==` o `===` compara sus referencias en memoria, no su contenido. Dos arrays con los mismos elementos pero creados por separado no serán iguales (`[1, 2] !== [1, 2]`).

---

## Uso de Tuplas (`[string, number]`)

Las tuplas son una característica de TypeScript que permite definir arrays con un número fijo de elementos, donde el tipo de cada elemento está predefinido en una posición específica. A diferencia de los arrays, las tuplas tienen una longitud fija y tipos específicos para cada índice.

### Definición y Sintaxis

Las tuplas se definen utilizando corchetes `[]` y especificando los tipos de cada elemento en orden.

#### Ejemplos

```typescript
// Una tupla que representa un par clave-valor: string y number
let keyValuePair: [string, number];
keyValuePair = ["userId", 12345]; // Correcto

// keyValuePair = [12345, "userId"]; // Error: Type 'number' is not assignable to type 'string'. Type 'string' is not assignable to type 'number'.
// keyValuePair = ["userId", 12345, "extra"]; // Error: Type '[string, number, string]' is not assignable to type '[string, number]'. Source has 3 element(s) but target allows only 2.

// Una tupla para representar coordenadas (x, y, z)
let point3D: [number, number, number];
point3D = [10, 20, 30];

// Acceso a elementos por índice (con seguridad de tipos)
let userKey: string = keyValuePair[0]; // userKey es de tipo string
let userIdValue: number = keyValuePair[1]; // userIdValue es de tipo number

// console.log(keyValuePair[2]); // Error: Tuple type '[string, number]' of length '2' has no element at index '2'.

// Tuplas con tipos opcionales (usando '?')
let optionalTuple: [string, number?]; // El segundo elemento es opcional
optionalTuple = ["status"];          // Correcto
optionalTuple = ["status", 200];     // Correcto
// optionalTuple = ["status", 200, "extra"]; // Error: Tuple type '[string, number?]' of length '1 | 2' has no element at index '2'.

// Tuplas con elementos rest (usando '...')
let namesAndScores: [string, ...number[]]; // Un string seguido de cero o más números
namesAndScores = ["Scores"];               // Correcto
namesAndScores = ["Scores", 100, 95, 88];  // Correcto
// namesAndScores = ["Scores", 100, "90"]; // Error: Type 'string' is not assignable to type 'number'.
```

### Casos de Uso Reales y Recomendables

-   **Pares clave-valor:** Representar entradas de diccionarios o mapas donde el tipo de la clave y el valor son fijos.
-   **Coordenadas o puntos:** Almacenar coordenadas (2D, 3D) donde cada posición representa un eje específico.
-   **Valores de retorno múltiples:** Funciones que necesitan devolver un conjunto fijo de valores con tipos específicos (aunque a menudo es preferible devolver un objeto con propiedades nombradas para mayor claridad).
-   **Datos estructurados fijos:** Representar registros con un número fijo de campos y tipos definidos, como una fila de un archivo CSV con estructura conocida.
-   **Estado en React Hooks:** El hook `useState` de React devuelve una tupla `[State, (newState: State) => void]`.

### Consideraciones Importantes

-   **Longitud fija (en teoría):** Aunque la definición de la tupla implica una longitud fija, métodos de array como `push`, `pop`, `splice` *pueden* técnicamente modificar la longitud de una tupla en tiempo de ejecución en versiones anteriores de TypeScript o bajo ciertas configuraciones. Sin embargo, esto va en contra del propósito de las tuplas y puede llevar a errores. TypeScript moderno tiende a ser más estricto.
-   **Acceso por índice:** El acceso a los elementos se realiza mediante índices numéricos (`tupla[0]`, `tupla[1]`), lo que puede hacer el código menos legible si no está claro qué representa cada índice.
-   **Desestructuración:** La desestructuración es una forma muy común y legible de trabajar con tuplas.

```typescript
let employee: [number, string, boolean] = [1, "John Doe", true];

// Desestructuración
let [id, employeeName, isActive] = employee;
console.log(id); // 1 (number)
console.log(employeeName); // "John Doe" (string)
console.log(isActive); // true (boolean)
```

### Buenas Prácticas

-   **Usar tuplas para colecciones heterogéneas de longitud fija:** Son ideales cuando tienes un número conocido de elementos con tipos específicos y ordenados.
-   **Preferir la desestructuración para el acceso:** Mejora significativamente la legibilidad del código al asignar nombres significativos a cada elemento de la tupla.
-   **Considerar objetos para estructuras complejas:** Si tienes muchos elementos o la relación entre ellos es compleja, un objeto con propiedades nombradas suele ser más claro y mantenible que una tupla larga.
-   **Usar `readonly` para tuplas inmutables:** Al igual que con los arrays, puedes definir tuplas inmutables.

```typescript
let readOnlyConfig: readonly [string, number] = ["timeout", 5000];
// readOnlyConfig[0] = "delay"; // Error: Cannot assign to '0' because it is a read-only property.
// readOnlyConfig.push("extra"); // Error: Property 'push' does not exist on type 'readonly [string, number]'.
```

### Malas Prácticas

-   **Usar tuplas para listas homogéneas:** Si todos los elementos son del mismo tipo y la longitud puede variar, un array (`T[]`) es la elección correcta.
-   **Crear tuplas muy largas:** Tuplas con muchos elementos se vuelven difíciles de manejar y entender. En esos casos, un objeto o una clase es preferible.
-   **Abusar de `push`/`pop` en tuplas:** Aunque técnicamente posible en algunos escenarios, modifica la longitud esperada y viola el propósito de la tupla. Evítalo.
-   **Acceder a elementos solo por índice numérico sin contexto:** Dificulta la lectura y el mantenimiento. Usa desestructuración o tuplas con etiquetas.

### Errores Comunes y Trampas

-   **Confundir tuplas con arrays:** Aunque comparten sintaxis similar y métodos de array, su propósito es diferente (estructura fija vs. lista dinámica).
-   **Errores de índice:** Intentar acceder a un índice que no existe en la definición de la tupla será detectado por TypeScript, pero errores lógicos al usar el índice incorrecto (ej. `tupla[0]` esperando el valor de `tupla[1]`) no lo serán.
-   **Limitaciones de `push` (histórico):** En versiones antiguas, `push` permitía añadir elementos de *cualquier* tipo definido en la tupla, no necesariamente el tipo del último elemento, lo cual podía romper la estructura tipada. Las versiones modernas son más estrictas.

---

## Tuplas con Etiquetas (`[id: number, nombre: string]`)

TypeScript introduce la capacidad de añadir etiquetas a los elementos de las tuplas. Estas etiquetas son puramente para mejorar la documentación y la legibilidad del código; no cambian el comportamiento en tiempo de ejecución ni la forma en que se accede a los elementos (sigue siendo por índice numérico).

### Definición y Sintaxis

Se añaden etiquetas anteponiendo un nombre seguido de dos puntos (`:`) antes del tipo de cada elemento.

#### Ejemplos

```typescript
// Tupla sin etiquetas
let productInfo: [string, number, number];
productInfo = ["Laptop", 1200, 10]; // Código, Precio, Stock

// Tupla con etiquetas
let labeledProductInfo: [code: string, price: number, stock: number];
labeledProductInfo = ["Tablet", 400, 25];

// Acceso sigue siendo por índice
let productCode: string = labeledProductInfo[0]; // "Tablet"
let productPrice: number = labeledProductInfo[1]; // 400
let productStock: number = labeledProductInfo[2]; // 25

// Las etiquetas mejoran la información en el editor (autocompletado, tooltips)
function displayProduct(product: [code: string, price: number, stock: number]) {
    // Al pasar el cursor sobre product[1], el editor puede mostrar "price: number"
    console.log(`Product Code: ${product[0]}`);
    console.log(`Price: $${product[1]}`);
    console.log(`Stock: ${product[2]} units`);
}

displayProduct(labeledProductInfo);

// También se pueden usar con desestructuración (aunque las etiquetas no se usan directamente aquí)
let [code, price, stock]: [code: string, price: number, stock: number] = labeledProductInfo;
console.log(code, price, stock); // "Tablet", 400, 25

// Combinación con tipos opcionales y rest
type LabeledPoint = [x: number, y: number, z?: number];
type LabeledData = [id: string, ...values: number[]];
```

### Casos de Uso Reales y Recomendables

-   **Mejorar la legibilidad:** Hacen que el propósito de cada elemento de la tupla sea inmediatamente obvio sin necesidad de comentarios adicionales o de buscar la definición.
-   **API de funciones:** Al definir los tipos de los parámetros o el valor de retorno de una función que usa tuplas, las etiquetas sirven como documentación integrada.
-   **Tipos complejos:** En tipos que combinan varias tuplas o estructuras anidadas, las etiquetas ayudan a desenredar el significado de cada parte.

### Consideraciones Importantes

-   **Solo para documentación:** Las etiquetas no existen en el JavaScript compilado. Son una ayuda en tiempo de desarrollo. No puedes acceder a los elementos usando las etiquetas (ej. `labeledProductInfo.price` no funciona).
-   **Mantenimiento:** Si cambias el significado de una posición en la tupla, recuerda actualizar también la etiqueta correspondiente.

### Buenas Prácticas

-   **Usar etiquetas siempre que mejoren la claridad:** Especialmente en tuplas con más de dos elementos o cuyo significado no sea obvio por el contexto.
-   **Elegir nombres de etiquetas descriptivos:** Al igual que con las variables, usa nombres claros y concisos.
-   **Combinar con desestructuración:** Aunque las etiquetas no se usan en la desestructuración directamente, usarlas en la definición del tipo de la tupla mejora la comprensión general.

### Malas Prácticas

-   **Usar etiquetas engañosas o poco claras:** Nombres como `item1`, `val2` no aportan mucho valor.
-   **Confiar en que las etiquetas proporcionan acceso por nombre:** Recordar que el acceso sigue siendo numérico (`tupla[0]`). Si necesitas acceso por nombre, usa un objeto.

### Errores Comunes y Trampas

-   **Intentar acceder por etiqueta:** Es un error común pensar que `tupla.etiqueta` funcionará. El acceso es siempre `tupla[indice]`.
-   **Olvidar que son solo para desarrollo:** No esperar que las etiquetas tengan ningún efecto en el código JavaScript resultante.

Al dominar el uso de arrays y tuplas (incluidas las etiquetadas), puedes estructurar tus datos de forma más eficaz y segura en TypeScript, lo que lleva a un código más robusto, legible y fácil de mantener.
