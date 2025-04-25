<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/any-type)
<!-- MULTILANGUAJE MENU END -->

<!--
# El tipo `any` y su impacto en el código

- Cuándo usar `any` y sus riesgos
- Alternativas seguras con `unknown`
-->

# El tipo `any` y su impacto en el código

El tipo `any` en TypeScript representa una herramienta poderosa pero peligrosa, ya que permite desactivar el sistema de tipos y trabajar con valores de cualquier tipo sin restricciones. Aunque puede parecer una solución rápida ante problemas de tipado, su uso indiscriminado puede acarrear consecuencias graves en la mantenibilidad, robustez y calidad del código. A continuación, se desarrollan en profundidad los aspectos clave relacionados con el uso de `any`, sus riesgos, alternativas y buenas prácticas.

## Cuándo usar `any` y sus riesgos

El tipo `any` debería considerarse únicamente como último recurso, reservado para situaciones excepcionales donde no es posible determinar el tipo de un valor en tiempo de desarrollo, o cuando se está migrando código JavaScript heredado a TypeScript y se requiere una transición gradual.

### Ejemplo de uso de `any` en migraciones
```typescript
// Migración de código JavaScript antiguo
function processData(data: any) {
  // Se permite cualquier operación sobre data
  return data.value ? data.value : null;
}
```
// En este caso, el uso de `any` puede ser temporal mientras se identifican los tipos reales.
```

Sin embargo, abusar de `any` elimina las ventajas principales de TypeScript: la verificación de tipos y la detección temprana de errores. El uso excesivo de `any` puede provocar:
- Pérdida de seguridad de tipos, permitiendo errores en tiempo de ejecución difíciles de detectar.
- Dificultad para refactorizar el código, ya que el compilador no puede advertir sobre incompatibilidades.
- Reducción de la autocompletación y ayuda contextual en los editores.

### Caso real: error silencioso por uso de `any`
```typescript
function calculateTotal(order: any) {
  // Se asume que order tiene una propiedad amount
  return order.amount * 1.21; // Si order.amount es undefined, el resultado será NaN
}
```
// Si el objeto recibido no tiene la estructura esperada, el error pasará desapercibido hasta tiempo de ejecución.

#### Consideraciones importantes
- El uso de `any` puede ser necesario en integraciones con librerías externas sin tipado, pero siempre debe documentarse y limitarse a la menor superficie posible.
- Es recomendable encapsular el uso de `any` en funciones o módulos específicos, evitando su propagación por todo el proyecto.

#### Buenas prácticas
- Utilizar `any` solo cuando no exista una alternativa viable.
- Documentar claramente los motivos de su uso y planificar su eliminación progresiva.
- Preferir tipos más restrictivos o genéricos (`unknown`, `Record<string, unknown>`, etc.) cuando sea posible.

#### Malas prácticas
- Declarar variables, parámetros o propiedades como `any` por comodidad o desconocimiento del tipo real.
- Permitir que el uso de `any` se extienda sin control, comprometiendo la calidad del código.

## Alternativas seguras con `unknown`

El tipo `unknown` es una alternativa más segura a `any`, ya que obliga a realizar comprobaciones de tipo antes de manipular el valor. Esto permite mantener la flexibilidad sin renunciar a la seguridad de tipos.

### Ejemplo de uso de `unknown`
```typescript
function handleInput(input: unknown) {
  // Es necesario comprobar el tipo antes de operar
  if (typeof input === 'string') {
    // input es string en este bloque
    return input.toUpperCase();
  }
  // Otros casos de uso
  return null;
}
```
// Este enfoque previene errores al obligar a validar el tipo antes de acceder a propiedades o métodos.

### Caso de uso: integración con APIs externas
Cuando se reciben datos de una API cuyo formato puede variar, es preferible tipar la respuesta como `unknown` y validar su estructura antes de utilizarla:

```typescript
async function fetchData(): Promise<unknown> {
  const response = await fetch('https://api.example.com/data');
  return response.json();
}

async function processData() {
  const data = await fetchData();
  if (typeof data === 'object' && data !== null && 'value' in data) {
    // Se puede acceder a data.value de forma segura
    return (data as { value: number }).value;
  }
  // Manejo de error o estructura inesperada
  return null;
}
```

#### Consideraciones importantes
- `unknown` fuerza a los desarrolladores a pensar en los posibles tipos y a validar explícitamente los datos, reduciendo errores.
- Es especialmente útil en librerías, utilidades genéricas y validación de datos dinámicos.

#### Buenas prácticas
- Utilizar `unknown` como tipo de retorno en funciones que pueden recibir datos de fuentes no confiables.
- Implementar validaciones exhaustivas antes de realizar conversiones de tipo (type assertions).

#### Malas prácticas
- Convertir inmediatamente un valor `unknown` a un tipo concreto sin validación previa.
- Usar `unknown` como sustituto de un tipado adecuado cuando sí es posible definirlo.

### Errores comunes y trampas habituales
- Asumir que `any` y `unknown` son equivalentes: `any` desactiva el sistema de tipos, mientras que `unknown` lo refuerza.
- Olvidar validar los datos antes de operar con valores `unknown`, lo que puede llevar a errores en tiempo de ejecución.
- No planificar la eliminación progresiva de `any` tras una migración, dejando el código indefinidamente sin tipado.

### Recomendaciones finales sobre el uso de `any`
- Limitar su uso a casos estrictamente necesarios y documentados.
- Priorizar siempre alternativas más seguras y robustas.
- Revisar periódicamente el código para identificar y eliminar usos innecesarios de `any`. 