<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/void-type)
<!-- MULTILANGUAJE MENU END -->

<!--
# El tipo `void` y su uso en funciones
- Diferencias entre `void` y `undefined` en retornos
- Uso en funciones sin retorno explícito
-->

# El tipo `void` y su uso en funciones

En TypeScript, el tipo `void` representa la ausencia de valor. Se utiliza principalmente para indicar que una función no devuelve ningún valor. Comprender `void` y sus diferencias con `undefined` es crucial para escribir código TypeScript claro y robusto, especialmente al trabajar con funciones.

---

## Diferencias entre `void` y `undefined` en retornos

Aunque a primera vista `void` y `undefined` puedan parecer similares cuando se trata de funciones que no retornan valor, existen diferencias sutiles pero importantes en su significado y uso en TypeScript.

### `void`: Ausencia intencional de valor

- **Significado**: `void` indica explícitamente que una función está diseñada para *no* retornar un valor. Su propósito es realizar una acción, como modificar el estado, imprimir en consola o realizar una llamada de red, sin devolver un resultado directo.
- **Contexto de uso**: Es el tipo de retorno preferido para funciones que completan su ejecución sin un `return` explícito o que tienen un `return;` sin valor.
- **Asignabilidad**: Una función declarada con retorno `void` puede, técnicamente, retornar `undefined` (o `null` si `strictNullChecks` está desactivado), pero esto generalmente se considera una mala práctica o un descuido, ya que contradice la intención explícita de no retornar nada.

#### Ejemplo

```typescript
// Función que realiza una acción pero no devuelve nada
function logMessage(message: string): void {
  console.log(message);
  // No hay sentencia 'return' explícita
}

// Llamada a la función
logMessage("Hola Mundo");

// Aunque 'result' técnicamente contendrá 'undefined' en tiempo de ejecución,
// TypeScript nos advierte si intentamos usarlo como si tuviera un valor distinto de 'undefined' o 'null'.
const result = logMessage("Test");
// console.log(result.toUpperCase()); // Error: 'result' is possibly 'undefined'. (si strictNullChecks está activado)
```

### `undefined`: Valor no definido o ausente

- **Significado**: `undefined` es tanto un *tipo* como un *valor* en TypeScript (y JavaScript). Como tipo, representa variables que no han sido inicializadas. Como valor, indica que una variable ha sido declarada pero no se le ha asignado un valor, o que una función retorna explícitamente `undefined`.
- **Contexto de uso**: Se usa para indicar que un valor podría no estar presente o no ser aplicable en un momento dado. En el contexto de retornos de función, retornar `undefined` explícitamente puede indicar que la función *podría* retornar un valor en algunos casos, pero en la situación actual, no hay un valor válido para devolver. Sin embargo, si la intención es *nunca* devolver un valor, `void` es más apropiado.
- **Asignabilidad**: Las funciones que pueden retornar `undefined` deben declararlo explícitamente en su tipo de retorno, por ejemplo `string | undefined`, si también pueden retornar otro tipo.

#### Ejemplo

```typescript
// Función que puede devolver un string o undefined
function findUser(id: number): string | undefined {
  if (id === 1) {
    return "Alice";
  }
  // Retorna undefined explícitamente si el usuario no se encuentra
  return undefined;
}

const user1 = findUser(1); // user1 es de tipo 'string | undefined' y contiene "Alice"
const user2 = findUser(2); // user2 es de tipo 'string | undefined' y contiene undefined

if (user2) {
  console.log(user2.toUpperCase());
} else {
  console.log("Usuario no encontrado");
}
```

### Consideraciones importantes

- **Claridad de intención**: Usar `void` comunica claramente que la función no tiene la intención de devolver un valor útil. Usar `undefined` como tipo de retorno sugiere que `undefined` es un valor de retorno *posible* y significativo dentro del dominio de la función.
- **`strictNullChecks`**: Con la opción `strictNullChecks` activada (recomendado), `undefined` (y `null`) no son asignables a otros tipos por defecto. Esto refuerza la diferencia: `void` se trata de manera especial para funciones sin retorno, mientras que `undefined` requiere un manejo explícito.
- **Funciones callback**: Al definir tipos para funciones callback que no deben devolver valor, usar `void` es crucial. Si se usara `undefined`, se permitiría al callback retornar `undefined` explícitamente, lo cual podría ser interpretado erróneamente por el código que llama al callback.

