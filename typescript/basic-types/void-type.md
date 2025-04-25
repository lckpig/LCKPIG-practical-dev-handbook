<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/void-type)
<!-- MULTILANGUAJE MENU END -->

<!--
# El tipo `void` y su uso en funciones
- Diferencias entre `void` y `undefined` en retornos
- Uso en funciones sin retorno explícito
-->

# El tipo `void` y su uso en funciones

En TypeScript, el tipo `void` representa la ausencia intencional y explícita de un valor de retorno en una función. Su propósito principal es indicar que una función realiza una acción o efecto secundario (como modificar el estado, interactuar con el DOM, realizar logging o llamadas a APIs) pero no está diseñada para devolver un resultado computable al código que la invoca. Comprender `void`, sus matices y sus diferencias con otros tipos como `undefined` y `never` es fundamental para escribir código TypeScript preciso, legible y robusto, especialmente en el contexto de la programación funcional y el manejo de callbacks.

---

## Diferencias clave: `void`, `undefined` y `never`

Aunque `void` y `undefined` pueden parecer intercambiables a primera vista en funciones que no devuelven valor, y `never` también se relaciona con la ausencia de retorno, cada uno tiene un significado semántico distinto y un propósito específico en TypeScript.

### `void`: Ausencia intencional de valor

- **Significado**: `void` declara la *intención* de que una función no retorne un valor útil. Comunica que el propósito de la función reside en sus efectos secundarios, no en su resultado.
- **Contexto de uso**: Es el tipo de retorno estándar y preferido para funciones que:
    - No tienen sentencia `return`.
    - Usan `return;` sin un valor asociado (salida temprana).
    - Realizan operaciones como logging, manipulación del DOM, suscripción a eventos, etc.
- **Asignabilidad**: Una función tipada con `void` puede técnicamente devolver `undefined` (ya sea implícita o explícitamente). TypeScript permite esto por razones pragmáticas (compatibilidad con JavaScript), pero se considera una práctica que puede generar confusión si no se entiende bien. El valor devuelto (implícita o explícitamente `undefined`) generalmente se ignora en el contexto de `void`.

#### Ejemplo: Función de efecto secundario

```typescript
// Función que actualiza el DOM, un efecto secundario. No devuelve nada útil.
function updateStatusBar(message: string): void {
  const statusBar = document.getElementById("status-bar");
  if (statusBar) {
    statusBar.textContent = message;
  }
  // No hay 'return', implícitamente devuelve 'undefined', pero la intención es 'void'.
}

updateStatusBar("Proceso iniciado...");
```

### `undefined`: Indicador de valor potencialmente ausente

- **Significado**: `undefined` representa la ausencia de un valor *específico* o la no inicialización de una variable. Como tipo de retorno, indica que la función *podría* devolver un valor de un tipo determinado, pero en ciertas condiciones, devuelve `undefined` para señalar que no hay un valor válido o aplicable disponible.
- **Contexto de uso**: Se utiliza cuando `undefined` es un estado significativo dentro del dominio de la función. Es común en funciones de búsqueda que pueden o no encontrar un resultado, o en funciones que devuelven datos opcionales.
- **Asignabilidad**: Se debe declarar explícitamente en la firma de tipo (`tipo | undefined`) si la función puede retornar `undefined` además de otro tipo. Con `strictNullChecks` activado, el manejo de `undefined` debe ser explícito.

#### Ejemplo: Búsqueda opcional

```typescript
interface User {
  id: number;
  name: string;
}

const users: User[] = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
];

// La función busca un usuario. Puede devolver User o undefined.
function findUserById(id: number): User | undefined {
  return users.find(user => user.id === id);
  // find devuelve el elemento o 'undefined' si no lo encuentra.
}

const foundUser = findUserById(2); // Tipo: User | undefined
const notFoundUser = findUserById(3); // Tipo: User | undefined

if (foundUser) {
  console.log(`Usuario encontrado: ${foundUser.name}`);
} else {
  console.log("Usuario no encontrado (valor es undefined).");
}
```

### `never`: Indicador de que una función nunca retorna normalmente

- **Significado**: `never` indica que una función *nunca* completa su ejecución de forma normal. Esto significa que siempre lanza una excepción o entra en un bucle infinito. No es que devuelva "nada" como `void`, sino que el flujo de ejecución *nunca llega al punto de retorno*.
- **Contexto de uso**: Se utiliza para funciones que garantizan la interrupción del flujo normal del programa.
- **Asignabilidad**: El tipo `never` es asignable a cualquier otro tipo, pero ningún tipo (excepto `never` mismo) es asignable a `never`.

#### Ejemplo: Función que siempre lanza error

