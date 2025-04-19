<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/typescript)
<!-- MULTILANGUAJE MENU END -->

<details>
<summary>1. Introduction to TypeScript</summary>

- [**History and evolution of TypeScript**](introduction/history-evolution.md)
    - Creation by Microsoft and motivations behind TypeScript
    - Key differences between TypeScript and JavaScript
    - Notable versions and improvements introduced in each
- [**Advantages and main features of TypeScript**](introduction/advantages-features.md)
    - Static typing and early error detection
    - Compatibility with JavaScript and transpilation to ES5/ES6+
    - Support for object-oriented programming and generics
    - Integration with code editors and development tools
- [**How TypeScript works internally**](introduction/how-it-works.md)
    - Transpilation process (`tsc`)
    - Conversion of TypeScript code to standard JavaScript
    - Type definition files (`.d.ts`)
- [**Key differences between TypeScript and JavaScript**](introduction/key-differences.md)
    - Static typing vs. dynamic typing
    - Interfaces and type aliases
    - Compatibility with modules and namespaces

</details>

<details>
<summary>2. Environment Installation and Configuration</summary>

- [**Installing TypeScript**](installation-configuration/installation.md)
    - Global installation with `npm install -g typescript`
    - Project installation with `npm install --save-dev typescript`
    - Verifying installation with `tsc --version`
- [**Basic compiler configuration (`tsconfig.json`)**](installation-configuration/compiler-config.md)
    - Generating `tsconfig.json` with `tsc --init`
    - Essential parameters (`target`, `module`, `strict`, `outDir`, `rootDir`)
    - Incremental compilation with `incremental: true`
- [**Executing TypeScript code**](installation-configuration/code-execution.md)
    - Manual compilation with `tsc file.ts`
    - Automatic compilation with `tsc --watch`
    - Using `ts-node` to run TypeScript without compiling (`npx ts-node file.ts`)
- [**Configuration in editors and development tools**](installation-configuration/editor-setup.md)
    - Configuration in VS Code with TypeScript support
    - Integration with ESLint and Prettier for code formatting
    - Recommended extensions in Visual Studio Code

</details>

<details>
<summary>3. Basic Types in TypeScript</summary>

- [**Primitive types in TypeScript**](basic-types/primitive-types.md)
    - `string`, `number`, `boolean`, `null`, `undefined`
    - Differences between `null` and `undefined`
    - Using `bigint` for operations with large numbers
- [**Typing in variables and constants**](basic-types/variable-typing.md)
    - Declaration with `let`, `const` and their relation to types
    - Type inference vs. explicit annotations
- [**The `any` type and its impact on code**](basic-types/any-type.md)
    - When to use `any` and its risks
    - Safe alternatives with `unknown`
- [**The `void` type and its use in functions**](basic-types/void-type.md)
    - Differences between `void` and `undefined` in returns
    - Use in functions without explicit return
- [**The `never` type for functions that do not return values**](basic-types/never-type.md)
    - Functions that throw errors (`throw`)
    - Functions that never end (`while (true) {}`)
- [**Arrays and Tuples in TypeScript**](basic-types/arrays-tuples.md)
    - Array declaration (`number[]`, `Array<string>`)
    - Tuple usage (`[string, number]`)
    - Tuples with labels (`[id: number, name: string]`)

</details>

<details>
<summary>4. Type Inference and Annotations</summary>

- [**Type inference in TypeScript**](type-inference-annotations/type-inference.md)
    - Automatic inference in variables (`let x = 10; // x is number`)
    - Inference in functions (`function sum(a, b) { return a + b; }`)
    - Contextual inference based on value usage
- [**Type annotations on variables and functions**](type-inference-annotations/type-annotations.md)
    - Manual type specification (`let name: string = "TypeScript";`)
    - Annotations on function parameters (`function greet(name: string) {}`)
    - Explicit function return (`function add(a: number, b: number): number {}`)
- [**Using `unknown` as a safe alternative to `any`**](type-inference-annotations/unknown-vs-any.md)
    - Differences between `unknown` and `any`
    - Restrictions of `unknown` to prevent typing errors
- [**Typing functions and function expressions**](type-inference-annotations/function-typing.md)
    - Declaration of functions with input and output types
    - Using `type` and `interface` to define reusable functions