{% hint style="info" %}
En resumen: usa `void` para funciones que *nunca* retornan un valor por diseño. Usa `| undefined` en el tipo de retorno si la función *puede* retornar un valor de un tipo específico o `undefined` dependiendo de la lógica interna.
{% endhint %}

---

## Uso en funciones sin retorno explícito

El uso más común y fundamental del tipo `void` es en la declaración de funciones que no tienen una sentencia `return` que devuelva un valor, o que usan `return;` para salir prematuramente. TypeScript infiere automáticamente el tipo `void` para estas funciones si no se especifica un tipo de retorno.

### Inferencia automática de `void`

Si no anotas el tipo de retorno de una función y esta no devuelve ningún valor explícitamente, TypeScript inferirá `void`.

#### Ejemplo

```typescript
// TypeScript infiere el tipo de retorno como 'void'
function greet(name: string) {
  console.log(`¡Hola, ${name}!`);
}

// La función 'greet' tiene el tipo (name: string) => void
```

### Declaración explícita de `void`

Aunque la inferencia funciona, es una buena práctica declarar explícitamente `: void` como tipo de retorno para mejorar la legibilidad y la claridad de la intención del código. Esto deja claro a otros desarrolladores (y a ti mismo en el futuro) que la función está diseñada específicamente para no devolver nada.

#### Ejemplo

```typescript
// Declaración explícita de void para mayor claridad
function processData(data: any): void {
  // Lógica para procesar los datos...
  console.log("Datos procesados.");
  // No hay retorno de valor
}
```

### `void` en expresiones de función y tipos de función

`void` también se utiliza al definir tipos para variables que almacenarán funciones o al definir interfaces/tipos para callbacks.

#### Ejemplo: Tipo de función

```typescript
// Definimos un tipo para una función que acepta un número y no devuelve nada
type NumberConsumer = (n: number) => void;

const printNumber: NumberConsumer = (num) => {
  console.log(`El número es: ${num}`);
};

const alertNumber: NumberConsumer = (num) => {
  // alert(`Número: ${num}`); // Suponiendo entorno de navegador
  console.log(`Alerta simulada para: ${num}`);
};

printNumber(42);
alertNumber(100);
```

#### Ejemplo: Callback en una interfaz

```typescript
interface ButtonConfig {
  text: string;
  onClick: () => void; // Callback que no debe devolver valor
}

function createButton(config: ButtonConfig): void {
  console.log(`Creando botón con texto: "${config.text}"`);
  // Lógica para crear un botón real...
  // Simular clic para llamar al callback
  console.log("Simulando clic...");
  config.onClick();
}

createButton({
  text: "Enviar",
  onClick: () => {
    console.log("Botón 'Enviar' pulsado!");
    // Este callback no devuelve nada, coincide con '() => void'
  }
});

// Ejemplo de lo que void previene (si onClick fuera () => undefined):
/*
createButton({
  text: "Cancelar",
  onClick: () => {
    console.log("Operación cancelada.");
    return undefined; // Permitido si fuera () => undefined, pero confuso
  }
});
*/
```

### Consideraciones importantes

- **Ignorando el valor de retorno**: Una característica interesante de `void` en el contexto de tipos de función es que permite que una función que *sí* retorna un valor sea asignada a un tipo que espera `void`. El valor retornado simplemente se ignora. Esto es útil para funciones como `Array.prototype.forEach`, que espera un callback de tipo `(value: T, index: number, array: T[]) => void`, pero puedes pasarle una función que retorne algo (como `Array.prototype.push`, que retorna la nueva longitud).

  ```typescript
  const numbers = [1, 2, 3];
  const newNumbers: number[] = [];

  // forEach espera un callback de tipo (value: number, ...) => void
  // Le pasamos una función que llama a push, la cual retorna un número (la nueva longitud)
  // El valor retornado por push (4, 5, 6) es ignorado por forEach.
  numbers.forEach(num => newNumbers.push(num * 2));

  console.log(newNumbers); // Salida: [2, 4, 6]
  ```

- **Evitar confusión**: Usar `void` explícitamente ayuda a evitar la confusión sobre si una función *olvidó* retornar un valor o si intencionalmente no lo hace.

{% hint style="success" %}
Declarar `void` explícitamente en funciones sin retorno mejora la mantenibilidad y la comprensión del código, dejando clara la intención del diseño de la función.
{% endhint %}
