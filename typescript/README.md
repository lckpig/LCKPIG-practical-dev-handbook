<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript)
<!-- MULTILANGUAJE MENU END -->

<details>
<summary>1. Introducción a TypeScript</summary>

- [**Historia y evolución de TypeScript**](introduction/history-evolution.md)
    - Creación por Microsoft y motivaciones detrás de TypeScript
    - Diferencias clave entre TypeScript y JavaScript
    - Versiones destacadas y mejoras introducidas en cada una
- [**Ventajas y características principales de TypeScript**](introduction/advantages-features.md)
    - Tipado estático y detección temprana de errores
    - Compatibilidad con JavaScript y transpilación a ES5/ES6+
    - Soporte para programación orientada a objetos y genéricos
    - Integración con editores de código y herramientas de desarrollo
- [**Cómo funciona TypeScript internamente**](introduction/how-it-works.md)
    - Proceso de transpilación (`tsc`)
    - Conversión de código TypeScript a JavaScript estándar
    - Archivos de definición de tipos (`.d.ts`)
- [**Diferencias clave entre TypeScript y JavaScript**](introduction/key-differences.md)
    - Tipado estático vs. tipado dinámico
    - Interfaces y alias de tipos
    - Compatibilidad con módulos y namespaces

</details>

<details>
<summary>2. Instalación y Configuración del Entorno</summary>

- [**Instalación de TypeScript**](installation-configuration/installation.md)
    - Instalación global con `npm install -g typescript`
    - Instalación en un proyecto con `npm install --save-dev typescript`
    - Verificación de la instalación con `tsc --version`
- [**Configuración básica del compilador (`tsconfig.json`)**](installation-configuration/compiler-config.md)
    - Generación de `tsconfig.json` con `tsc --init`
    - Parámetros esenciales (`target`, `module`, `strict`, `outDir`, `rootDir`)
    - Compilación incremental con `incremental: true`
- [**Ejecución de código TypeScript**](installation-configuration/code-execution.md)
    - Compilación manual con `tsc archivo.ts`
    - Compilación automática con `tsc --watch`
    - Uso de `ts-node` para ejecutar TypeScript sin compilar (`npx ts-node archivo.ts`)
- [**Configuración en editores y herramientas de desarrollo**](installation-configuration/editor-setup.md)
    - Configuración en VS Code con soporte para TypeScript
    - Integración con ESLint y Prettier para formateo de código
    - Extensiones recomendadas en Visual Studio Code

</details>

<details>
<summary>3. Tipos Básicos en TypeScript</summary>

- [**Tipos primitivos en TypeScript**](basic-types/primitive-types.md)
    - `string`, `number`, `boolean`, `null`, `undefined`
    - Diferencias entre `null` y `undefined`
    - Uso de `bigint` para operaciones con grandes números
- [**Tipado en variables y constantes**](basic-types/variable-typing.md)
    - Declaración con `let`, `const` y su relación con los tipos
    - Inferencia de tipos vs. anotaciones explícitas
- [**El tipo `any` y su impacto en el código**](basic-types/any-type.md)
    - Cuándo usar `any` y sus riesgos
    - Alternativas seguras con `unknown`
- [**El tipo `void` y su uso en funciones**](basic-types/void-type.md)
    - Diferencias entre `void` y `undefined` en retornos
    - Uso en funciones sin retorno explícito
- [**El tipo `never` para funciones que no devuelven valores**](basic-types/never-type.md)
    - Funciones que arrojan errores (`throw`)
    - Funciones que nunca terminan (`while (true) {}`)
- [**Arrays y Tuplas en TypeScript**](basic-types/arrays-tuples.md)
    - Declaración de arrays (`number[]`, `Array<string>`)
    - Uso de tuplas (`[string, number]`)
    - Tuplas con etiquetas (`[id: number, nombre: string]`)

</details>

<details>
<summary>4. Inferencia y Anotaciones de Tipos</summary>

- [**Inferencia de tipos en TypeScript**](type-inference-annotations/type-inference.md)
    - Inferencia automática en variables (`let x = 10; // x es number`)
    - Inferencia en funciones (`function suma(a, b) { return a + b; }`)
    - Inferencia contextual basada en el uso de valores
- [**Anotaciones de tipos en variables y funciones**](type-inference-annotations/type-annotations.md)
    - Especificación manual de tipos (`let nombre: string = "TypeScript";`)
    - Anotaciones en parámetros de funciones (`function saludar(nombre: string) {}`)
    - Retorno explícito de funciones (`function sumar(a: number, b: number): number {}`)
- [**El uso de `unknown` como alternativa segura a `any`**](type-inference-annotations/unknown-vs-any.md)
    - Diferencias entre `unknown` y `any`
    - Restricciones de `unknown` para evitar errores de tipado