- [**Type Assertions (`as` and `<Type>`)**](type-inference-annotations/type-assertions.md)
    - Compile-time type conversion
    - When to use `as` and `<Type>` and their differences
    - Risks and best practices in Type Assertions

</details>

<details>
<summary>5. Advanced Types in TypeScript</summary>

- [**Union Types**](advanced-types/union-types.md)
    - Using `|` to allow multiple types (`let value: string | number;`)
    - Validations in functions with union types
- [**Intersection Types**](advanced-types/intersection-types.md)
    - Combining multiple types with `&`
    - Use cases in complex data structures
- [**`unknown` vs `any` (Advanced)**](advanced-types/unknown-vs-any-advanced.md)
    - Differences and when to use each
    - Restrictions of `unknown` in operations
- [**The `never` type and its application (Advanced)**](advanced-types/never-type-advanced.md)
    - Functions that never return a value (`throw new Error()`)
    - Use in exhaustive checks
- [**Literal Types and Enums**](advanced-types/literal-enums.md)
    - Literal types (`type Color = "red" | "green" | "blue"`)
    - Definition and use of `enum` (`enum Status { Active, Inactive }`)
    - Enums with numeric and string values
- [**The `typeof` operator in TypeScript**](advanced-types/typeof-operator.md)
    - Type inference based on existing values
    - Use in generic functions
- [**`keyof`, `typeof`, and `in` in TypeScript**](advanced-types/keyof-typeof-in.md)
    - Using `keyof` to access object keys
    - `typeof` in combination with `keyof`
    - The `in` operator for property validations

</details>

<details>
<summary>6. Handling Utility Types in TypeScript</summary>

- [**Partial and optional types**](utility-types/partial-required.md)
    - `Partial<T>`: Converting all properties to optional
    - `Required<T>`: Converting all properties to mandatory
- [**Manipulating objects with `Pick`, `Omit`, and `Record`**](utility-types/pick-omit-record.md)
    - `Pick<T, K>`: Selecting specific properties of a type
    - `Omit<T, K>`: Excluding properties from a type
    - `Record<K, T>`: Creating a type with specific keys and values
- [**The `Readonly<T>` type and its application**](utility-types/readonly-type.md)
    - Preventing modifications on objects with `Readonly<T>`
    - Use cases in immutable structures
- [**`Extract<T, U>` and `Exclude<T, U>`**](utility-types/extract-exclude.md)
    - `Extract<T, U>`: Extracting only matching types
    - `Exclude<T, U>`: Removing specific types
- [**`NonNullable<T>` and `ReturnType<T>`**](utility-types/nonnullable-returntype.md)
    - `NonNullable<T>`: Removing `null` and `undefined` from a type
    - `ReturnType<T>`: Inferring the return type of a function
- [**Using `InstanceType<T>` and `ThisParameterType<T>`**](utility-types/instancetype-thisparametertype.md)
    - `InstanceType<T>`: Inferring the type of a class instance
    - `ThisParameterType<T>`: Extracting the type of `this` in a function

</details>

<details>
<summary>7. Handling Generic Types in TypeScript</summary>

- [**Introduction to generic types**](generic-types/introduction.md)
    - Defining generic functions (`function identity<T>(value: T): T { return value; }`)
    - Benefits of generic types in code reuse
- [**Generics in functions and methods**](generic-types/generics-functions-methods.md)
    - Using `<T>` in function parameters
    - Applying constraints (`extends`) on generics
- [**Generics in interfaces and custom types**](generic-types/generics-interfaces-types.md)
    - Creating generic interfaces (`interface Box<T> { content: T; }`)
    - Types with multiple generic parameters
- [**Generics in classes**](generic-types/generics-classes.md)
    - Implementing generic classes (`class Repository<T>`)
    - Use cases in data models
- [**Using `keyof` and `typeof` in generics**](generic-types/keyof-typeof-generics.md)
    - Accessing keys dynamically with `keyof`
    - Type inference based on objects with `typeof`
- [**Advanced manipulation of generics**](generic-types/advanced-manipulation.md)
    - Conditional types with `extends` (`T extends U ? X : Y`)
    - Automatic inference with `infer` (`ReturnType<T>`)
    - Using `Mapped Types` to transform structures

</details>

<details>
<summary>8. Handling Conditional and Mapped Types</summary>

