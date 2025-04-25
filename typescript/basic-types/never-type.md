<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/never-type)
<!-- MULTILANGUAJE MENU END -->

<!--
# El tipo `never` para funciones que no devuelven valores

- Funciones que arrojan errores (`throw`)
- Funciones que nunca terminan (`while (true) {}`)
-->

# El tipo `never` para funciones que no devuelven valores

El tipo `never` en TypeScript representa el tipo de valor que nunca ocurre. Es fundamental para describir funciones que no retornan ningún valor porque terminan abruptamente, ya sea lanzando una excepción o ejecutándose indefinidamente. Comprender el uso correcto de `never` permite escribir código más seguro, robusto y expresivo, facilitando la detección de errores y el control exhaustivo de flujos en aplicaciones complejas.

## Funciones que arrojan errores (`throw`)

Las funciones que lanzan una excepción y nunca retornan un valor utilizan el tipo `never` como tipo de retorno. Esto es útil para indicar explícitamente que, tras la ejecución de la función, el flujo del programa no continuará normalmente.

### Contexto y motivación

En aplicaciones reales, es común definir funciones utilitarias para validar datos, manejar errores o implementar aserciones. Cuando estas funciones detectan una condición inválida, suelen lanzar una excepción y nunca devuelven un valor. Declarar el tipo de retorno como `never` ayuda a que el compilador detecte usos incorrectos y mejora la legibilidad del código.

#### Ejemplo básico de función que arroja un error

```typescript
function throwError(message: string): never {
  // Esta función siempre lanza una excepción y nunca retorna
  throw new Error(message);
}
```

En este ejemplo, el compilador sabe que cualquier llamada a `throwError` interrumpe el flujo de ejecución.

#### Caso de uso real: validación de parámetros

Supongamos que queremos validar parámetros de entrada en una función. Si el parámetro es inválido, lanzamos un error:

```typescript
function validatePositiveNumber(value: number): void {
  if (value <= 0) {
    throwError('El valor debe ser un número positivo');
  }
  // Resto de la lógica...
}
```

Aquí, `throwError` garantiza que, si se detecta un valor inválido, la ejecución se detiene inmediatamente.

### Consideraciones importantes

- El tipo `never` permite al compilador detectar ramas de código inalcanzables, mejorando la seguridad.
- Es útil en funciones de aserción, validaciones y utilidades de manejo de errores.
- Si una función puede retornar en algún caso, su tipo de retorno no debe ser `never`.

### Buenas prácticas

- Utilizar `never` solo cuando la función realmente nunca retorna.
- Documentar claramente el propósito de la función y el motivo por el que nunca retorna.
- Evitar abusar de funciones que lanzan errores como mecanismo de control de flujo general.

### Malas prácticas

- Declarar `never` en funciones que pueden retornar en algún caso.
- Utilizar excepciones para controlar flujos normales del programa.

### Errores comunes

- Olvidar declarar el tipo `never` en funciones que solo lanzan errores puede dificultar la comprensión del código y la detección de errores en tiempo de compilación.

---

## Funciones que nunca terminan (`while (true) {}`)

Otra situación en la que se utiliza el tipo `never` es en funciones que entran en bucles infinitos y nunca finalizan su ejecución de forma normal. Este patrón es menos frecuente, pero resulta útil en ciertos contextos como servidores, listeners o procesos que deben permanecer activos indefinidamente.

### Contexto y motivación

En sistemas que requieren procesos en ejecución continua, como servidores HTTP, listeners de eventos o demonios, es habitual implementar funciones que nunca retornan. Declarar el tipo de retorno como `never` comunica claramente esta intención y ayuda a evitar errores de lógica.

#### Ejemplo básico de función con bucle infinito

```typescript
function infiniteLoop(): never {
  while (true) {
    // Proceso que nunca termina
  }
}
```

#### Caso de uso real: servidor que escucha conexiones

```typescript
function startServer(): never {
  while (true) {
    // Lógica para aceptar y manejar conexiones
    // Por ejemplo, en un servidor TCP
  }
}
```

En estos casos, el compilador entiende que la función nunca retorna y puede optimizar el análisis de flujo.

### Consideraciones importantes

- El uso de `never` en bucles infinitos debe estar justificado y documentado.
- Es fundamental asegurar que no existen condiciones de escape no controladas.
- Utilizar este patrón solo cuando sea estrictamente necesario.

### Buenas prácticas

- Documentar claramente la razón por la que la función nunca termina.
- Utilizar mecanismos de control externo para detener el proceso si es necesario (por ejemplo, señales del sistema operativo).

### Malas prácticas

- Implementar bucles infinitos sin justificación clara.
- No prever mecanismos de parada o control externo cuando sea necesario.

### Errores comunes

- Olvidar documentar el comportamiento infinito de la función puede llevar a malentendidos y errores de mantenimiento.
- No manejar adecuadamente los recursos dentro de bucles infinitos puede provocar fugas de memoria o bloqueos. 