- [**Tipado de funciones y expresiones de función**](type-inference-annotations/function-typing.md)
    - Declaración de funciones con tipos de entrada y salida
    - Uso de `type` y `interface` para definir funciones reutilizables
- [**Type Assertions (`as` y `<Type>`)**](type-inference-annotations/type-assertions.md)
    - Conversión de tipos en tiempo de compilación
    - Cuándo usar `as` y `<Type>` y sus diferencias
    - Riesgos y mejores prácticas en `Type Assertions`

</details>

<details>
<summary>5. Tipos Avanzados en TypeScript</summary>

- [**Unión de tipos (`Union Types`)**](advanced-types/union-types.md)
    - Uso de `|` para permitir múltiples tipos (`let valor: string | number;`)
    - Validaciones en funciones con unión de tipos
- [**Intersección de tipos (`Intersection Types`)**](advanced-types/intersection-types.md)
    - Combinación de múltiples tipos con `&`
    - Casos de uso en estructuras de datos complejas
- [**El tipo `unknown` vs `any` (Avanzado)**](advanced-types/unknown-vs-any-advanced.md)
    - Diferencias y cuándo usar cada uno
    - Restricciones de `unknown` en operaciones
- [**El tipo `never` y su aplicación (Avanzado)**](advanced-types/never-type-advanced.md)
    - Funciones que nunca devuelven un valor (`throw new Error()`)
    - Uso en validaciones exhaustivas
- [**Literal Types y Enums**](advanced-types/literal-enums.md)
    - Tipos literales (`type Color = "rojo" | "verde" | "azul"`)
    - Definición y uso de `enum` (`enum Estado { Activo, Inactivo }`)
    - Enums con valores numéricos y de cadena
- [**El operador `typeof` en TypeScript**](advanced-types/typeof-operator.md)
    - Inferencia de tipos basada en valores existentes
    - Uso en funciones genéricas
- [**`keyof`, `typeof` y `in` en TypeScript**](advanced-types/keyof-typeof-in.md)
    - Uso de `keyof` para acceder a las claves de un objeto
    - `typeof` en combinación con `keyof`
    - El operador `in` para validaciones de propiedades

</details>

<details>
<summary>6. Manejo de Tipos Utilitarios en TypeScript</summary>

- [**Tipos parciales y opcionales**](utility-types/partial-required.md)
    - `Partial<T>`: Conversión de todas las propiedades a opcionales
    - `Required<T>`: Conversión de todas las propiedades a obligatorias
- [**Manipulación de objetos con `Pick`, `Omit` y `Record`**](utility-types/pick-omit-record.md)
    - `Pick<T, K>`: Seleccionar propiedades específicas de un tipo
    - `Omit<T, K>`: Excluir propiedades de un tipo
    - `Record<K, T>`: Creación de un tipo con claves y valores específicos
- [**El tipo `Readonly<T>` y su aplicación**](utility-types/readonly-type.md)
    - Evitar modificaciones en objetos con `Readonly<T>`
    - Casos de uso en estructuras inmutables
- [**`Extract<T, U>` y `Exclude<T, U>`**](utility-types/extract-exclude.md)
    - `Extract<T, U>`: Extraer solo los tipos coincidentes
    - `Exclude<T, U>`: Remover tipos específicos
- [**`NonNullable<T>` y `ReturnType<T>`**](utility-types/nonnullable-returntype.md)
    - `NonNullable<T>`: Eliminación de `null` y `undefined` en un tipo
    - `ReturnType<T>`: Inferencia del tipo de retorno de una función
- [**Uso de `InstanceType<T>` y `ThisParameterType<T>`**](utility-types/instancetype-thisparametertype.md)
    - `InstanceType<T>`: Inferir el tipo de una instancia de clase
    - `ThisParameterType<T>`: Extraer el tipo de `this` en una función

</details>

<details>
<summary>7. Manejo de Tipos Genéricos en TypeScript</summary>

- [**Introducción a los tipos genéricos**](generic-types/introduction.md)
    - Definición de funciones genéricas (`function identidad<T>(valor: T): T { return valor; }`)
    - Beneficios de los tipos genéricos en reutilización de código
- [**Genéricos en funciones y métodos**](generic-types/generics-functions-methods.md)
    - Uso de `<T>` en parámetros de funciones
    - Aplicación de restricciones (`extends`) en genéricos
- [**Genéricos en interfaces y tipos personalizados**](generic-types/generics-interfaces-types.md)
    - Creación de interfaces genéricas (`interface Caja<T> { contenido: T; }`)
    - Tipos con múltiples parámetros genéricos
- [**Genéricos en clases**](generic-types/generics-classes.md)
    - Implementación de clases genéricas (`class Repositorio<T>`)
    - Casos de uso en modelos de datos
