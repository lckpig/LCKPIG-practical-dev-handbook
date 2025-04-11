<!-- MULTILANGUAGE MENU START -->
[ES](https://lckpig.gitbook.io/practical-dev-handbook/typescript) | EN
<!-- MULTILANGUAGE MENU END -->

* [Types in TypeScript](types.md)

<details>
<summary>1. Introduction to TypeScript</summary>

- **History and evolution of TypeScript**
    - Creation by Microsoft and motivations behind TypeScript
    - Key differences between TypeScript and JavaScript
    - Notable versions and improvements introduced in each
- **Advantages and main features of TypeScript**
    - Static typing and early error detection
    - Compatibility with JavaScript and transpilation to ES5/ES6+
    - Support for object-oriented programming and generics
    - Integration with code editors and development tools
- **How TypeScript works internally**
    - Transpilation process (`tsc`)
    - Conversion of TypeScript code to standard JavaScript
    - Type definition files (`.d.ts`)
- **Key differences between TypeScript and JavaScript**
    - Static typing vs. dynamic typing
    - Interfaces and type aliases
    - Compatibility with modules and namespaces

</details>

<details>
<summary>2. Installation and Environment Setup</summary>

- **TypeScript Installation**
    - Global installation with `npm install -g typescript`
    - Project installation with `npm install --save-dev typescript`
    - Installation verification with `tsc --version`
- **Basic compiler configuration (`tsconfig.json`)**
    - Generation of `tsconfig.json` with `tsc --init`
    - Essential parameters (`target`, `module`, `strict`, `outDir`, `rootDir`)
    - Incremental compilation with `incremental: true`
- **Running TypeScript code**
    - Manual compilation with `tsc file.ts`
    - Automatic compilation with `tsc --watch`
    - Using `ts-node` to run TypeScript without compilation (`npx ts-node file.ts`)
- **Configuration in editors and development tools**
    - VS Code configuration with TypeScript support
    - Integration with ESLint and Prettier for code formatting
    - Recommended extensions in Visual Studio Code

</details>

<details>
<summary>3. Basic Types in TypeScript</summary>

- **Primitive types in TypeScript**
    - `string`, `number`, `boolean`, `null`, `undefined`
    - Differences between `null` and `undefined`
    - Using `bigint` for operations with large numbers
- **Typing in variables and constants**
    - Declaration with `let`, `const` and their relationship with types
    - Type inference vs. explicit annotations
- **The `any` type and its impact on code**
    - When to use `any` and its risks
    - Safe alternatives with `unknown`
- **The `void` type and its use in functions**
    - Differences between `void` and `undefined` in returns
    - Use in functions without explicit return
- **The `never` type for functions that don't return values**
    - Functions that throw errors (`throw`)
    - Functions that never end (`while (true) {}`)
- **Arrays and Tuples in TypeScript**
    - Array declaration (`number[]`, `Array<string>`)
    - Using tuples (`[string, number]`)
    - Labeled tuples (`[id: number, name: string]`)

</details>

<details>
<summary>4. Type Inference and Annotations</summary>

- **Type inference in TypeScript**
    - Automatic inference in variables (`let x = 10; // x is number`)
    - Inference in functions (`function sum(a, b) { return a + b; }`)
    - Contextual inference based on value usage
- **Type annotations in variables and functions**
    - Manual type specification (`let name: string = "TypeScript";`)
    - Annotations in function parameters (`function greet(name: string) {}`)
    - Explicit function returns (`function add(a: number, b: number): number {}`)
- **Using `unknown` as a safe alternative to `any`**
    - Differences between `unknown` and `any`
    - `unknown` restrictions to prevent typing errors
- **Function typing and function expressions**
    - Function declaration with input and output types
    - Using `type` and `interface` to define reusable functions
- **Type Assertions (`as` and `<Type>`)**
    - Type conversion at compile time
    - When to use `as` and `<Type>` and their differences
    - Risks and best practices in `Type Assertions`

</details>

<details>
<summary>5. Advanced Types in TypeScript</summary>

- **Union Types**
    - Using `|` to allow multiple types (`let value: string | number;`)
    - Validations in functions with union types
- **Intersection Types**
    - Combining multiple types with `&`
    - Use cases in complex data structures
- **The `unknown` type vs `any`**
    - Differences and when to use each
    - `unknown` restrictions in operations
- **The `never` type and its application**
    - Functions that never return a value (`throw new Error()`)
    - Use in exhaustive validations
- **Literal Types and Enums**
    - Literal types (`type Color = "red" | "green" | "blue"`)
    - Definition and use of `enum` (`enum State { Active, Inactive }`)
    - Enums with numeric and string values
- **The `typeof` operator in TypeScript**
    - Type inference based on existing values
    - Use in generic functions
- **`keyof`, `typeof` and `in` in TypeScript**
    - Using `keyof` to access object keys
    - `typeof` in combination with `keyof`
    - The `in` operator for property validations

</details>

<details>
<summary>6. Handling Utility Types in TypeScript</summary>

- **Partial and optional types**
    - `Partial<T>`: Converting all properties to optional
    - `Required<T>`: Converting all properties to required
- **Object manipulation with `Pick`, `Omit` and `Record`**
    - `Pick<T, K>`: Selecting specific properties from a type
    - `Omit<T, K>`: Excluding properties from a type
    - `Record<K, T>`: Creating a type with specific keys and values
- **The `Readonly<T>` type and its application**
    - Preventing modifications in objects with `Readonly<T>`
    - Use cases in immutable structures
- **`Extract<T, U>` and `Exclude<T, U>`**
    - `Extract<T, U>`: Extracting only matching types
    - `Exclude<T, U>`: Removing specific types
- **`NonNullable<T>` and `ReturnType<T>`**
    - `NonNullable<T>`: Removing `null` and `undefined` from a type
    - `ReturnType<T>`: Inferring the return type of a function
- **Using `InstanceType<T>` and `ThisParameterType<T>`**
    - `InstanceType<T>`: Inferring the type of a class instance
    - `ThisParameterType<T>`: Extracting the type of `this` in a function

</details>

<details>
<summary>7. Handling Generic Types in TypeScript</summary>

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
    - Implementing generic classes (`class Repository<T>`)
    - Use cases in data models
- **Using `keyof` and `typeof` in generics**
    - Dynamic key access with `keyof`
    - Type inference based on objects with `typeof`
- **Advanced generic manipulation**
    - Conditional types with `extends` (`T extends U ? X : Y`)
    - Automatic inference with `infer` (`ReturnType<T>`)
    - Using `Mapped Types` to transform structures

</details>

<details>
<summary>8. Handling Conditional and Mapped Types</summary>

- **Introduction to conditional types**
    - Basic syntax (`T extends U ? X : Y`)
    - Use cases in dynamic type validations
- **Using `infer` in conditional types**
    - Extracting internal types with `infer` (`ReturnType<T>`)
    - Advanced applications with automatic inference
- **Mapped Types**
    - Transforming object properties
    - Using `as` in `Mapped Types` to change keys
- **Modifying properties with `Readonly<T>`, `Partial<T>` and `Required<T>`**
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
<summary>9. Handling Recursive and Advanced Types</summary>

- **Recursive types in TypeScript**
    - Defining recursive structures (`type Node<T> = { value: T; children?: Node<T>[] };`)
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
    - Implementing type validation at compile time
    - Using `IsNever<T>` and `IsUnknown<T>` for type flow control

</details>

<details>
<summary>10. Modules and Namespaces</summary>

- **Module handling in TypeScript**
    - Differences between `ES Modules` and `CommonJS`
    - Imports and exports (`import { something } from './file'`, `export function something()`)
    - Default exports vs. named exports
- **Code organization with modules**
    - Using `index.ts` to centralize exports
    - Separation of concerns in reusable modules
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