- [**Introduction to conditional types**](conditional-mapped-types/introduction.md)
    - Basic syntax (`T extends U ? X : Y`)
    - Use cases in dynamic type validations
- [**Using `infer` in conditional types**](conditional-mapped-types/using-infer.md)
    - Extracting internal types with `infer` (`ReturnType<T>`)
    - Advanced applications with automatic inference
- [**Mapped Types**](conditional-mapped-types/mapped-types.md)
    - Transforming object properties
    - Using `as` in Mapped Types to change keys
- [**Modifying properties with `Readonly<T>`, `Partial<T>`, and `Required<T>`**](conditional-mapped-types/modifying-properties.md)
    - Creating derived types from existing structures
    - Restricting and expanding properties
- [**Using `Record<K, T>` in creating dynamic structures**](conditional-mapped-types/using-record.md)
    - Creating typed objects with specific keys and values
    - Use cases in configuration structures
- [**Advanced examples of conditional types**](conditional-mapped-types/advanced-examples.md)
    - Implementing compile-time type validations and transformations
    - Creating `DeepPartial<T>` to make nested types optional

</details>

<details>
<summary>9. Handling Recursive and Advanced Types</summary>

- [**Recursive types in TypeScript**](recursive-advanced-types/recursive-types.md)
    - Definition of recursive structures (`type Node<T> = { value: T; children?: Node<T>[] };`)
    - Use in data structures like trees and nested lists
- [**`DeepPartial<T>` and `DeepReadonly<T>`**](recursive-advanced-types/deep-partial-readonly.md)
    - Transforming nested structures to optional (`DeepPartial<T>`)
    - Applying immutability at deep levels with `DeepReadonly<T>`
- [**Advanced manipulation of tuples and arrays**](recursive-advanced-types/advanced-tuples-arrays.md)
    - Using `T[number]` to extract values from typed arrays
    - Concatenation and manipulation of tuples (`[...T, U]`)
    - Creating dynamic tuples with `Extract<T, U>`
- [**Advanced inference with `infer` and `keyof`**](recursive-advanced-types/advanced-inference.md)
    - Using `infer` in type destructuring
    - Creating custom utilities with `keyof` and `Mapped Types`
- [**Practical examples of advanced types**](recursive-advanced-types/practical-examples.md)
    - Implementing compile-time type validations
    - Using `IsNever<T>` and `IsUnknown<T>` for type flow control

</details>

<details>
<summary>10. Modules and Namespaces</summary>

- [**Handling modules in TypeScript**](modules-namespaces/handling-modules.md)
    - Differences between `ES Modules` and `CommonJS`
    - Imports and exports (`import { something } from './file'`, `export function something()`)
    - Default exports vs. named exports
- [**Organizing code with modules**](modules-namespaces/code-organization.md)
    - Using `index.ts` to centralize exports
    - Separation of responsibilities into reusable modules
- [**Namespaces in TypeScript**](modules-namespaces/namespaces.md)
    - Defining a `namespace` (`namespace MyNamespace { export class MyClass {} }`)
    - Importing elements from a `namespace` (`MyNamespace.MyClass`)
    - Differences between `namespace` and `module` in modern TypeScript
- [**Module configuration in `tsconfig.json`**](modules-namespaces/module-config.md)
    - Parameters `module`, `moduleResolution`, `baseUrl`, `paths`
    - Module aliases with `paths` and `baseUrl`
- [**Using modules with bundlers and frameworks**](modules-namespaces/bundlers-frameworks.md)
    - Configuration in Webpack, Rollup, and Vite
    - Integration with Node.js and `ts-node`

</details>

<details>
<summary>11. Object-Oriented Programming in TypeScript</summary>

- [**Classes in TypeScript**](object-oriented-programming/classes.md)
    - Class declaration (`class Person {}`)
    - Public, private, and protected properties and methods
    - Constructors and constructor overloading
- [**Inheritance and superclasses**](object-oriented-programming/inheritance.md)
    - Using `extends` to inherit from another class
    - Calling the parent constructor with `super()`
- [**Interfaces and abstract classes**](object-oriented-programming/interfaces-abstract-classes.md)
    - Differences between `interface` and `abstract class`
    - Implementing interfaces in classes with `implements`
- [**Access modifiers and encapsulation**](object-oriented-programming/access-modifiers.md)
    - `public`, `private`, `protected`, `readonly`
    - `get` and `set` methods for property access control