- [**Uso de `keyof` y `typeof` en genéricos**](generic-types/keyof-typeof-generics.md)
    - Acceso a claves dinámicamente con `keyof`
    - Inferencia de tipos basada en objetos con `typeof`
- [**Manipulación avanzada de genéricos**](generic-types/advanced-manipulation.md)
    - Tipos condicionales con `extends` (`T extends U ? X : Y`)
    - Inferencia automática con `infer` (`ReturnType<T>`)
    - Uso de `Mapped Types` para transformar estructuras

</details>

<details>
<summary>8. Manejo de Tipos Condicionales y Mapeados</summary>

- [**Introducción a los tipos condicionales**](conditional-mapped-types/introduction.md)
    - Sintaxis básica (`T extends U ? X : Y`)
    - Casos de uso en validaciones de tipos dinámicos
- [**Uso de `infer` en tipos condicionales**](conditional-mapped-types/using-infer.md)
    - Extraer tipos internos con `infer` (`ReturnType<T>`)
    - Aplicaciones avanzadas con inferencia automática
- [**Tipos mapeados (`Mapped Types`)**](conditional-mapped-types/mapped-types.md)
    - Transformación de propiedades de un objeto
    - Uso de `as` en `Mapped Types` para cambiar claves
- [**Modificación de propiedades con `Readonly<T>`, `Partial<T>` y `Required<T>`**](conditional-mapped-types/modifying-properties.md)
    - Creación de tipos derivados a partir de estructuras existentes
    - Restricción y expansión de propiedades
- [**Uso de `Record<K, T>` en la creación de estructuras dinámicas**](conditional-mapped-types/using-record.md)
    - Creación de objetos tipados con claves y valores específicos
    - Casos de uso en estructuras de configuración
- [**Ejemplos avanzados de tipos condicionales**](conditional-mapped-types/advanced-examples.md)
    - Implementación de filtros y transformaciones en tiempo de compilación
    - Creación de `DeepPartial<T>` para hacer tipos anidados opcionales

</details>

<details>
<summary>9. Manejo de Tipos Recursivos y Avanzados</summary>

- [**Tipos recursivos en TypeScript**](recursive-advanced-types/recursive-types.md)
    - Definición de estructuras recursivas (`type Nodo<T> = { valor: T; hijos?: Nodo<T>[] };`)
    - Uso en estructuras de datos como árboles y listas anidadas
- [**`DeepPartial<T>` y `DeepReadonly<T>`**](recursive-advanced-types/deep-partial-readonly.md)
    - Transformación de estructuras anidadas a opcionales (`DeepPartial<T>`)
    - Aplicación de inmutabilidad en niveles profundos con `DeepReadonly<T>`
- [**Manipulación de tuplas y arrays avanzados**](recursive-advanced-types/advanced-tuples-arrays.md)
    - Uso de `T[number]` para extraer valores de arrays tipados
    - Concatenación y manipulación de tuplas (`[...T, U]`)
    - Creación de tuplas dinámicas con `Extract<T, U>`
- [**Inferencia avanzada con `infer` y `keyof`**](recursive-advanced-types/advanced-inference.md)
    - Uso de `infer` en la desestructuración de tipos
    - Creación de utilitarios personalizados con `keyof` y `Mapped Types`
- [**Ejemplos prácticos de tipos avanzados**](recursive-advanced-types/practical-examples.md)
    - Implementación de validaciones de tipo en tiempo de compilación
    - Uso de `IsNever<T>` y `IsUnknown<T>` para control de flujo de tipos

</details>

<details>
<summary>10. Módulos y Namespaces</summary>

- [**Manejo de módulos en TypeScript**](modules-namespaces/handling-modules.md)
    - Diferencias entre `ES Modules` y `CommonJS`
    - Importaciones y exportaciones (`import { algo } from './archivo'`, `export function algo()`)
    - Exportaciones por defecto vs. exportaciones nombradas
- [**Organización del código con módulos**](modules-namespaces/code-organization.md)
    - Uso de `index.ts` para centralizar exportaciones
    - Separación de responsabilidades en módulos reutilizables
- [**Namespaces en TypeScript**](modules-namespaces/namespaces.md)
    - Definición de un `namespace` (`namespace MiEspacio { export class MiClase {} }`)
    - Importación de elementos de un `namespace` (`MiEspacio.MiClase`)
    - Diferencias entre `namespace` y `module` en TypeScript moderno
- [**Configuración de módulos en `tsconfig.json`**](modules-namespaces/module-config.md)
    - Parámetros `module`, `moduleResolution`, `baseUrl`, `paths`
    - Alias de módulos con `paths` y `baseUrl`
- [**Uso de módulos con bundlers y frameworks**](modules-namespaces/bundlers-frameworks.md)
    - Configuración en Webpack, Rollup y Vite
    - Integración con Node.js y `ts-node`

</details>