```typescript
// Función que siempre lanza un error, nunca retorna normalmente.
function throwError(message: string): never {
  throw new Error(message);
  // El código después de 'throw' es inalcanzable.
}

// Función con bucle infinito
function infiniteLoop(): never {
  while (true) {
    console.log("Looping...");
    // Nunca sale del bucle, nunca retorna.
  }
}

// Uso en validación exhaustiva (type guards)
type Shape = "circle" | "square";

function getArea(shape: Shape): number {
    switch (shape) {
        case "circle": return Math.PI;
        case "square": return 1;
        default:
            // Si añadimos un nuevo tipo a Shape sin actualizar el switch,
            // 'exhaustiveCheck' tendrá tipo 'never', pero si llega aquí,
            // TypeScript dará un error porque el nuevo tipo no es asignable a 'never'.
            const exhaustiveCheck: never = shape;
            return exhaustiveCheck;
    }
}
```

### Tabla Comparativa

| Característica       | `void`                                   | `undefined`                               | `never`                                       |
| :------------------- | :--------------------------------------- | :---------------------------------------- | :-------------------------------------------- |
| **Significado**      | Ausencia intencional de valor de retorno | Valor ausente o no inicializado           | Función nunca retorna normalmente             |
| **Intención**        | Realizar efectos secundarios             | Indicar un estado de "no valor" posible   | Señalar interrupción o finalización anormal   |
| **¿Retorna algo?**   | Implícitamente `undefined` (ignorado)    | Puede retornar `undefined` explícitamente | Nunca alcanza el punto de retorno             |
| **Uso común**        | Event handlers, logging, DOM manip.      | Búsquedas, valores opcionales             | Lanzar errores, bucles infinitos, type guards |
| **strictNullChecks** | Tratamiento especial para funciones      | Requiere manejo explícito                 | No directamente afectado                      |

---

## Uso de `void` en Funciones

El uso más extendido y fundamental de `void` es en la declaración y definición de tipos para funciones que no están destinadas a devolver un valor.

### Inferencia Automática vs. Declaración Explícita

TypeScript es capaz de inferir `: void` si una función no tiene una sentencia `return` que devuelva un valor o solo usa `return;`.

#### Ejemplo: Inferencia

```typescript
// TypeScript infiere el tipo de retorno como 'void'
function logStartupMessage(appName: string) { // Infiere (appName: string) => void
  console.log(`${appName} ha iniciado.`);
  // No hay return
}
```

Aunque la inferencia funciona, declarar explícitamente `: void` es una **buena práctica** por varias razones:

1.  **Claridad y Legibilidad**: Hace explícita la intención del diseñador de la función. Cualquiera que lea la firma sabe inmediatamente que no debe esperar un valor de retorno.
2.  **Mantenibilidad**: Previene errores si en el futuro alguien modifica la función y accidentalmente añade una sentencia `return` con un valor. TypeScript señalará la discrepancia.
3.  **Contrato Explícito**: Define un contrato claro para los consumidores de la función.

#### Ejemplo: Declaración explícita

```typescript
// Declaración explícita mejora la claridad
function shutdownProcess(processId: number): void {
  console.log(`Iniciando apagado del proceso ${processId}...`);
  // Lógica de apagado (efecto secundario)
  // ...
  console.log(`Proceso ${processId} apagado.`);
  // return; // Opcional, también cumple con void
}
```

### `void` en Tipos de Función y Callbacks

`void` es crucial al definir tipos para variables que almacenarán funciones o al especificar la firma de callbacks en interfaces, tipos o parámetros de función.

#### Ejemplo: Tipo de Función `EventHandler`

```typescript
// Tipo para manejadores de eventos que no devuelven nada
type EventHandler = (eventData: any) => void;

const clickLogger: EventHandler = (event) => {
  console.log(`Clic detectado en: ${event.target.id}`);
};

const mouseMoveTracker: EventHandler = (event) => {
  // Actualizar UI con coordenadas (efecto secundario)
  const coordsElement = document.getElementById("coords");
  if (coordsElement) {
    coordsElement.textContent = `X: ${event.clientX}, Y: ${event.clientY}`;
  }
};

// Uso
// buttonElement.addEventListener('click', clickLogger);
// document.addEventListener('mousemove', mouseMoveTracker);
```

#### Ejemplo: Callback en Configuración

```typescript
interface TaskConfig {
  id: string;
  description: string;
  onComplete: (taskId: string, success: boolean) => void; // Callback informativo
}

function executeTask(config: TaskConfig): void {
  console.log(`Ejecutando tarea: ${config.description}`);
  try {
    // Simular ejecución de la tarea...
    const isSuccess = Math.random() > 0.3; // Simula éxito/fallo
    console.log(`Tarea ${config.id} completada.`);
    // Llamar al callback informando el resultado
    config.onComplete(config.id, isSuccess);
  } catch (error) {
    console.error(`Error en tarea ${config.id}:`, error);
    config.onComplete(config.id, false); // Informar fallo
  }
}

// Uso
executeTask({
  id: "TASK-001",
  description: "Procesar datos de entrada",
  onComplete: (id, success) => { // Implementación del callback void
    console.log(`Callback: Tarea ${id} finalizada. Éxito: ${success}`);
    // Podríamos actualizar UI, loggear, etc. No devolvemos nada.
  }
});
```

### Consideración Importante: Ignorando el Valor de Retorno (`void` en Callbacks)