- [**Static methods and properties**](object-oriented-programming/static-members.md)
    - Declaration with `static`
    - Accessing methods without instantiating the class
- [**Design patterns applied in TypeScript**](object-oriented-programming/design-patterns.md)
    - Using `Singleton`, `Factory`, `Decorator`
    - Implementing `Strategy` and `Observer` in TypeScript

</details>

<details>
<summary>12. Functional Programming with TypeScript</summary>

- [**Principles of functional programming in TypeScript**](functional-programming/functional-programming-principles.md)
    - Immutability and pure functions
    - Avoiding side effects in functions
- [**Higher-order functions and callbacks**](functional-programming/higher-order-functions-callbacks.md)
    - Passing functions as arguments (`map()`, `filter()`, `reduce()`)
    - Creating higher-order functions
- [**Closures and currying in TypeScript**](functional-programming/closures-currying.md)
    - Using closures to encapsulate data
    - Implementing currying to partially apply functions
- [**Using generic types in functional functions**](functional-programming/generics-functional-functions.md)
    - Creating generic functions (`function process<T>(value: T): T {}`)
    - Applications of `Partial<T>`, `Readonly<T>`, `Pick<T, K>` in functional programming
- [**Function composition and `pipe`**](functional-programming/function-composition-pipe.md)
    - Chaining functions with composition (`f(g(x))`)
    - Implementing the `pipe()` pattern
- [**Using `ReadonlyArray<T>` and `ReadonlyMap<K, V>`**](functional-programming/readonly-collections.md)
    - Preventing mutations in lists and data structures

</details>

<details>
<summary>13. TypeScript with Asynchronous JavaScript</summary>

- [**Handling Promises in TypeScript**](async-javascript/handling-promises.md)
    - Typing promises (`Promise<T>`)
    - Returning typed promises in functions
- [**Using `async/await` in TypeScript**](async-javascript/async-await.md)
    - Declaring asynchronous functions with `async`
    - Awaiting promises with `await`
- [**Typing asynchronous functions**](async-javascript/typing-async-functions.md)
    - Explicit typing of `async` functions (`async function fetchData(): Promise<string>`)
    - Typing errors in `try...catch`
- [**`Promise.all()`, `Promise.race()`, `Promise.allSettled()`**](async-javascript/advanced-promises.md)
    - Typing and advanced usage in concurrency
- [**AbortController and Promise cancellation**](async-javascript/abort-controller.md)
    - Implementing `AbortController` in `fetch`
    - Using `signal` to cancel HTTP requests
- [**Error handling in asynchronous code**](async-javascript/error-handling-async.md)
    - Using `catch` in Promises
    - Strategies with `try...catch` in `async` functions

</details>

<details>
<summary>14. DOM Manipulation with TypeScript</summary>

- [**Accessing DOM elements with TypeScript**](dom-manipulation/accessing-dom-elements.md)
    - Typing `document.getElementById()`, `querySelector()`, and `querySelectorAll()`
    - Using `HTMLElement`, `HTMLInputElement`, `HTMLButtonElement`, and other specific types
- [**Modifying elements in the DOM**](dom-manipulation/modifying-dom-elements.md)
    - Changing content with `textContent` and `innerHTML`
    - Manipulating attributes with `setAttribute()` and `getAttribute()`
- [**Events in TypeScript**](dom-manipulation/handling-events.md)
    - Typing events (`MouseEvent`, `KeyboardEvent`, `Event`)
    - Handling `addEventListener()` with specific types
- [**Creating and removing elements**](dom-manipulation/creating-removing-elements.md)
    - `document.createElement()`, `appendChild()`, `removeChild()`
    - Using `insertAdjacentHTML()` to insert dynamic content
- [**Event delegation and typed `event.target`**](dom-manipulation/event-delegation.md)
    - Implementing event delegation in dynamic lists
    - Safe usage of `event.target` with `as HTMLElement`
- [**Using `MutationObserver` to detect DOM changes**](dom-manipulation/mutation-observer.md)
    - Implementing `MutationObserver`
    - Use cases in dynamic applications

</details>

<details>
<summary>15. Strict Typing and Security Strategies</summary>

- [**Enabling strict mode in TypeScript**](strict-typing-security/enabling-strict-mode.md)
    - Configuring `strict: true` in `tsconfig.json`
    - Effects of `strictNullChecks`, `noImplicitAny`, `strictFunctionTypes`
