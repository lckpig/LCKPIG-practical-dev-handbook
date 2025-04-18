<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/typescript)
<!-- MULTILANGUAJE MENU END -->

<details>
<summary>1. Introduction to TypeScript</summary>

- [**History and evolution of TypeScript**](introduction/history-evolution.md)
    - Creation by Microsoft and motivations behind TypeScript
    - Key differences between TypeScript and JavaScript
    - Highlighted versions and improvements introduced in each
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
<summary>2. Installation and Environment Setup</summary>

- [**TypeScript Installation**](installation-configuration/installation.md)
    - Global installation with `npm install -g typescript`
    - Project installation with `npm install --save-dev typescript`
    - Verify installation with `tsc --version`
- [**Basic Compiler Configuration (`tsconfig.json`)**](installation-configuration/compiler-config.md)
    - Generate `tsconfig.json` with `tsc --init`
    - Essential parameters (`target`, `module`, `strict`, `outDir`, `rootDir`)
    - Incremental compilation with `incremental: true`
- [**Executing TypeScript Code**](installation-configuration/code-execution.md)
    - Manual compilation with `tsc file.ts`
    - Automatic compilation with `tsc --watch`
    - Using `ts-node` to run TypeScript without compiling (`npx ts-node file.ts`)
- [**Setup in Editors and Development Tools**](installation-configuration/editor-setup.md)
    - VS Code configuration with TypeScript support
    - Integration with ESLint and Prettier for code formatting
    - Recommended extensions in Visual Studio Code

</details>

<details>
<summary>3. Basic Types in TypeScript</summary>

- [**Primitive types in TypeScript**](basic-types/primitive-types.md)
    - `string`, `number`, `boolean`, `null`, `undefined`
    - Differences between `null` and `undefined`
    - Using `bigint` for operations with large numbers
- [**Typing Variables and Constants**](basic-types/variable-typing.md)
    - Declaration with `let`, `const` and their relationship with types
    - Type inference vs. explicit annotations
- [**The `any` Type and its Impact on Code**](basic-types/any-type.md)
    - When to use `any` and its risks
    - Safe alternatives with `unknown`
- [**The `void` Type and its Use in Functions**](basic-types/void-type.md)
    - Differences between `void` and `undefined` in returns
    - Use in functions without explicit return
