[EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript)

<details>
<summary>1. Introducción a TypeScript</summary>

- **Historia y evolución de TypeScript**
    - Creación por Microsoft y motivaciones detrás de TypeScript
    - Diferencias clave entre TypeScript y JavaScript
    - Versiones destacadas y mejoras introducidas en cada una
- **Ventajas y características principales de TypeScript**
    - Tipado estático y detección temprana de errores
    - Compatibilidad con JavaScript y transpilación a ES5/ES6+
    - Soporte para programación orientada a objetos y genéricos
    - Integración con editores de código y herramientas de desarrollo
- **Cómo funciona TypeScript internamente**
    - Proceso de transpilación (`tsc`)
    - Conversión de código TypeScript a JavaScript estándar
    - Archivos de definición de tipos (`.d.ts`)
- **Diferencias clave entre TypeScript y JavaScript**
    - Tipado estático vs. tipado dinámico
    - Interfaces y alias de tipos
    - Compatibilidad con módulos y namespaces

</details>

<details>
<summary>2. Instalación y Configuración del Entorno</summary>

- **Instalación de TypeScript**
    - Instalación global con `npm install -g typescript`
    - Instalación en un proyecto con `npm install --save-dev typescript`
    - Verificación de la instalación con `tsc --version`
- **Configuración básica del compilador (`tsconfig.json`)**
    - Generación de `tsconfig.json` con `tsc --init`
    - Parámetros esenciales (`target`, `module`, `strict`, `outDir`, `rootDir`)
    - Compilación incremental con `incremental: true`
- **Ejecución de código TypeScript**
    - Compilación manual con `tsc archivo.ts`
    - Compilación automática con `tsc --watch`
    - Uso de `ts-node` para ejecutar TypeScript sin compilar (`npx ts-node archivo.ts`)
- **Configuración en editores y herramientas de desarrollo**
    - Configuración en VS Code con soporte para TypeScript
    - Integración con ESLint y Prettier para formateo de código
    - Extensiones recomendadas en Visual Studio Code

</details>

<details>
<summary>3. Tipos Básicos en TypeScript</summary>

- **Tipos primitivos en TypeScript**
    - `string`, `number`, `boolean`, `null`, `undefined`
    - Diferencias entre `null` y `undefined`
    - Uso de `bigint` para operaciones con grandes números
- **Tipado en variables y constantes**
    - Declaración con `let`, `const` y su relación con los tipos
    - Inferencia de tipos vs. anotaciones explícitas
- **El tipo `any` y su impacto en el código**
    - Cuándo usar `any` y sus riesgos
    - Alternativas seguras con `unknown`
- **El tipo `void` y su uso en funciones**
    - Diferencias entre `void` y `undefined` en retornos
    - Uso en funciones sin retorno explícito
- **El tipo `never` para funciones que no devuelven valores**
    - Funciones que arrojan errores (`throw`)
    - Funciones que nunca terminan (`while (true) {}`)
- **Arrays y Tuplas en TypeScript**
    - Declaración de arrays (`number[]`, `Array<string>`)
    - Uso de tuplas (`[string, number]`)
    - Tuplas con etiquetas (`[id: number, nombre: string]`)

</details>

<details>
<summary>4. Inferencia y Anotaciones de Tipos</summary>

- **Inferencia de tipos en TypeScript**
    - Inferencia automática en variables (`let x = 10; // x es number`)
    - Inferencia en funciones (`function suma(a, b) { return a + b; }`)
    - Inferencia contextual basada en el uso de valores
- **Anotaciones de tipos en variables y funciones**
    - Especificación manual de tipos (`let nombre: string = "TypeScript";`)
    - Anotaciones en parámetros de funciones (`function saludar(nombre: string) {}`)
    - Retorno explícito de funciones (`function sumar(a: number, b: number): number {}`)
- **El uso de `unknown` como alternativa segura a `any`**
    - Diferencias entre `unknown` y `any`
    - Restricciones de `unknown` para evitar errores de tipado
