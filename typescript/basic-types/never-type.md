<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/never-type)
<!-- MULTILANGUAJE MENU END -->

<!-- 
# El tipo `never` para funciones que no devuelven valores

- Funciones que arrojan errores (`throw`)
- Funciones que nunca terminan (`while (true) {}`)
 -->

# El tipo `never` para funciones que no devuelven valores

El tipo `never` en TypeScript representa el tipo de valores que nunca ocurren. Se utiliza principalmente en dos escenarios clave: para indicar que una función nunca retorna (o más precisamente, que nunca completa su ejecución normalmente) y en análisis de flujo de control para representar estados inalcanzables.

A diferencia de `void`, que indica que una función no retorna un valor *útil* (retorna `undefined` implícitamente), `never` significa que la función *nunca* llega al punto de retorno.

---

## Funciones que arrojan errores (`throw`)

Una función que siempre lanza una excepción antes de poder retornar un valor tiene un tipo de retorno inferido o explícito de `never`. Esto se debe a que la ejecución normal de la función se interrumpe abruptamente por el error.

### Definición y utilidad

Cuando una función está diseñada para detener la ejecución del programa o una parte específica de él mediante un error, el tipo `never` comunica claramente esta intención. Indica a otros desarrolladores y al propio compilador de TypeScript que cualquier código que siga a la llamada de esta función en el mismo bloque de ejecución es, en teoría, inalcanzable (siempre que la excepción no sea capturada).

#### Ejemplo

```typescript
function throwError(message: string): never {
  // Lanza siempre una excepción, nunca retorna un valor
  throw new Error(message); 
}

function processInput(input: string | number) {
  if (typeof input === 'string') {
    console.log(`Processing string: ${input}`);
  } else if (typeof input === 'number') {
    console.log(`Processing number: ${input}`);
  } else {
    // En este punto, 'input' tiene tipo 'never' porque
    // teóricamente, todas las posibilidades (string, number)
    // ya han sido cubiertas. Si llegara aquí, sería un error lógico.
    // Lanzar un error aquí es una forma de manejar este estado inesperado.
    throwError("Tipo de entrada inválido e inesperado."); 
  }
}
```

### Casos de uso reales y recomendables

- **Validación estricta**: Funciones que validan datos críticos y lanzan un error si la validación falla, deteniendo el proceso.
- **Manejo de estados imposibles**: Como en el ejemplo anterior, usar `never` para señalar ramas de código que lógicamente no deberían alcanzarse.
- **Funciones de utilidad para errores**: Crear funciones reutilizables que centralicen el lanzamiento de errores específicos de la aplicación.

### Consideraciones importantes

- **Captura de excepciones**: Aunque una función retorne `never` porque lanza un error, este error puede ser capturado por un bloque `try...catch`. En ese caso, el flujo del programa puede continuar después del bloque `catch`. El tipo `never` se refiere a la ruta de ejecución *normal* de la función.
- **Claridad del código**: Usar `never` explícitamente mejora la legibilidad y la intención del código, dejando claro que la función está diseñada para no retornar.

### Buenas prácticas

- Anotar explícitamente `: never` en funciones diseñadas para siempre lanzar errores, aunque TypeScript pueda inferirlo.
- Utilizar funciones que retornan `never` para manejar casos imposibles en lógica condicional o `switch` exhaustivos.

### Malas prácticas

- Usar `never` en funciones que *podrían* retornar bajo ciertas condiciones. Si hay alguna ruta de ejecución que permite un retorno normal, `never` no es el tipo adecuado (podría ser `void` u otro tipo).
- Capturar errores de funciones `never` y continuar como si nada sin un manejo adecuado, ya que esto podría ocultar problemas graves en la lógica.

---

## Funciones que nunca terminan (`while (true) {}`)

El segundo escenario principal para `never` es en funciones que contienen un bucle infinito o alguna forma de lógica que impide que la función llegue a su fin.

### Definición y utilidad

Estas funciones están diseñadas para ejecutarse indefinidamente. Un ejemplo clásico es un bucle `while (true)` que no tiene una condición de salida (`break`) alcanzable dentro del contexto de la función. El tipo `never` indica que la función, por diseño, no completará su ejecución.

#### Ejemplo

```typescript
function infiniteLoop(): never {
  // Este bucle se ejecuta indefinidamente
  while (true) { 
    console.log("Ejecutando...");
    // Aquí podría haber lógica que se repite, como escuchar eventos,
    // procesar tareas en segundo plano, etc.
    // Importante: No hay 'break' ni 'return' alcanzables.
  }
}

// Ejemplo de uso en un servidor o proceso de fondo
// console.log("Iniciando servidor...");
// infiniteLoop(); // El programa se quedaría aquí
// console.log("Servidor detenido."); // Esta línea nunca se alcanzaría
```

### Casos de uso reales y recomendables

- **Servicios o demonios**: Procesos de fondo que deben ejecutarse continuamente, como servidores web, listeners de eventos o tareas programadas que se reinician internamente.
- **Bucles principales de juegos**: En el desarrollo de juegos, el bucle principal que actualiza el estado y renderiza la pantalla a menudo se ejecuta indefinidamente hasta que el juego se cierra.
- **Sistemas embebidos o de bajo nivel**: Donde un proceso principal debe mantenerse activo constantemente.

### Consideraciones importantes

- **Bloqueo del hilo**: Una función que nunca termina bloqueará el hilo de ejecución en el que se ejecuta (en JavaScript/Node.js, generalmente el hilo principal), a menos que se maneje específicamente con concurrencia (Workers, etc.). Esto es crucial en entornos como Node.js o navegadores, donde bloquear el hilo principal puede congelar la aplicación.
- **Propósito claro**: Deben usarse con un propósito muy específico y generalmente en contextos donde la ejecución continua es el comportamiento deseado y esperado.

### Buenas prácticas

- Reservar funciones `never` de bucle infinito para puntos de entrada de procesos o tareas que deben ejecutarse continuamente.
- Asegurarse de que el bloqueo del hilo (si ocurre) sea intencional y no afecte negativamente a la capacidad de respuesta de la aplicación (especialmente en entornos de un solo hilo como JavaScript).
- Documentar claramente por qué la función está diseñada para nunca terminar.

### Malas prácticas

- Crear bucles infinitos accidentalmente sin una condición de salida adecuada.
- Usar una función `never` de bucle infinito donde una función de larga duración pero finita sería más apropiada.
- Bloquear el hilo principal en aplicaciones interactivas (como una aplicación web) con un bucle infinito, impidiendo que la interfaz de usuario responda.
- No considerar las implicaciones de recursos (CPU, memoria) de un bucle que se ejecuta continuamente.