Una característica fundamental y a veces contraintuitiva de `void` en tipos de función (especialmente callbacks) es que permite asignar una función que *sí* devuelve un valor a un tipo que espera `void`. TypeScript simplemente **ignora** el valor devuelto.

#### ¿Por qué se permite esto?

Esta flexibilidad es intencional y muy práctica. Permite reutilizar funciones existentes como callbacks incluso si devuelven algo, sin necesidad de crear wrappers innecesarios. El ejemplo clásico es `Array.prototype.forEach`.

#### Ejemplo: `forEach` y `push`

```typescript
const sourceItems = ['a', 'b', 'c'];
const processedItems: string[] = [];

// forEach espera un callback de tipo (value: string, ...) => void
// Array.prototype.push devuelve la nueva longitud del array (un number)

// A pesar de que push devuelve un number, podemos usarlo aquí.
// El valor devuelto por push (1, 2, 3) es simplemente ignorado por forEach.
sourceItems.forEach(item => processedItems.push(item.toUpperCase()));
//                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
//                Esta función devuelve un number, pero se asigna a un tipo que espera void.

console.log(processedItems); // Salida: ['A', 'B', 'C']

// Si TypeScript no permitiera esto, tendríamos que hacer algo más verboso:
sourceItems.forEach(item => {
  processedItems.push(item.toUpperCase());
  // return; // O añadir un return explícito, innecesario gracias a la regla de void
});
```


{% hint style="warning" %}
**Precaución importante**: Aunque TypeScript ignora el valor de retorno de una función asignada a un tipo `void`, **no debes depender** de este valor ignorado en el código que consume el callback. El contrato `void` significa "no esperes un valor útil". Si necesitas el valor, la firma del callback debería reflejarlo (p.ej., `() => number`).
{% endhint %}

---

## Buenas Prácticas y Errores Comunes

### Buenas Prácticas

1.  **Declarar `: void` Explícitamente**: Siempre declara `: void` en funciones diseñadas para no retornar valor. Mejora la claridad y previene errores futuros.
2.  **Usar `void` para Efectos Secundarios**: Es el tipo ideal para funciones cuyo propósito principal es interactuar con el exterior (DOM, consola, red, estado global) sin devolver un resultado.
3.  **Preferir `void` sobre `undefined` para Callbacks sin Retorno**: Si un callback no debe devolver nada, usa `() => void`. Usar `() => undefined` permitiría retornar `undefined` explícitamente, lo cual puede ser ambiguo.
4.  **Entender la Ignorancia del Retorno**: Comprende por qué TypeScript permite asignar funciones con retorno a tipos `void` (flexibilidad en callbacks) pero no dependas del valor ignorado.

### Malas Prácticas / Errores Comunes

1.  **Confundir `void` con `undefined` en Tipos de Retorno**: No uses `| undefined` si la función *nunca* debe devolver un valor significativo. Usa `void` para expresar la ausencia intencional.
    ```typescript
    // Mal: Sugiere que undefined es un valor de retorno posible y significativo
    // function logEvent(data: any): undefined { console.log(data); return undefined; }

    // Bien: Indica claramente que no hay valor de retorno intencional
    function logEvent(data: any): void { console.log(data); }
    ```
2.  **Intentar Usar el Resultado de una Función `void`**: Aunque técnicamente el valor es `undefined` en runtime, TypeScript (con `strictNullChecks`) te advertirá si intentas usarlo, ya que la intención era no devolver nada útil.
    ```typescript
    function doSomething(): void { console.log("Doing something..."); }

    const result = doSomething(); // result es 'undefined'
    // const upperResult = result.toUpperCase(); // Error: 'result' is possibly 'undefined'.
    ```
3.  **Retornar Valores Significativos desde Funciones `void`**: Aunque técnicamente posible (y el valor será ignorado si se asigna a un tipo `void`), es conceptualmente incorrecto y confuso. Si una función calcula un valor útil, su tipo de retorno debería reflejarlo.
    ```typescript
    // Mal: La función calcula algo pero lo descarta por el tipo void
    function calculateAndLog(a: number, b: number): void {
      const sum = a + b;
      console.log(`Sum: ${sum}`);
      // return sum; // Esto sería un error de tipo si se descomenta
    }

    // Bien (si necesitas el valor):
    function calculateSum(a: number, b: number): number {
      const sum = a + b;
      console.log(`Sum: ${sum}`); // Puede tener efecto secundario Y retornar valor
      return sum;
    }
    ```
4.  **Usar `void` como Tipo de Parámetro (Raro y Generalmente Incorrecto)**: Es muy infrecuente necesitar un parámetro de tipo `void`. Generalmente indica un mal diseño. `void` se usa casi exclusivamente como tipo de retorno.
    ```typescript
    // Muy raro, ¿qué significa esperar 'void' como entrada?
    // function processVoidInput(input: void) { ... }
    ```

{% hint style="info" %}
La elección correcta entre `void`, `undefined`, y `never` depende de la **semántica** y la **intención** de la función. `void` es la herramienta estándar para declarar explícitamente la ausencia intencional de un valor de retorno útil.
{% endhint %}