- [**The `never` Type for Functions That Don't Return Values**](basic-types/never-type.md)
    - Functions that throw errors (`throw`)
    - Functions that never terminate (`while (true) {}`)
- [**Arrays and Tuples in TypeScript**](basic-types/arrays-tuples.md)
    - Array declaration (`number[]`, `Array<string>`)
    - Using tuples (`[string, number]`)
    - Labeled tuples (`[id: number, name: string]`)

</details>

<details>
<summary>4. Type Inference and Annotations</summary>

- [**Type inference in TypeScript**](type-inference-annotations/type-inference.md)
    - Automatic inference in variables (`let x = 10; // x is number`)
    - Inference in functions (`function sum(a, b) { return a + b; }`)
    - Contextual inference based on value usage
- [**Type annotations in variables and functions**](type-inference-annotations/type-annotations.md)
    - Manual type specification (`let name: string = "TypeScript";`)
    - Annotations in function parameters (`function greet(name: string) {}`)
    - Explicit function returns (`function add(a: number, b: number): number {}`)
- [**Using `unknown` as a safe alternative to `any`**](type-inference-annotations/unknown-vs-any.md)
    - Differences between `unknown` and `any`
    - `unknown` restrictions to prevent typing errors
- [**Typing functions and function expressions**](type-inference-annotations/function-typing.md)
    - Function declaration with input and output types
    - Using `type` and `interface` to define reusable functions
- [**Type Assertions (`as` and `<Type>`)**](type-inference-annotations/type-assertions.md)
    - Type conversion at compile time
    - When to use `as` and `<Type>` and their differences
    - Risks and best practices in `Type Assertions`

</details>

<details>
<summary>5. Advanced Types in TypeScript</summary>

- [**Union Types**](advanced-types/union-types.md)
    - Using `|` to allow multiple types (`let value: string | number;`)
    - Validations in functions with union types
- [**Intersection Types**](advanced-types/intersection-types.md)
    - Combining multiple types with `&`
    - Use cases in complex data structures
- [**The `unknown` type vs `any` (Advanced)**](advanced-types/unknown-vs-any-advanced.md)
    - Differences and when to use each one
    - `unknown` restrictions in operations
- [**The `never` type and its application (Advanced)**](advanced-types/never-type-advanced.md)
    - Functions that never return a value (`throw new Error()`)
    - Use in exhaustive validations
- [**Literal Types and Enums**](advanced-types/literal-enums.md)
    - Literal types (`type Color = "red" | "green" | "blue"`)
    - Definition and use of `enum` (`enum Status { Active, Inactive }`)
    - Enums with numeric and string values
- [**The `typeof` operator in TypeScript**](advanced-types/typeof-operator.md)
    - Type inference based on existing values
    - Use in generic functions
- [**`keyof`, `typeof` and `in` in TypeScript**](advanced-types/keyof-typeof-in.md)
    - Using `keyof` to access an object's keys
    - `typeof` in combination with `keyof`
    - The `in` operator for property validations

</details>

<details>
<summary>6. Utility Types in TypeScript</summary>

- [**Partial and optional types**](utility-types/partial-required.md)
    - `Partial<T>`: Converting all properties to optional
    - `Required<T>`: Converting all properties to required
- [**Object manipulation with `Pick`, `Omit` and `Record`**](utility-types/pick-omit-record.md)
    - `Pick<T, K>`: Selecting specific properties from a type
    - `Omit<T, K>`: Excluding properties from a type
    - `Record<K, T>`: Creating a type with specific keys and values
- [**The `Readonly<T>` type and its application**](utility-types/readonly-type.md)
    - Preventing modifications in objects with `Readonly<T>`
    - Use cases in immutable structures
- [**`Extract<T, U>` and `Exclude<T, U>`**](utility-types/extract-exclude.md)
    - `Extract<T, U>`: Extracting only matching types
    - `Exclude<T, U>`: Removing specific types
- [**`NonNullable<T>` and `ReturnType<T>`**](utility-types/nonnullable-returntype.md)
    - `NonNullable<T>`: Removing `null` and `undefined` from a type
    - `ReturnType<T>`: Inferring a function's return type
- [**Using `InstanceType<T>` and `ThisParameterType<T>`**](utility-types/instancetype-thisparametertype.md)
    - `InstanceType<T>`: Inferring the type of a class instance
    - `ThisParameterType<T>`: Extracting the `this` type in a function

</details>

<details>
<summary>7. Generic Types in TypeScript</summary>

- **Introduction to generic types**
    - Definition of generic functions (`function identity<T>(value: T): T { return value; }`)
    - Benefits of generic types in code reuse
- **Generics in functions and methods**
    - Using `<T>` in function parameters
    - Applying constraints (`extends`) in generics
- **Generics in interfaces and custom types**
    - Creating generic interfaces (`interface Box<T> { content: T; }`)
    - Types with multiple generic parameters
- **Generics in classes**
    - Implementation of generic classes (`class Repository<T>`)
    - Use cases in data models
- **Using `keyof` and `typeof` in generics**
    - Dynamically accessing keys with `keyof`
    - Type inference based on objects with `typeof`
- **Advanced generic manipulation**
    - Conditional types with `extends` (`T extends U ? X : Y`)
    - Automatic inference with `infer` (`ReturnType<T>`)
    - Using `Mapped Types` to transform structures

</details>

<details>
<summary>8. Conditional and Mapped Types</summary>

- **Introduction to conditional types**
    - Basic syntax (`T extends U ? X : Y`)
    - Use cases in dynamic type validations
- **Using `infer` in conditional types**
    - Extracting internal types with `infer` (`ReturnType<T>`)
    - Advanced applications with automatic inference
- **Mapped Types**
    - Transforming object properties
    - Using `as` in `Mapped Types` to change keys
- **Property modification with `Readonly<T>`, `Partial<T>` and `Required<T>`**
    - Creating derived types from existing structures
    - Restricting and expanding properties
- **Using `Record<K, T>` in creating dynamic structures**
    - Creating typed objects with specific keys and values
    - Use cases in configuration structures
- **Advanced examples of conditional types**
    - Implementing filters and transformations at compile time
    - Creating `DeepPartial<T>` to make nested types optional

</details>

<details>
<summary>9. Recursive and Advanced Types</summary>

- **Recursive types in TypeScript**
    - Definition of recursive structures (`type Node<T> = { value: T; children?: Node<T>[] };`)
    - Use in data structures like trees and nested lists
- **`DeepPartial<T>` and `DeepReadonly<T>`**
    - Transforming nested structures to optional (`DeepPartial<T>`)
    - Applying immutability at deep levels with `DeepReadonly<T>`
- **Advanced tuple and array manipulation**
    - Using `T[number]` to extract values from typed arrays
    - Concatenation and manipulation of tuples (`[...T, U]`)
    - Creating dynamic tuples with `Extract<T, U>`
- **Advanced inference with `infer` and `keyof`**
    - Using `infer` in type destructuring
    - Creating custom utilities with `keyof` and `Mapped Types`
- **Practical examples of advanced types**
    - Implementing type validations at compile time
    - Using `IsNever<T>` and `IsUnknown<T>` for type flow control

</details>

<details>
<summary>10. Modules and Namespaces</summary>

- **Managing modules in TypeScript**
    - Differences between `ES Modules` and `CommonJS`
    - Imports and exports (`import { something } from './file'`, `export function something()`)
    - Default exports vs. named exports
- **Code organization with modules**
    - Using `index.ts` to centralize exports
    - Separation of responsibilities in reusable modules
- **Namespaces in TypeScript**
    - Defining a `namespace` (`namespace MySpace { export class MyClass {} }`)
    - Importing elements from a `namespace` (`MySpace.MyClass`)
    - Differences between `namespace` and `module` in modern TypeScript
- **Module configuration in `tsconfig.json`**
    - Parameters `module`, `moduleResolution`, `baseUrl`, `paths`
    - Module aliases with `paths` and `baseUrl`
- **Using modules with bundlers and frameworks**
    - Configuration in Webpack, Rollup and Vite
    - Integration with Node.js and `ts-node`

</details>

<details>
<summary>11. Object-Oriented Programming in TypeScript</summary>

- **Classes in TypeScript**
    - Class declaration (`class Person {}`)
    - Public, private, and protected properties and methods
    - Constructors and constructor overloading
- **Inheritance and superclasses**
    - Using `extends` to inherit from another class
    - Calling the parent constructor with `super()`
- **Interfaces and abstract classes**
    - Differences between `interface` and `abstract class`
    - Implementing interfaces in classes with `implements`
- **Access modifiers and encapsulation**
    - `public`, `private`, `protected`, `readonly`
    - `get` and `set` methods for property access control
- **Static methods and properties**
    - Declaration with `static`
    - Accessing methods without instantiating the class
- **Design patterns applied in TypeScript**
    - Using `Singleton`, `Factory`, `Decorator`
    - Implementation of `Strategy` and `Observer` in TypeScript

</details>

<details>
<summary>12. Functional Programming with TypeScript</summary>

- **Principles of functional programming in TypeScript**
    - Immutability and pure functions
    - Avoiding side effects in functions
- **Higher-order functions and callbacks**
    - Passing functions as arguments (`map()`, `filter()`, `reduce()`)
    - Creating higher-order functions
- **Closures and currying in TypeScript**
    - Using closures to encapsulate data
    - Implementing currying to partially apply functions
- **Using generic types in functional functions**
    - Creating generic functions (`function process<T>(value: T): T {}`)
    - Applications of `Partial<T>`, `Readonly<T>`, `Pick<T, K>` in functional programming
- **Function composition and `pipe`**
    - Chaining functions with composition (`f(g(x))`)
    - Implementing the `pipe()` pattern
- **Using `ReadonlyArray<T>` and `ReadonlyMap<K, V>`**
    - Preventing mutations in lists and data structures

</details>

<details>
<summary>13. TypeScript with Asynchronous JavaScript</summary>

- **Managing Promises in TypeScript**
    - Typing promises (`Promise<T>`)
    - Returning typed promises in functions
- **Using `async/await` in TypeScript**
    - Declaring asynchronous functions with `async`
    - Waiting for promises with `await`
- **Typing asynchronous functions**
    - Explicit typing of `async` functions (`async function getData(): Promise<string>`)
    - Error typing in `try...catch`
- **`Promise.all()`, `Promise.race()`, `Promise.allSettled()`**
    - Advanced typing and usage in concurrency
- **AbortController and Promise cancellation**
    - Implementation of `AbortController` in `fetch`
    - Using `signal` to cancel HTTP requests
- **Error handling in asynchronous code**
    - Using `catch` in Promises
    - Strategies with `try...catch` in `async` functions

</details>

<details>
<summary>14. DOM Manipulation with TypeScript</summary>

- **Accessing DOM elements with TypeScript**
    - Typing `document.getElementById()`, `querySelector()` and `querySelectorAll()`
    - Using `HTMLElement`, `HTMLInputElement`, `HTMLButtonElement` and other specific types
- **Modifying elements in the DOM**
    - Changing content with `textContent` and `innerHTML`
    - Manipulating attributes with `setAttribute()` and `getAttribute()`
- **Events in TypeScript**
    - Typing events (`MouseEvent`, `KeyboardEvent`, `Event`)
    - Handling `addEventListener()` with specific types
- **Creating and removing elements**
    - `document.createElement()`, `appendChild()`, `removeChild()`
    - Using `insertAdjacentHTML()` to insert dynamic content
- **Event delegation and typed `event.target`**
    - Implementing event delegation in dynamic lists
    - Safe use of `event.target` with `as HTMLElement`
- **Using `MutationObserver` to detect changes in the DOM**
    - Implementation of `MutationObserver`
    - Use cases in dynamic applications

</details>

<details>
<summary>15. Strict Typing and Security Strategies</summary>

- **Activating strict mode in TypeScript**
    - Configuring `strict: true` in `tsconfig.json`
    - Effects of `strictNullChecks`, `noImplicitAny`, `strictFunctionTypes`
- **Safe handling of null and optional values**
    - Using `strictNullChecks` to avoid `null` or `undefined` values
    - Optional chaining operator (`?.`)
    - Nullish coalescing operator (`??`)
- **Using `unknown` instead of `any`**
    - Differences and best practices with `unknown`
    - Usage restrictions and validation needs
- **Security in data and API handling**
    - Input validation with `typeof` and `instanceof`
    - Using `never` to ensure exhaustiveness in `switch`
- **Protection against errors in objects and classes**
    - Implementation of `Readonly<T>` to prevent mutations
    - Safe typing with `Partial<T>` and `Required<T>`
- **Avoiding problems in typing dynamic structures**
    - Strategies for handling JSON structures in APIs (`Record<string, unknown>`)
    - Strict typing of `fetch()` responses

</details>

<details>
<summary>16. Error Handling and Debugging</summary>

- **Error handling with `try...catch` in TypeScript**
    - Typing errors in `catch` blocks (`error: unknown`)
    - Using `instanceof` to verify error type
- **Errors in asynchronous code**
    - Capturing errors in `async/await` with `try...catch`
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
    - Proper use of `interfaces` and `types`
- **Writing maintainable code**
    - Naming conventions for variables and functions
    - Using `readonly` and `const` to prevent accidental modifications
- **Performance optimization in TypeScript**
    - Avoiding unnecessary type conversions (`as any`)
    - Efficient use of data structures (`Map`, `Set`, `Record<K, T>`)
- **Reducing complexity in functions and classes**
    - Applying the **DRY** principle (Don't Repeat Yourself)
    - Using pure functions and modularization
- **Preventing errors at compile time**
    - Enabling `strict` in `tsconfig.json`
    - Using `unknown` instead of `any`
- **Compatibility and scalability in large projects**
    - Using `namespace` vs. `modules`
    - Implementing `Abstract Classes` for easier extensibility

</details>

<details>
<summary>18. Decorators in TypeScript</summary>

- **Introduction to decorators**
    - What are decorators and how do they work in TypeScript?
    - Configuring `experimentalDecorators` in `tsconfig.json`
- **Types of decorators in TypeScript**
    - **Class decorators** (`@ClassDecorator`)
    - **Property decorators** (`@PropertyDecorator`)
    - **Method decorators** (`@MethodDecorator`)
    - **Parameter decorators** (`@ParameterDecorator`)
- **Using decorators in Angular**
    - `@Component()`, `@Injectable()`, `@Directive()`, `@Pipe()`
    - Customizing decorators in services and modules
- **Using decorators in NestJS**
    - `@Controller()`, `@Get()`, `@Post()`, `@Param()`, `@Body()`
    - Creating custom decorators with `Reflect.metadata()`
- **Decorator composition and chaining**
    - Applying multiple decorators to the same entity
    - Execution order of decorators in classes
- **Decorators with parameters and dynamic configuration**
    - Decorators that accept arguments (`@MyDecorator(config)`)
    - Using `factory functions` in decorators

</details>

<details>
<summary>19. TypeScript Integration with Angular</summary>

- **Configuring Angular environment with TypeScript**
    - Installing Angular CLI and generating projects (`ng new`)
    - Configuring `tsconfig.json` in Angular
- **Typing and structure in Angular**
    - Typing components, services, and directives
    - Using interfaces and classes in Angular
    - Handling `strictPropertyInitialization` in components
- **Dependency injection and services**
    - Typing `Injectable` and `providers`
    - Using `HttpClient` with safe typing
    - Using `Subject<T>` and `BehaviorSubject<T>` in reactive services
- **Form handling in Angular with TypeScript**
    - Typing `FormGroup`, `FormControl`, `FormArray`
    - Validations with `Validators` and `AbstractControl`
- **Performance optimization in Angular with TypeScript**
    - Using `OnPush` and `trackBy` in `ngFor`
    - Avoiding `any` in state management

</details>

<details>
<summary>20. TypeScript Integration with NestJS</summary>

- **Configuration and structure of a NestJS project**
    - Installing NestJS and folder structure (`nest new`)
    - Configuring `tsconfig.json` in NestJS
- **Typing in controllers and services**
    - Typing `@Controller()`, `@Get()`, `@Post()`, `@Put()`
    - Typing `@Body()`, `@Param()`, `@Query()` in routes
    - Using DTOs (`Data Transfer Objects`) with type validations
- **Dependency injection in NestJS**
    - Using `@Injectable()` and `@Inject()` for typed dependencies
    - Managing `Providers` with interfaces and `useClass`, `useFactory`, `useValue`
- **Database management with TypeORM and Prisma**
    - Typing entities with `@Entity()`, `@Column()`, `@PrimaryGeneratedColumn()`
    - Using `Repository<T>` for typed database access
- **WebSocket and GraphQL handling in NestJS with TypeScript**
    - Typing `@WebSocketGateway()`, `@SubscribeMessage()`
    - Using `@Resolver()`, `@Query()`, `@Mutation()` in GraphQL

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
