<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/any-type)
<!-- MULTILANGUAJE MENU END -->

<!--
# El tipo `any` y su impacto en el código

- Cuándo usar `any` y sus riesgos
- Alternativas seguras con `unknown`
-->

# El tipo `any` y su impacto en el código

## Cuándo usar `any` y sus riesgos

El tipo `any` en TypeScript representa una vía de escape del sistema de tipos, permitiendo que cualquier valor sea asignado a una variable sin restricciones ni comprobaciones en tiempo de compilación. Aunque puede parecer útil en situaciones donde la tipificación estricta resulta un obstáculo, su uso indiscriminado puede acarrear graves problemas de mantenibilidad, fiabilidad y seguridad en el código.

El uso de `any` suele justificarse en escenarios como la migración de proyectos JavaScript a TypeScript, la integración con librerías externas sin tipado o cuando se manipulan datos de origen desconocido. Sin embargo, recurrir a `any` elimina las ventajas clave de TypeScript, como la detección temprana de errores y el autocompletado inteligente.

### Riesgos y consecuencias de abusar de `any`

- **Pérdida de seguridad de tipos:** Al utilizar `any`, el compilador deja de advertir sobre posibles errores de tipo, permitiendo operaciones peligrosas y propensas a fallos en tiempo de ejecución.
- **Dificultad en el mantenimiento:** El código con variables `any` es más difícil de entender y refactorizar, ya que no existe información fiable sobre los tipos esperados.
- **Propagación de errores:** Un valor `any` puede contaminar otras partes del código, propagando la inseguridad de tipos y dificultando la localización de errores.
- **Limitación de herramientas:** Se pierde la ayuda de autocompletado, refactorización segura y navegación por tipos que ofrece el ecosistema TypeScript.

#### Ejemplo de uso problemático de `any`

```typescript
// Se permite cualquier operación, aunque sea incorrecta
declare function fetchData(): any;

const result = fetchData();
// No hay advertencias, aunque la propiedad no exista
document.body.innerHTML = result.nonExistentProperty;
```

En este ejemplo, el uso de `any` impide detectar que `nonExistentProperty` no existe realmente, lo que puede provocar errores en tiempo de ejecución difíciles de depurar.

### Casos en los que puede ser aceptable usar `any`

- Prototipado rápido o migración inicial de código JavaScript.
- Interacción con APIs externas sin definiciones de tipos disponibles.
- Manipulación de datos dinámicos donde el tipo es realmente desconocido y no se puede inferir razonablemente.

{% hint style="warning" %}
El uso de `any` debe ser siempre una decisión consciente y justificada, nunca una solución por defecto ante problemas de tipado.
{% endhint %}

---

## Alternativas seguras con `unknown`

El tipo `unknown` es una alternativa más segura a `any` cuando se necesita flexibilidad sin renunciar a la seguridad de tipos. A diferencia de `any`, `unknown` obliga a realizar comprobaciones explícitas antes de manipular el valor, evitando así errores accidentales.

### Diferencias clave entre `any` y `unknown`

Antes de manipular un valor de tipo `unknown`, TypeScript exige que se realicen comprobaciones de tipo, lo que añade una capa de seguridad y reduce la probabilidad de errores en tiempo de ejecución.

#### Ejemplo de uso seguro con `unknown`

```typescript
// La función puede devolver cualquier tipo de dato
declare function fetchData(): unknown;

const result = fetchData();
// Error: no se puede acceder a propiedades sin comprobar el tipo
// document.body.innerHTML = result.nonExistentProperty;

if (typeof result === 'object' && result !== null && 'nonExistentProperty' in result) {
  // Ahora es seguro acceder a la propiedad
  document.body.innerHTML = (result as any).nonExistentProperty;
}
```

En este caso, TypeScript obliga a comprobar el tipo antes de acceder a propiedades, lo que previene errores comunes y mejora la robustez del código.

### Ventajas de `unknown` sobre `any`

- **Mayor seguridad:** Obliga a validar el tipo antes de operar sobre el valor.
- **Mejor mantenibilidad:** Facilita la comprensión y el refactorizado del código.
- **Prevención de errores:** Reduce la probabilidad de fallos en tiempo de ejecución.

### Buenas prácticas y recomendaciones

- **Prefiere `unknown` sobre `any` siempre que sea posible.**
- **Limita el uso de `any` a zonas muy acotadas y documentadas del código.**
- **Utiliza utilidades de TypeScript como type guards, assertions y tipos personalizados para refinar el tipo de los datos dinámicos.**

#### Ejemplo de type guard personalizado

```typescript
// Type guard para comprobar si un valor es de tipo User
interface User {
  id: number;
  name: string;
}

function isUser(value: unknown): value is User {
  return (
    typeof value === 'object' &&
    value !== null &&
    'id' in value &&
    'name' in value
  );
}

const data: unknown = fetchData();
if (isUser(data)) {
  // Aquí TypeScript sabe que data es User
  console.log(data.name);
}
```

### Malas prácticas a evitar

- Utilizar `any` como solución rápida ante errores de tipado sin analizar el problema de fondo.
- Propagar variables de tipo `any` a lo largo del código, contaminando otras funciones y módulos.
- Omitir comprobaciones de tipo al trabajar con datos dinámicos o externos.

{% hint style="danger" %}
El abuso de `any` puede anular por completo los beneficios de TypeScript y convertir el código en una base tan frágil como JavaScript puro.
{% endhint %}

### Errores comunes y trampas habituales

- Asumir que el uso de `any` es inocuo en pequeñas partes del código; estos focos de inseguridad tienden a expandirse y dificultar el mantenimiento.
- No revisar ni refactorizar variables `any` tras una migración inicial a TypeScript.
- Olvidar documentar los motivos por los que se recurre a `any` en casos excepcionales.

### Comparativa entre `any` y `unknown`

A continuación se muestra una tabla comparativa entre ambos tipos para clarificar sus diferencias y usos recomendados:

### Diferencias entre `any` y `unknown`

Antes de la tabla, es importante entender que aunque ambos tipos permiten flexibilidad, solo `unknown` preserva la seguridad de tipos.

| Característica     | any                          | unknown                         |
| ------------------ | ---------------------------- | ------------------------------- |
| Seguridad de tipos | No hay comprobación          | Requiere comprobación explícita |
| Autocompletado     | No disponible                | Disponible tras comprobación    |
| Refactorización    | Difícil, propaga inseguridad | Más sencilla, mantiene tipado   |
| Uso recomendado    | Casos muy excepcionales      | Datos dinámicos o externos      |
| Riesgo de errores  | Alto                         | Bajo                            |