- **Tipado de funciones y expresiones de función**
    - Declaración de funciones con tipos de entrada y salida
    - Uso de `type` y `interface` para definir funciones reutilizables
- **Type Assertions (`as` y `<Type>`)**
    - Conversión de tipos en tiempo de compilación
    - Cuándo usar `as` y `<Type>` y sus diferencias
    - Riesgos y mejores prácticas en `Type Assertions`

</details>

<details>
<summary>5. Tipos Avanzados en TypeScript</summary>

- **Unión de tipos (`Union Types`)**
    - Uso de `|` para permitir múltiples tipos (`let valor: string | number;`)
    - Validaciones en funciones con unión de tipos
- **Intersección de tipos (`Intersection Types`)**
    - Combinación de múltiples tipos con `&`
    - Casos de uso en estructuras de datos complejas
- **El tipo `unknown` vs `any`**
    - Diferencias y cuándo usar cada uno
    - Restricciones de `unknown` en operaciones
- **El tipo `never` y su aplicación**
    - Funciones que nunca devuelven un valor (`throw new Error()`)
    - Uso en validaciones exhaustivas
- **Literal Types y Enums**
    - Tipos literales (`type Color = "rojo" | "verde" | "azul"`)
    - Definición y uso de `enum` (`enum Estado { Activo, Inactivo }`)
    - Enums con valores numéricos y de cadena
- **El operador `typeof` en TypeScript**
    - Inferencia de tipos basada en valores existentes
    - Uso en funciones genéricas
- **`keyof`, `typeof` y `in` en TypeScript**
    - Uso de `keyof` para acceder a las claves de un objeto
    - `typeof` en combinación con `keyof`
    - El operador `in` para validaciones de propiedades

</details>

<details>
<summary>6. Manejo de Tipos Utilitarios en TypeScript</summary>

- **Tipos parciales y opcionales**
    - `Partial<T>`: Conversión de todas las propiedades a opcionales
    - `Required<T>`: Conversión de todas las propiedades a obligatorias
- **Manipulación de objetos con `Pick`, `Omit` y `Record`**
    - `Pick<T, K>`: Seleccionar propiedades específicas de un tipo
    - `Omit<T, K>`: Excluir propiedades de un tipo
    - `Record<K, T>`: Creación de un tipo con claves y valores específicos
- **El tipo `Readonly<T>` y su aplicación**
    - Evitar modificaciones en objetos con `Readonly<T>`
    - Casos de uso en estructuras inmutables
- **`Extract<T, U>` y `Exclude<T, U>`**
    - `Extract<T, U>`: Extraer solo los tipos coincidentes
    - `Exclude<T, U>`: Remover tipos específicos
- **`NonNullable<T>` y `ReturnType<T>`**
    - `NonNullable<T>`: Eliminación de `null` y `undefined` en un tipo
    - `ReturnType<T>`: Inferencia del tipo de retorno de una función
- **Uso de `InstanceType<T>` y `ThisParameterType<T>`**
    - `InstanceType<T>`: Inferir el tipo de una instancia de clase
    - `ThisParameterType<T>`: Extraer el tipo de `this` en una función

</details>

<details>
<summary>7. Manejo de Tipos Genéricos en TypeScript</summary>

- **Introducción a los tipos genéricos**
    - Definición de funciones genéricas (`function identidad<T>(valor: T): T { return valor; }`)
    - Beneficios de los tipos genéricos en reutilización de código
- **Genéricos en funciones y métodos**
    - Uso de `<T>` en parámetros de funciones
    - Aplicación de restricciones (`extends`) en genéricos
- **Genéricos en interfaces y tipos personalizados**
    - Creación de interfaces genéricas (`interface Caja<T> { contenido: T; }`)
    - Tipos con múltiples parámetros genéricos
- **Genéricos en clases**
    - Implementación de clases genéricas (`class Repositorio<T>`)
    - Casos de uso en modelos de datos