<details>
<summary>11. Programación Orientada a Objetos en TypeScript</summary>

- [**Clases en TypeScript**](object-oriented-programming/classes.md)
    - Declaración de clases (`class Persona {}`)
    - Propiedades y métodos públicos, privados y protegidos
    - Constructores y sobrecarga de constructores
- [**Herencia y superclases**](object-oriented-programming/inheritance.md)
    - Uso de `extends` para heredar de otra clase
    - Llamada al constructor padre con `super()`
- [**Interfaces y clases abstractas**](object-oriented-programming/interfaces-abstract-classes.md)
    - Diferencias entre `interface` y `abstract class`
    - Implementación de interfaces en clases con `implements`
- [**Modificadores de acceso y encapsulación**](object-oriented-programming/access-modifiers.md)
    - `public`, `private`, `protected`, `readonly`
    - Métodos `get` y `set` para control de acceso a propiedades
- [**Métodos y propiedades estáticas**](object-oriented-programming/static-members.md)
    - Declaración con `static`
    - Acceso a métodos sin instanciar la clase
- [**Patrones de diseño aplicados en TypeScript**](object-oriented-programming/design-patterns.md)
    - Uso de `Singleton`, `Factory`, `Decorator`
    - Implementación de `Strategy` y `Observer` en TypeScript

</details>

<details>
<summary>12. Programación Funcional con TypeScript</summary>

- **Principios de programación funcional en TypeScript**
    - Inmutabilidad y funciones puras
    - Evitar efectos secundarios en funciones
- **Funciones de orden superior y callbacks**
    - Paso de funciones como argumentos (`map()`, `filter()`, `reduce()`)
    - Creación de funciones de orden superior
- **Closures y currying en TypeScript**
    - Uso de closures para encapsular datos
    - Implementación de currying para parcializar funciones
- **Uso de tipos genéricos en funciones funcionales**
    - Creación de funciones genéricas (`function procesar<T>(valor: T): T {}`)
    - Aplicaciones de `Partial<T>`, `Readonly<T>`, `Pick<T, K>` en programación funcional
- **Composición de funciones y `pipe`**
    - Encadenamiento de funciones con composición (`f(g(x))`)
    - Implementación del patrón `pipe()`
- **Uso de `ReadonlyArray<T>` y `ReadonlyMap<K, V>`**
    - Evitar mutaciones en listas y estructuras de datos

</details>

<details>
<summary>13. TypeScript con JavaScript Asíncrono</summary>

- **Manejo de Promesas en TypeScript**
    - Tipado de promesas (`Promise<T>`)
    - Retorno de promesas tipadas en funciones
- **Uso de `async/await` en TypeScript**
    - Declaración de funciones asíncronas con `async`
    - Espera de promesas con `await`
- **Tipado de funciones asíncronas**
    - Tipado explícito de funciones `async` (`async function obtenerDatos(): Promise<string>`)
    - Tipado de errores en `try...catch`
- **`Promise.all()`, `Promise.race()`, `Promise.allSettled()`**
    - Tipado y uso avanzado en concurrencia
- **AbortController y cancelación de Promesas**
    - Implementación de `AbortController` en `fetch`
    - Uso de `signal` para cancelar peticiones HTTP
- **Manejo de errores en código asíncrono**
    - Uso de `catch` en Promesas
    - Estrategias con `try...catch` en funciones `async`

</details>

<details>
<summary>14. Manipulación del DOM con TypeScript</summary>

- **Acceso a elementos del DOM con TypeScript**
    - Tipado de `document.getElementById()`, `querySelector()` y `querySelectorAll()`
    - Uso de `HTMLElement`, `HTMLInputElement`, `HTMLButtonElement` y otros tipos específicos
- **Modificación de elementos en el DOM**
    - Cambio de contenido con `textContent` y `innerHTML`
    - Manipulación de atributos con `setAttribute()` y `getAttribute()`
- **Eventos en TypeScript**
    - Tipado de eventos (`MouseEvent`, `KeyboardEvent`, `Event`)
    - Manejo de `addEventListener()` con tipos específicos
- **Creación y eliminación de elementos**
    - `document.createElement()`, `appendChild()`, `removeChild()`
    - Uso de `insertAdjacentHTML()` para insertar contenido dinámico
- **Delegación de eventos y `event.target` tipado**
    - Implementación de delegación de eventos en listas dinámicas
    - Uso seguro de `event.target` con `as HTMLElement`
- **Uso de `MutationObserver` para detectar cambios en el DOM**
    - Implementación de `MutationObserver`
    - Casos de uso en aplicaciones dinámicas

</details>

<details>
<summary>15. Tipado Estricto y Estrategias de Seguridad</summary>

- **Activación del modo estricto en TypeScript**
    - Configuración de `strict: true` en `tsconfig.json`
    - Efectos de `