- [**Safe handling of null and optional values**](strict-typing-security/handling-null-optional.md)
    - Using `strictNullChecks` to avoid `null` or `undefined` values
    - Optional chaining operator (`?.`)
    - Nullish coalescing operator (`??`)
- [**Using `unknown` instead of `any`**](strict-typing-security/unknown-vs-any-security.md)
    - Differences and best practices with `unknown`
    - Usage restrictions and the need for validation
- [**Security in data handling and APIs**](strict-typing-security/data-api-security.md)
    - Input validation with `typeof` and `instanceof`
    - Using `never` to ensure exhaustiveness in `switch`
- [**Protecting against errors in objects and classes**](strict-typing-security/object-class-protection.md)
    - Implementing `Readonly<T>` to prevent mutations
    - Safe typing with `Partial<T>` and `Required<T>`
- [**Avoiding typing problems in dynamic structures**](strict-typing-security/dynamic-structure-typing.md)
    - Strategies for handling JSON structures in APIs (`Record<string, unknown>`)
    - Strict typing of `fetch()` responses

</details>

<details>
<summary>16. Error Handling and Debugging</summary>

- **Error handling with `try...catch` in TypeScript**
    - Typing errors in `catch` blocks (`error: unknown`)
    - Using `instanceof` to check error type
- **Errors in asynchronous code**
    - Catching errors in `async/await` with `try...catch`
    - Typing failed responses in Promises
- **Debugging with `console.log()` and `console.error()`**
    - Efficient use of `console.table()` to visualize objects
    - `debugger` in browser DevTools
- **Integration with debugging tools**
    - Using `tsc --watch` to detect errors during development
    - Debugging in VS Code with `launch.json`
- **Error handling in classes and functions**
    - Creating custom error classes (`class CustomError extends Error`)
    - Controlled error throwing with `throw`
- **Error prevention in TypeScript**
    - Using `strictNullChecks` and `noImplicitAny`
    - Strategies to avoid `any` and ensure safe typing

</details>

<details>
<summary>17. Best Practices and Code Optimization</summary>

- **Code structure and organization**
    - Separating logic into modules and files
    - Proper use of namespaces and modules
- **Naming conventions and style guide**
    - Consistent use of `camelCase`, `PascalCase`
    - Following community style guides (e.g., Airbnb, Google)
- **Writing clean and readable code**
    - Use of comments only when necessary
    - Short functions and single responsibility principle
- **Performance optimization**
    - Minimizing `any` usage
    - Efficient use of utility types
- **TypeScript integration with testing tools**
    - Configuration with Jest, Mocha, Jasmine
    - Writing unit and integration tests with types
- **Code documentation with TSDoc**
    - Using TSDoc comments for functions, classes, and interfaces
    - Generating documentation automatically

</details>

<details>
<summary>18. Real-World Projects and Case Studies</summary>

- **Example 1: Building a REST API with Node.js and TypeScript**
    - Project setup with Express and TypeScript
    - Definition of routes and controllers with types
    - Database integration with TypeORM or Prisma
- **Example 2: Creating a frontend application with Angular/React/Vue and TypeScript**
    - Component structuring with types
    - State management with Redux/NgRx/Vuex and TypeScript
    - Interaction with backend APIs using typed services
- **Example 3: Developing a library/package with TypeScript**
    - Configuration for library publishing
    - Definition of public API with types
    - Testing and documentation generation
- **Case Study: Migration from JavaScript to TypeScript**
    - Incremental migration strategies
    - Challenges and benefits observed
- **Case Study: Using TypeScript in large-scale projects**
    - Team collaboration strategies
    - Impact on code maintainability and scalability

</details>

<details>
<summary>19. Advanced Topics and Future of TypeScript</summary>

- **Decorators in TypeScript**
    - Using decorators for classes, methods, properties, and parameters
    - Custom decorator implementation
    - Use cases in frameworks like Angular and NestJS
- **Advanced inference and type manipulation**
    - Deep dive into conditional types and `infer`
    - Complex mapped types and template literal types
- **Integration with WebAssembly**
    - Using AssemblyScript (a subset of TypeScript) to compile to Wasm
    - Interoperability between TypeScript and WebAssembly