- **Uso de `keyof` y `typeof` en genéricos**
    - Acceso a claves dinámicamente con `keyof`
    - Inferencia de tipos basada en objetos con `typeof`
- **Manipulación avanzada de genéricos**
    - Tipos condicionales con `extends` (`T extends U ? X : Y`)
    - Inferencia automática con `infer` (`ReturnType<T>`)
    - Uso de `Mapped Types` para transformar estructuras

</details>

<details>
<summary>8. Manejo de Tipos Condicionales y Mapeados</summary>

- **Introducción a los tipos condicionales**
    - Sintaxis básica (`T extends U ? X : Y`)
    - Casos de uso en validaciones de tipos dinámicos
- **Uso de `infer` en tipos condicionales**
    - Extraer tipos internos con `infer` (`ReturnType<T>`)
    - Aplicaciones avanzadas con inferencia automática
- **Tipos mapeados (`Mapped Types`)**
    - Transformación de propiedades de un objeto
    - Uso de `as` en `Mapped Types` para cambiar claves
- **Modificación de propiedades con `Readonly<T>`, `Partial<T>` y `Required<T>`**
    - Creación de tipos derivados a partir de estructuras existentes
    - Restricción y expansión de propiedades
- **Uso de `Record<K, T>` en la creación de estructuras dinámicas**
    - Creación de objetos tipados con claves y valores específicos
    - Casos de uso en estructuras de configuración
- **Ejemplos avanzados de tipos condicionales**
    - Implementación de filtros y transformaciones en tiempo de compilación
    - Creación de `DeepPartial<T>` para hacer tipos anidados opcionales

</details>

<details>
<summary>9. Manejo de Tipos Recursivos y Avanzados</summary>

- **Tipos recursivos en TypeScript**
    - Definición de estructuras recursivas (`type Nodo<T> = { valor: T; hijos?: Nodo<T>[] };`)
    - Uso en estructuras de datos como árboles y listas anidadas
- **`DeepPartial<T>` y `DeepReadonly<T>`**
    - Transformación de estructuras anidadas a opcionales (`DeepPartial<T>`)
    - Aplicación de inmutabilidad en niveles profundos con `DeepReadonly<T>`
- **Manipulación de tuplas y arrays avanzados**
    - Uso de `T[number]` para extraer valores de arrays tipados
    - Concatenación y manipulación de tuplas (`[...T, U]`)
    - Creación de tuplas dinámicas con `Extract<T, U>`
- **Inferencia avanzada con `infer` y `keyof`**
    - Uso de `infer` en la desestructuración de tipos
    - Creación de utilitarios personalizados con `keyof` y `Mapped Types`
- **Ejemplos prácticos de tipos avanzados**
    - Implementación de validaciones de tipo en tiempo de compilación
    - Uso de `IsNever<T>` y `IsUnknown<T>` para control de flujo de tipos

</details>

<details>
<summary>10. Módulos y Namespaces</summary>

- **Manejo de módulos en TypeScript**
    - Diferencias entre `ES Modules` y `CommonJS`
    - Importaciones y exportaciones (`import { algo } from './archivo'`, `export function algo()`)
    - Exportaciones por defecto vs. exportaciones nombradas
- **Organización del código con módulos**
    - Uso de `index.ts` para centralizar exportaciones
    - Separación de responsabilidades en módulos reutilizables
- **Namespaces en TypeScript**
    - Definición de un `namespace` (`namespace MiEspacio { export class MiClase {} }`)
    - Importación de elementos de un `namespace` (`MiEspacio.MiClase`)
    - Diferencias entre `namespace` y `module` en TypeScript moderno
- **Configuración de módulos en `tsconfig.json`**
    - Parámetros `module`, `moduleResolution`, `baseUrl`, `paths`
    - Alias de módulos con `paths` y `baseUrl`
- **Uso de módulos con bundlers y frameworks**
    - Configuración en Webpack, Rollup y Vite
    - Integración con Node.js y `ts-node`

</details>

<details>
<summary>11. Programación Orientada a Objetos en TypeScript</summary>

