<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/any-type)
<!-- MULTILANGUAJE MENU END -->

<details>
<summary>Índice de contenidos</summary>

<!-- no toc -->
- [Cuándo usar `any` y sus riesgos](#cuando-usar-any-y-sus-riesgos)
  - [Definición, Contexto y Motivación](#definicion-contexto-y-motivacion)
  - [Casos de uso reales y recomendables (con extrema precaución)](#casos-de-uso-reales-y-recomendables-con-extrema-precaucion)
  - [Consideraciones importantes y rendimiento](#consideraciones-importantes-y-rendimiento)
  - [Buenas prácticas (si es inevitable usar `any`)](#buenas-practicas-si-es-inevitable-usar-any)
  - [Malas prácticas a evitar](#malas-practicas-a-evitar)
  - [Errores comunes y trampas habituales](#errores-comunes-y-trampas-habituales)
- [Alternativas seguras con `unknown`](#alternativas-seguras-con-unknown)
  - [Explicación detallada de `unknown`](#explicacion-detallada-de-unknown)
  - [Uso seguro de `unknown`: Estrechamiento de Tipos (Type Narrowing)](#uso-seguro-de-unknown-estrechamiento-de-tipos-type-narrowing)
  - [Casos de uso reales y recomendables para `unknown`](#casos-de-uso-reales-y-recomendables-para-unknown)
  - [Consideraciones importantes y buenas prácticas con `unknown`](#consideraciones-importantes-y-buenas-practicas-con-unknown)

</details>

<!-- 
# El tipo `any` y su impacto en el código
- Cuándo usar `any` y sus riesgos
- Alternativas seguras con `unknown`
-->

# El tipo `any` y su impacto en el código

El tipo `any` en TypeScript representa una válvula de escape del sistema de tipos estáticos. Permite asignar cualquier tipo de valor a una variable, desactivando efectivamente las comprobaciones del compilador para esa variable específica. Si bien puede parecer una solución rápida para problemas de tipado, su uso generalizado socava los beneficios fundamentales de TypeScript, como la seguridad de tipos y la detección temprana de errores. Comprender su propósito, sus riesgos inherentes y sus alternativas es crucial para escribir código TypeScript robusto, mantenible y seguro.

## Cuándo usar `any` y sus riesgos

Utilizar `any` es indicarle explícitamente al compilador de TypeScript que "desactive" su análisis de tipos para una variable o expresión en particular. Esto significa que puedes asignarle cualquier valor y acceder a cualquier propiedad o método sobre ella sin que el compilador genere errores, incluso si esas operaciones son inválidas y provocarán fallos en tiempo de ejecución.

### Definición, Contexto y Motivación

`any` existe principalmente por dos razones:

1.  **Interoperabilidad con JavaScript:** Facilita la integración de código TypeScript con bibliotecas JavaScript existentes que no tienen información de tipos.
2.  **Migración Gradual:** Permite migrar bases de código JavaScript a TypeScript de forma progresiva, usando `any` como marcador temporal para partes del código aún no tipadas.

La motivación detrás de `any` no es fomentar su uso indiscriminado, sino proporcionar flexibilidad en escenarios específicos donde el sistema de tipos estricto podría ser un obstáculo temporal. Sin embargo, esta flexibilidad tiene un coste muy alto en términos de seguridad y mantenibilidad.

#### Ejemplo Básico de Funcionamiento

```typescript
// Declaramos una variable con tipo any
let flexible: any = 123;

// Accedemos a un método de Number, funciona correctamente
console.log(flexible.toFixed(2)); // Salida: "123.00"

// Reasignamos un string, permitido por any
flexible = "Hola Mundo";

// Accedemos a un método de String, funciona correctamente
console.log(flexible.toUpperCase()); // Salida: "HOLA MUNDO"

// Reasignamos un booleano
flexible = true;

// Intentamos acceder a un método que no existe en boolean
// El compilador NO SE QUEJA debido a 'any'
// console.log(flexible.toFixed(2)); 
// ¡ERROR EN TIEMPO DE EJECUCIÓN! -> TypeError: flexible.toFixed is not a function
```

Este ejemplo ilustra el peligro fundamental de `any`: oculta errores potenciales que solo se manifestarán durante la ejecución del programa, anulando la promesa principal de TypeScript de detectar errores en tiempo de compilación.

### Casos de uso reales y recomendables (con extrema precaución)

Aunque la recomendación general es evitar `any`, existen situaciones muy puntuales donde su uso *podría* estar justificado, siempre como último recurso y con plena conciencia de los riesgos.

1.  **Prototipado Rápido y Exploratorio:** En fases muy tempranas de desarrollo, cuando la estructura de los datos es incierta o cambia constantemente, `any` *podría* usarse temporalmente para agilizar la experimentación. Sin embargo, es crucial reemplazarlo con tipos específicos tan pronto como la estructura se estabilice.
2.  **Integración con APIs o Bibliotecas sin Tipos (sin @types):** Si se trabaja con una biblioteca JavaScript muy antigua o poco mantenida que carece de declaraciones de tipos (`.d.ts`) y no existen definiciones de la comunidad en DefinitelyTyped (`@types/nombre-biblioteca`), `any` puede ser la única forma viable de interactuar con ella. Aun así, es preferible intentar crear definiciones básicas propias o encapsular la interacción en un módulo específico para limitar la propagación de `any`.

#### Ejemplo: Interfaz con Biblioteca Externa sin Tipos

Supongamos que usamos una biblioteca `legacyAnalytics` que no tiene tipos:

```typescript
// Asumimos que globalmente existe un objeto 'legacyAnalytics' inyectado
declare const legacyAnalytics: any; 

function trackPageView(url: string, referrer?: string): void {
  try {
    // Llamamos a métodos de la biblioteca externa usando 'any'
    // El compilador no puede verificar si 'track' o 'setReferrer' existen
    // o si los argumentos son correctos.
    legacyAnalytics.setReferrer(referrer || document.referrer);
    legacyAnalytics.track('pageView', { url: url, timestamp: Date.now() });
    console.log(`Page view tracked for: ${url}`);
  } catch (error) {
    // Es crucial manejar errores porque 'any' no ofrece garantías
    console.error("Error tracking page view with legacy system:", error);
    // Aquí podríamos implementar un fallback o log adicional
  }
}

// Uso
trackPageView('/home');
trackPageView('/profile', '/home'); 
```

En este caso, `any` permite la interacción, pero requiere un manejo de errores explícito (`try...catch`) porque no hay garantías del compilador.

### Consideraciones importantes y rendimiento

El impacto de `any` va más allá de la simple desactivación de comprobaciones:

1.  **Pérdida Total de Seguridad de Tipos:** Es la consecuencia más grave. Se pierde la capacidad del compilador para razonar sobre los tipos y prevenir errores comunes como typos en nombres de propiedades, llamadas a métodos inexistentes o paso de argumentos incorrectos.
2.  **Dificulta la Refactorización:** Renombrar una propiedad o cambiar la firma de una función se vuelve peligroso. El compilador no puede detectar todos los lugares donde el código que usa `any` podría romperse debido al cambio.
3.  **Empeora la Experiencia de Desarrollo:** Se pierde el autocompletado inteligente, la navegación por el código (ir a definición, encontrar referencias) se vuelve menos fiable y la detección de errores en el editor (linting) es ineficaz.
4.  **"Efecto Viral":** El uso de `any` tiende a propagarse. Si una función acepta o devuelve `any`, el código que la utiliza a menudo se ve obligado a usar `any` también, contaminando gradualmente la base de código.
5.  **Impacto en el Rendimiento (Indirecto):** Si bien `any` en sí no suele afectar directamente el rendimiento del código JavaScript generado, los errores en tiempo de ejecución que no se detectaron debido a `any` pueden causar fallos, ralentizaciones o comportamientos inesperados en producción. Además, la falta de tipos claros puede dificultar optimizaciones futuras.

{% hint style="danger" %}
**Riesgo Crítico:** Abusar de `any` equivale a renunciar a las ventajas de TypeScript. El código se vuelve frágil, difícil de mantener y propenso a errores en tiempo de ejecución. Debe considerarse una herramienta excepcional, no una solución habitual.
{% endhint %}

### Buenas prácticas (si es inevitable usar `any`)

1.  **Minimizar el Alcance:** Usar `any` en el lugar más específico posible. Evitar tipar objetos completos o valores de retorno de funciones como `any` si solo una pequeña parte es desconocida.
2.  **Documentar Rigurosamente:** Añadir comentarios explicando *por qué* se está usando `any`, qué estructura se espera (si se sabe algo) y si hay planes para reemplazarlo.
3.  **Validar en Tiempo de Ejecución:** Si una variable es `any`, añadir comprobaciones explícitas (`if (typeof flexible === 'string')`, `if (flexible && flexible.prop)`) antes de usar sus propiedades o métodos.
4.  **Considerar Tipos Intermedios:** A veces, en lugar de `any`, se puede definir una interfaz o tipo parcial con las propiedades conocidas y dejar el resto más abierto (aunque `unknown` suele ser mejor para esto).
5.  **Configurar Linters:** Herramientas como ESLint con plugins de TypeScript (`@typescript-eslint`) pueden configurarse para advertir o prohibir el uso explícito de `any`, ayudando a mantener la disciplina.

### Malas prácticas a evitar

1.  **Usar `any` por Pereza:** Recurrir a `any` para silenciar rápidamente errores del compilador sin entender o solucionar el problema de tipado subyacente.
2.  **Tipar Parámetros de Función y Retornos como `any`:** A menos que sea absolutamente inevitable (como en el ejemplo de la biblioteca externa), esto destruye la seguridad de tipos en los límites de la función.
3.  **Asignar `any` a Tipos Específicos sin Validación:** `let miNumero: number = miVariableAny;` Esto es peligroso y anula el propósito del tipado estático.
4.  **Dejar `any` Residual:** Olvidarse de reemplazar `any` utilizados durante la migración o el prototipado una vez que los tipos correctos son conocidos.
5.  **Ignorar las Alternativas:** No considerar `unknown` u otras técnicas de tipado más seguras antes de recurrir a `any`.

### Errores comunes y trampas habituales

-   **Falsa Sensación de Seguridad:** Creer que porque el código compila, está libre de errores de tipo, cuando `any` puede estar ocultando problemas latentes.
-   **Propagación Inconsciente:** Una variable `any` puede "colarse" en partes del código que deberían ser seguras a través de asignaciones o llamadas a funciones, degradando la calidad general del tipado.
-   **Errores de Typo:** `miAny.propiedadExistenete` vs `miAny.propidadExistente` (typo). El compilador no detectará el segundo caso si `miAny` es `any`.
-   **Confusión con `Object` o `{}`:** `any` es mucho menos restrictivo que `Object` (que solo permite acceso a métodos de `Object.prototype`) o `{}` (similar a `Object`). `any` permite *cualquier* operación.

---

## Alternativas seguras con `unknown`

Introducido en TypeScript 3.0, `unknown` es la alternativa segura y preferida a `any`. Representa un valor cuyo tipo no se conoce en tiempo de compilación, pero a diferencia de `any`, `unknown` impone restricciones estrictas: **no puedes realizar ninguna operación sobre un valor `unknown` hasta que hayas verificado o asegurado su tipo específico.**

### Explicación detallada de `unknown`

`unknown` es el "tipo superior" seguro en la jerarquía de tipos de TypeScript. Esto significa que cualquier valor puede ser asignado a una variable de tipo `unknown`, pero una variable `unknown` no puede ser asignada a ninguna otra variable con un tipo específico (excepto `any` y `unknown` mismo) sin una comprobación de tipo explícita.

#### Ejemplo Comparativo: `any` vs `unknown`

```typescript
let variableAny: any = "Soy un string";
let variableUnknown: unknown = "También soy un string";

// Con 'any', podemos operar directamente (inseguro)
console.log(variableAny.toUpperCase()); // OK en compilación, pero podría fallar si no fuera string
// variableAny = 10;
// console.log(variableAny.toUpperCase()); // ¡ERROR EN TIEMPO DE EJECUCIÓN!

// Con 'unknown', el compilador nos fuerza a verificar el tipo
// console.log(variableUnknown.toUpperCase()); // ERROR de compilación: Object is of type 'unknown'.

// Verificación obligatoria para usar 'unknown'
if (typeof variableUnknown === 'string') {
  // Dentro de este bloque, TypeScript "sabe" que variableUnknown es string
  console.log(variableUnknown.toUpperCase()); // OK
}

// Asignación
let miString: string;

miString = variableAny; // OK: 'any' se salta las comprobaciones

// miString = variableUnknown; // ERROR de compilación: Type 'unknown' is not assignable to type 'string'.

// Asignación segura después de la verificación
if (typeof variableUnknown === 'string') {
  miString = variableUnknown; // OK
}
```
La diferencia clave es que `unknown` te obliga a escribir código que maneje la incertidumbre del tipo de forma segura, utilizando mecanismos de estrechamiento de tipos (type narrowing).

### Uso seguro de `unknown`: Estrechamiento de Tipos (Type Narrowing)

Para poder utilizar un valor `unknown`, debes convencer a TypeScript de su tipo real. Esto se logra mediante varias técnicas:

1.  **Comprobaciones con `typeof`:** Ideal para tipos primitivos.
    ```typescript
    function procesarPrimitivo(valor: unknown) {
      if (typeof valor === 'string') {
        console.log("String:", valor.trim());
      } else if (typeof valor === 'number') {
        console.log("Número:", valor.toFixed(2));
      } else if (typeof valor === 'boolean') {
        console.log("Booleano:", valor ? "Verdadero" : "Falso");
      } else {
        console.log("Tipo no primitivo o null/undefined:", typeof valor);
      }
    }
    procesarPrimitivo("  Hola  ");
    procesarPrimitivo(123.456);
    procesarPrimitivo(true);
    procesarPrimitivo(null); 
    procesarPrimitivo({ a: 1 });
    ```

2.  **Comprobaciones con `instanceof`:** Útil para verificar si un valor es una instancia de una clase específica.
    ```typescript
    class Coche { conducir() { console.log("Brum brum"); } }
    class Bicicleta { pedalear() { console.log("Ñic ñic"); } }

    function moverVehiculo(vehiculo: unknown) {
      if (vehiculo instanceof Coche) {
        vehiculo.conducir(); // OK, TypeScript sabe que es un Coche
      } else if (vehiculo instanceof Bicicleta) {
        vehiculo.pedalear(); // OK, TypeScript sabe que es una Bicicleta
      } else {
        console.log("No sé cómo mover este vehículo.");
      }
    }
    moverVehiculo(new Coche());
    moverVehiculo(new Bicicleta());
    moverVehiculo("patinete"); 
    ```

3.  **Type Guards Personalizados:** Funciones que devuelven un predicado de tipo (`parametro is TipoDeseado`). Permiten crear lógica de validación compleja y reutilizable.
    ```typescript
    interface Usuario { id: number; nombre: string; }
    interface Producto { sku: string; precio: number; }

    // Type guard para Usuario
    function esUsuario(obj: unknown): obj is Usuario {
      return (
        typeof obj === 'object' && // Es un objeto
        obj !== null &&            // No es null
        'id' in obj && typeof obj.id === 'number' && // Tiene 'id' numérico
        'nombre' in obj && typeof obj.nombre === 'string' // Tiene 'nombre' string
      );
    }

    function mostrarNombre(entidad: unknown) {
      if (esUsuario(entidad)) {
        console.log("Nombre de usuario:", entidad.nombre.toUpperCase()); // OK, es Usuario
      } else {
        console.log("La entidad proporcionada no es un usuario válido.");
      }
    }

    mostrarNombre({ id: 1, nombre: "Ana" });
    mostrarNombre({ sku: "XYZ", precio: 99.9 }); 
    mostrarNombre("Juan");
    ```

4.  **Aserciones de Tipo (`as Tipo`):** Es la forma menos segura, similar a `any` pero localizada. Le dices al compilador "confía en mí, sé que este valor es de tipo `Tipo`". Debe usarse con extrema precaución, solo cuando estás absolutamente seguro del tipo y las otras técnicas no son aplicables. Abusar de ellas invalida la seguridad que `unknown` proporciona.
    ```typescript
    function obtenerLongitud(dato: unknown): number | undefined {
      // ¡PELIGROSO! Si 'dato' no es un array o string, esto fallará en tiempo de ejecución.
      // Solo usar si hay una garantía externa muy fuerte sobre el tipo.
      if (typeof (dato as any[] | string).length === 'number') { 
         return (dato as any[] | string).length;
      }
      return undefined;
    }

    console.log(obtenerLongitud("hola")); // 4
    console.log(obtenerLongitud([1, 2, 3])); // 3
    // console.log(obtenerLongitud({})); // ¡ERROR EN TIEMPO DE EJECUCIÓN! Cannot read properties of undefined (reading 'length') si no se controla bien
    ```

{% hint style="warning" %}
Las aserciones de tipo (`as Type`) son una "mentira" al compilador. Desactivan la comprobación de tipos para esa operación específica. Úsalas como último recurso absoluto, preferiblemente encapsuladas en funciones de validación seguras (type guards). Abusar de ellas introduce la misma inseguridad que `any`.
{% endhint %}

### Casos de uso reales y recomendables para `unknown`

`unknown` es la elección correcta siempre que trabajes con datos cuyo tipo no puedes garantizar en tiempo de compilación:

1.  **Datos de APIs Externas o Almacenamiento:** Al recibir JSON de una API, leer de `localStorage` o interactuar con cualquier fuente de datos externa, el tipo inicial debería ser `unknown`. Luego, se debe validar y transformar a un tipo específico.
    ```typescript
    interface ConfiguracionUsuario { tema: 'claro' | 'oscuro'; notificaciones: boolean; }

    // Type guard robusto para ConfiguracionUsuario
    function esConfiguracionUsuario(obj: unknown): obj is ConfiguracionUsuario {
        if (typeof obj !== 'object' || obj === null) return false;
        const config = obj as Record<string, unknown>; // Aserción temporal segura dentro del guard
        return (
            (config.tema === 'claro' || config.tema === 'oscuro') &&
            typeof config.notificaciones === 'boolean'
        );
    }

    async function cargarConfiguracion(): Promise<ConfiguracionUsuario> {
        const datosRaw: unknown = await fetch('/api/user-config').then(res => res.json());

        if (esConfiguracionUsuario(datosRaw)) {
            // Validación exitosa, podemos devolver el tipo específico
            return datosRaw;
        } else {
            console.error("Datos de configuración inválidos recibidos:", datosRaw);
            // Devolver una configuración por defecto o lanzar un error
            return { tema: 'claro', notificaciones: true }; 
        }
    }
    ```

{% hint style="info" %}
Bibliotecas como `zod` o `io-ts` son excelentes para definir esquemas y validar datos `unknown` de forma declarativa y segura, simplificando enormemente este proceso.
{% endhint %}

2.  **Funciones Genéricas de Alto Nivel:** Funciones que operan sobre diversos tipos de datos pero necesitan inspeccionarlos de forma segura.
3.  **Contenedores de Tipos Mixtos (con Precaución):** Si necesitas una estructura que pueda contener valores de tipos muy diferentes, `unknown[]` o `Record<string, unknown>` son más seguros que `any[]` o `Record<string, any>`, ya que obligan a verificar el tipo al extraer un elemento.
4.  **Refactorización Segura desde `any`:** Reemplazar sistemáticamente `any` por `unknown` en una base de código es un paso excelente para mejorar la seguridad. Obligará a añadir las comprobaciones de tipo necesarias que antes faltaban.

### Consideraciones importantes y buenas prácticas con `unknown`

-   **Siempre Preferir `unknown` sobre `any`:** `unknown` es la opción por defecto cuando el tipo es incierto. `any` debe ser la excepción absoluta.
-   **Implementar Validaciones Robustas:** La utilidad de `unknown` depende directamente de la calidad de las comprobaciones de tipo (type guards, `typeof`, `instanceof`). Sé exhaustivo.
-   **Evitar Aserciones de Tipo (`as`) como Atajo:** Resiste la tentación de usar `as Tipo` para silenciar errores de `unknown`. Esto anula el propósito de usar `unknown`. Invierte tiempo en escribir type guards correctos.
-   **Combinar con Genéricos:** En funciones, a menudo se puede usar genéricos (`<T>`) en lugar de `unknown` si la función debe operar sobre un tipo específico pero desconocido *a priori*, preservando el tipo original. `unknown` es mejor cuando realmente no sabes nada del tipo o necesitas manejar múltiples posibilidades explícitamente.

{% hint style="success" %}
Adoptar `unknown` en lugar de `any` representa un cambio fundamental hacia una programación TypeScript más segura y explícita. Te obliga a confrontar la incertidumbre de los tipos de manera controlada, resultando en un código más fiable, predecible y fácil de mantener a largo plazo.
{% endhint %}