- **TypeScript in Deno**
    - Native TypeScript support in Deno
    - Differences from Node.js environment
- **Future trends and upcoming features**
    - Overview of proposals in TC39 and their potential impact on TypeScript
    - Evolution of the type system and tooling

</details>

<details>
<summary>20. Community and Resources</summary>

- **Official TypeScript documentation and resources**
    - Handbook, tutorials, playground
- **Relevant blogs, tutorials, and online courses**
    - Recommended sites and learning platforms
- **Key community members and influencers**
    - Developers and educators to follow
- **Conferences and events related to TypeScript**
    - Major events in the ecosystem
- **How to contribute to TypeScript and its ecosystem**
    - GitHub repository, contributing guidelines

</details>

<details>
<summary>21. Interoperability with JavaScript and Code Migration</summary>

- **Compatibility between TypeScript and JavaScript**
    - Using `allowJs` in `tsconfig.json` to mix `.js` and `.ts` files
    - Benefits of TypeScript in existing JavaScript projects
- **Progressive migration from JavaScript to TypeScript**
    - Incremental migration strategy (`ts-check` and `@ts-nocheck`)
    - Converting `.js` files to `.ts` and error detection
- **Typing JavaScript libraries in TypeScript**
    - Using type definition files (`@types/package`)
    - Manual creation of `.d.ts` for libraries without official typing
- **Using `declare` to extend JavaScript without modifying source code**
    - Creating custom types for external libraries
    - Declaring untyped modules with `declare module "package"`
- **Converting dynamic objects and `any` to safe types**
    - Using `unknown` instead of `any` in migrated structures
    - Implementing validations with `typeof`, `instanceof` and `asserts`
- **Best practices in hybrid projects (JS + TS)**
    - Gradual refactoring in large projects
    - Using `strict: true` and progressive elimination of `any`

</details>

<details>
<summary>22. Advanced TypeScript Configuration (`tsconfig.json`)</summary>

- **Structure and purpose of `tsconfig.json`**
    - What is `tsconfig.json` and how does it affect compilation?
    - Automatic generation with `tsc --init`
- **Essential configurations in `compilerOptions`**
    - `target`: Specifying ECMAScript version
    - `module`: Configuring the module system (`ESNext`, `CommonJS`)
    - `strict`: Activating strict mode for greater security
- **Directory control and file output**
    - `rootDir` and `outDir`: Organizing source and compiled files
    - `include`, `exclude` and `files`: Defining files in compilation
- **Optimization and performance in compilation**
    - `incremental`: Incremental compilation to reduce times
    - `noEmitOnError`: Preventing code generation if there are errors
    - `sourceMap`: Creating source code maps for debugging
- **Handling type files (`@types` and `declaration`)**
    - `declaration`: Generating `.d.ts` files for libraries
    - `typeRoots` and `types`: Control of external type definitions
- **Advanced configurations in large projects**
    - `paths` and `baseUrl` for module aliases
    - `composite` and `references` for modular projects

</details>

<details>
<summary>23. Automation and Testing in TypeScript</summary>

### **Automation in TypeScript**

- **Using `npm scripts` to run tasks**
    - Configuring scripts in `package.json`
    - Running compilation and cleaning (`tsc`, `rimraf dist`)
- **Automation with bundling tools**
    - Configuring `Webpack` and `Vite` with TypeScript
    - Using `esbuild` for fast compilations
- **Linting and code formatting**
    - Configuring `ESLint` with TypeScript (`@typescript-eslint`)
    - Integration with `Prettier` for automatic formatting

### **Testing in TypeScript**

- **Unit testing with Jest and Vitest**
    - Configuring Jest in TypeScript (`ts-jest`)
    - Creating tests with `describe()`, `test()`, `expect()`
    - Using mocks (`jest.mock()`, `jest.fn()`, `spyOn()`)
- **Integration testing in NestJS and Angular**
    - Testing services in NestJS with `TestingModule`
    - Tests in Angular with `TestBed` and `ComponentFixture`
- **End-to-end (E2E) testing with Cypress and Playwright**
    - Configuring Cypress in TypeScript projects
    - Creating UI tests (`cy.visit()`, `cy.get()`, `cy.click()`)
- **Code coverage and report generation**
    - Using `jest --coverage` for test metrics
    - Configuring `nyc` for coverage analysis

</details> 