- **Clases en TypeScript**
    - Declaración de clases (`class Persona {}`)
    - Propiedades y métodos públicos, privados y protegidos
    - Constructores y sobrecarga de constructores
- **Herencia y superclases**
    - Uso de `extends` para heredar de otra clase
    - Llamada al constructor padre con `super()`
- **Interfaces y clases abstractas**
    - Diferencias entre `interface` y `abstract class`
    - Implementación de interfaces en clases con `implements`
- **Modificadores de acceso y encapsulación**
    - `public`, `private`, `protected`, `readonly`
    - Métodos `get` y `set` para control de acceso a propiedades
- **Métodos y propiedades estáticas**
    - Declaración con `static`
    - Acceso a métodos sin instanciar la clase
- **Patrones de diseño aplicados en TypeScript**
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
    - Efectos de `strictNullChecks`, `noImplicitAny`, `strictFunctionTypes`
- **Manejo seguro de valores nulos y opcionales**
    - Uso de `strictNullChecks` para evitar valores `null` o `undefined`
    - Operador de encadenamiento opcional (`?.`)
    - Operador de coalescencia nula (`??`)
- **Uso de `unknown` en lugar de `any`**
    - Diferencias y mejores prácticas con `unknown`
    - Restricciones de uso y necesidad de validaciones
- **Seguridad en el manejo de datos y APIs**
    - Validación de entradas con `typeof` y `instanceof`
    - Uso de `never` para asegurar exhaustividad en `switch`
- **Protección contra errores en objetos y clases**
    - Implementación de `Readonly<T>` para prevenir mutaciones
    - Tipado seguro con `Partial<T>` y `Required<T>`
- **Evitar problemas en tipado de estructuras dinámicas**
    - Estrategias para manejar estructuras JSON en APIs (`Record<string, unknown>`)
    - Tipado estricto de respuestas de `fetch()`

</details>

<details>
<summary>16. Manejo de Errores y Depuración</summary>

- **Manejo de errores con `try...catch` en TypeScript**
    - Tipado de errores en bloques `catch` (`error: unknown`)
    - Uso de `instanceof` para verificar el tipo de error
- **Errores en código asíncrono**
    - Captura de errores en `async/await` con `try...catch`
    - Tipado de respuestas fallidas en Promesas
- **Depuración con `console.log()` y `console.error()`**
    - Uso eficiente de `console.table()` para visualizar objetos
    - `debugger` en DevTools del navegador
- **Integración con herramientas de depuración**
    - Uso de `tsc --watch` para detectar errores en tiempo de desarrollo
    - Depuración en VS Code con `launch.json`
- **Manejo de errores en clases y funciones**
    - Creación de clases de error personalizadas (`class CustomError extends Error`)
    - Lanzamiento controlado de errores con `throw`
- **Prevención de errores en TypeScript**
    - Uso de `strictNullChecks` y `noImplicitAny`
    - Estrategias para evitar `any` y garantizar tipado seguro

</details>

<details>
<summary>17. Buenas Prácticas y Optimización de Código</summary>

- **Estructura y organización del código**
    - Separación de lógica en módulos y archivos
    - Uso adecuado de `interfaces` y `types`
- **Escritura de código mantenible**
    - Convenciones de nombres en variables y funciones
    - Uso de `readonly` y `const` para evitar modificaciones accidentales
- **Optimización del rendimiento en TypeScript**
    - Evitar conversiones innecesarias de tipos (`as any`)
    - Uso eficiente de estructuras de datos (`Map`, `Set`, `Record<K, T>`)
- **Reducción de complejidad en funciones y clases**
    - Aplicación del principio **DRY** (Don't Repeat Yourself)
    - Uso de funciones puras y modularización
- **Prevención de errores en tiempo de compilación**
    - Habilitación de `strict` en `tsconfig.json`
    - Uso de `unknown` en lugar de `any`
- **Compatibilidad y escalabilidad en proyectos grandes**
    - Uso de `namespace` vs. `modules`
    - Implementación de `Abstract Classes` para facilitar extensibilidad

</details>

<details>
<summary>18. Decoradores en TypeScript</summary>

- **Introducción a los decoradores**
    - ¿Qué son los decoradores y cómo funcionan en TypeScript?
    - Configuración de `experimentalDecorators` en `tsconfig.json`
- **Tipos de decoradores en TypeScript**
    - **Decoradores de clase** (`@ClaseDecorator`)
    - **Decoradores de propiedad** (`@PropiedadDecorator`)
    - **Decoradores de método** (`@MetodoDecorator`)
    - **Decoradores de parámetros** (`@ParametroDecorator`)
- **Uso de decoradores en Angular**
    - `@Component()`, `@Injectable()`, `@Directive()`, `@Pipe()`
    - Personalización de decoradores en servicios y módulos
- **Uso de decoradores en NestJS**
    - `@Controller()`, `@Get()`, `@Post()`, `@Param()`, `@Body()`
    - Creación de decoradores personalizados con `Reflect.metadata()`
- **Composición y encadenamiento de decoradores**
    - Aplicación de múltiples decoradores en una misma entidad
    - Orden de ejecución de los decoradores en clases
- **Decoradores con parámetros y configuración dinámica**
    - Decoradores que aceptan argumentos (`@MiDecorator(config)`)
    - Uso de `factory functions` en decoradores

</details>

<details>
<summary>19. Integración de TypeScript con Angular</summary>

- **Configuración del entorno de Angular con TypeScript**
    - Instalación de Angular CLI y generación de proyectos (`ng new`)
    - Configuración de `tsconfig.json` en Angular
- **Tipado y estructura en Angular**
    - Tipado de componentes, servicios y directivas
    - Uso de interfaces y clases en Angular
    - Manejo de `strictPropertyInitialization` en componentes
- **Inyección de dependencias y servicios**
    - Tipado de `Injectable` y `providers`
    - Uso de `HttpClient` con tipado seguro
    - Uso de `Subject<T>` y `BehaviorSubject<T>` en servicios reactivos
- **Manejo de formularios en Angular con TypeScript**
    - Tipado de `FormGroup`, `FormControl`, `FormArray`
    - Validaciones con `Validators` y `AbstractControl`
- **Optimización del rendimiento en Angular con TypeScript**
    - Uso de `OnPush` y `trackBy` en `ngFor`
    - Evitar `any` en la gestión de estados

</details>

<details>
<summary>20. Integración de TypeScript con NestJS</summary>

- **Configuración y estructura de un proyecto NestJS**
    - Instalación de NestJS y estructura de carpetas (`nest new`)
    - Configuración de `tsconfig.json` en NestJS
- **Tipado en controladores y servicios**
    - Tipado de `@Controller()`, `@Get()`, `@Post()`, `@Put()`
    - Tipado de `@Body()`, `@Param()`, `@Query()` en rutas
    - Uso de DTOs (`Data Transfer Objects`) con validaciones de tipo
- **Inyección de dependencias en NestJS**
    - Uso de `@Injectable()` y `@Inject()` para dependencias tipadas
    - Manejo de `Providers` con interfaces y `useClass`, `useFactory`, `useValue`
- **Gestión de bases de datos con TypeORM y Prisma**
    - Tipado de entidades con `@Entity()`, `@Column()`, `@PrimaryGeneratedColumn()`
    - Uso de `Repository<T>` para acceso tipado a la base de datos
- **Manejo de WebSockets y GraphQL en NestJS con TypeScript**
    - Tipado de `@WebSocketGateway()`, `@SubscribeMessage()`
    - Uso de `@Resolver()`, `@Query()`, `@Mutation()` en GraphQL

</details>

<details>
<summary>21. Interoperabilidad con JavaScript y Migración de Código</summary>

- **Compatibilidad entre TypeScript y JavaScript**
    - Uso de `allowJs` en `tsconfig.json` para mezclar archivos `.js` y `.ts`
    - Beneficios de TypeScript en proyectos JavaScript existentes
- **Migración progresiva de JavaScript a TypeScript**
    - Estrategia de migración incremental (`ts-check` y `@ts-nocheck`)
    - Conversión de archivos `.js` a `.ts` y detección de errores
- **Tipado de librerías JavaScript en TypeScript**
    - Uso de archivos de definición de tipos (`@types/paquete`)
    - Creación manual de `.d.ts` para bibliotecas sin tipado oficial
- **Uso de `declare` para extender JavaScript sin modificar código fuente**
    - Creación de tipos personalizados para bibliotecas externas
    - Declaración de módulos sin tipado con `declare module "paquete"`
- **Conversión de objetos dinámicos y `any` a tipos seguros**
    - Uso de `unknown` en lugar de `any` en estructuras migradas
    - Implementación de validaciones con `typeof`, `instanceof` y `asserts`
- **Buenas prácticas en proyectos híbridos (JS + TS)**
    - Refactorización gradual en grandes proyectos
    - Uso de `strict: true` y eliminación progresiva de `any`

</details>

<details>
<summary>22. Configuración Avanzada de TypeScript (`tsconfig.json`)</summary>

- **Estructura y propósito de `tsconfig.json`**
    - ¿Qué es `tsconfig.json` y cómo afecta la compilación?
    - Generación automática con `tsc --init`
- **Configuraciones esenciales en `compilerOptions`**
    - `target`: Especificación de la versión de ECMAScript
    - `module`: Configuración del sistema de módulos (`ESNext`, `CommonJS`)
    - `strict`: Activación del modo estricto para mayor seguridad
- **Control de directorios y salida de archivos**
    - `rootDir` y `outDir`: Organización de archivos fuente y compilados
    - `include`, `exclude` y `files`: Definición de archivos en la compilación
- **Optimización y rendimiento en la compilación**
    - `incremental`: Compilación incremental para reducir tiempos
    - `noEmitOnError`: Evitar generación de código si hay errores
    - `sourceMap`: Creación de mapas de código fuente para depuración
- **Manejo de archivos de tipado (`@types` y `declaration`)**
    - `declaration`: Generación de archivos `.d.ts` para librerías
    - `typeRoots` y `types`: Control de definición de tipos externos
- **Configuraciones avanzadas en proyectos grandes**
    - `paths` y `baseUrl` para alias de módulos
    - `composite` y `references` para proyectos modulares

</details>

<details>
<summary>23. Automatización y Testing en TypeScript</summary>

### **Automatización en TypeScript**

- **Uso de `npm scripts` para ejecutar tareas**
    - Configuración de scripts en `package.json`
    - Ejecución de compilación y limpieza (`tsc`, `rimraf dist`)
- **Automatización con herramientas de bundling**
    - Configuración de `Webpack` y `Vite` con TypeScript
    - Uso de `esbuild` para compilaciones rápidas
- **Linting y formateo de código**
    - Configuración de `ESLint` con TypeScript (`@typescript-eslint`)
    - Integración con `Prettier` para formateo automático

### **Testing en TypeScript**

- **Testing unitario con Jest y Vitest**
    - Configuración de Jest en TypeScript (`ts-jest`)
    - Creación de pruebas con `describe()`, `test()`, `expect()`
    - Uso de mocks (`jest.mock()`, `jest.fn()`, `spyOn()`)
- **Testing de integración en NestJS y Angular**
    - Pruebas de servicios en NestJS con `TestingModule`
    - Pruebas en Angular con `TestBed` y `ComponentFixture`
- **Pruebas end-to-end (E2E) con Cypress y Playwright**
    - Configuración de Cypress en proyectos TypeScript
    - Creación de pruebas de UI (`cy.visit()`, `cy.get()`, `cy.click()`)
- **Cobertura de código y generación de reportes**
    - Uso de `jest --coverage` para métricas de test
    - Configuración de `nyc` para análisis de cobertura

</details>