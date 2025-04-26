<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/typescript/basic-types/variable-typing)
<!-- MULTILANGUAJE MENU END -->

<!--
# Typing in variables and constants

- Declaration with `let`, `const` and their relationship with types
- Type inference vs. explicit annotations
-->

<details id="toc-container">
<summary>Table of Contents</summary>

<!-- no toc -->
- [Declaration with `let`, `const` and their Relationship with Types](#declaration-with-let-const-and-their-relationship-with-types)
  - [`let`: Mutable Variables and Wide Types](#let-mutable-variables-and-wide-types)
  - [`const`: Constants and Narrow Literal Types](#const-constants-and-narrow-literal-types)
  - [Important Considerations](#important-considerations)
  - [Real Use Cases](#real-use-cases)
  - [Best Practices](#best-practices)
  - [Bad Practices](#bad-practices)
- [Type Inference vs. Explicit Annotations](#type-inference-vs-explicit-annotations)
  - [Type Inference](#type-inference)
  - [Advantages of Type Inference](#advantages-of-type-inference)
  - [Explicit Annotations](#explicit-annotations)
  - [When to Use Explicit Annotations](#when-to-use-explicit-annotations)
  - [Real Use Cases](#real-use-cases-1)
  - [Best Practices](#best-practices-1)
  - [Bad Practices](#bad-practices-1)
  - [Common Errors](#common-errors)

</details>

# Typing in Variables and Constants

In TypeScript, how we declare variables and constants (`let` vs `const`) and how we specify their types (inference vs explicit annotation) are fundamental decisions that directly impact the robustness, readability, and maintainability of our code. Understanding these options and their implications is crucial for fully leveraging TypeScript's capabilities.

---

## Declaration with `let`, `const` and their Relationship with Types

TypeScript, being a superset of JavaScript, inherits the `let` and `const` keywords for variable declaration. However, it adds a layer of meaning related to the type system. The choice between them not only affects the mutability of the assigned *value* but also how TypeScript *infers* and *treats* the type of that variable.

### `let`: Mutable Variables and Wide Types

When we declare a variable using `let` and initialize it with a primitive value (like a number, string, or boolean) without explicitly specifying its type, TypeScript adopts a conservative stance and infers a "wide" type. It assumes that since the variable *can* change its value in the future (because it was declared with `let`), it could be assigned *any other* value of the same primitive type.

#### Example: Type inference with `let`

```typescript
let visitCounter = 0; // TypeScript infers 'number'
// We can reassign any other number
visitCounter = 10;
visitCounter = visitCounter + 1;

let currentState = "pending"; // TypeScript infers 'string'
// We can reassign any other string
currentState = "processing";
currentState = "completed";

let showDetails = false; // TypeScript infers 'boolean'
// We can reassign true or false
showDetails = true;
```
**Relevance:** This flexibility is useful for variables that naturally need to change their value during program execution, such as counters, accumulators, or evolving status indicators.

[↑ Back to Top](#toc-container)


### `const`: Constants and Narrow Literal Types

In contrast, when we use `const`, we are telling TypeScript that the value assigned to this variable will be *constant* and *never* change after initialization. Leveraging this immutability guarantee, TypeScript infers the most specific type possible: a *literal type*. Instead of just inferring `number`, it will infer the exact number; instead of `string`, the exact string value.

#### Example: Type inference with `const`

```typescript
const MAX_ATTEMPTS = 3; // TypeScript infers the literal type 3
// MAX_ATTEMPTS = 4; // Error: Cannot assign to 'MAX_ATTEMPTS' because it is a constant.

const DEFAULT_ERROR_CODE = "NOT_FOUND"; // TypeScript infers the literal type "NOT_FOUND"
// DEFAULT_ERROR_CODE = "UNAUTHORIZED"; // Error

const DEBUG_MODE = false; // TypeScript infers the literal type false
// DEBUG_MODE = true; // Error
```
**Relevance:** Literal types are extremely useful because they allow creating very specific sets of allowed values, enhancing code safety and expressiveness, especially when combined with union types.

[↑ Back to Top](#toc-container)


### Important Considerations

- **Mutability vs. Typing:** The key difference is mutability. `let` implies potential change and leads to more general types. `const` guarantees assignment immutability and allows for precise literal types.
- **Safety and Predictability:** Using `const` by default increases safety by preventing accidental reassignments. It makes the code more predictable, as you know the value associated with a `const` identifier won't change.
- **`const` with Objects and Arrays:** This is a crucial point and a common source of confusion! `const` applied to an object or array **only makes the reference (the assignment) to that object/array immutable, not its internal content**. You can still modify the object's properties or the array's elements.

#### Example: Mutability of objects and arrays with `const`

```typescript
// The 'configuration' reference is constant
const configuration = { port: 8080, ssl: false };

// This would cause an error:
// configuration = { port: 3000, ssl: true }; // Error: Cannot assign to 'configuration'...

// But this is perfectly valid:
configuration.port = 3001; // Modifying an internal property
configuration.ssl = true;
console.log(configuration); // { port: 3001, ssl: true }

// The 'permissions' reference is constant
const permissions = ["read", "write"];

// This would cause an error:
// permissions = ["admin"]; // Error: Cannot assign to 'permissions'...

// But this is valid:
permissions.push("delete"); // Modifying the array's content
permissions[0] = "read"; // Modifying an existing element
console.log(permissions); // [ 'read', 'write', 'delete' ]
```

{% hint style="warning" %}
**Don't confuse `const` with Deep Immutability:** If you need to ensure that neither an object's properties nor an array's elements can be modified, `const` alone is insufficient. You'll need to use TypeScript's `readonly` types or utilities like `Object.freeze()`.
{% endhint %}


[↑ Back to Top](#toc-container)


### Real Use Cases

- **`let` for Changing State:**
    - Counters in loops: `for (let i = 0; i < 10; i++) { ... }`
    - Accumulators: `let totalSum = 0; items.forEach(item => totalSum += item.value);`
    - State in UI components (React, Vue, Angular): Variables holding whether a modal is open, an input's value, etc. `let isLoading = true; ... isLoading = false;`
    - Temporary variables within functions: `let temporaryResult = complexOperation();`
- **`const` for Fixed Values:**
    - Mathematical or configuration constants: `const SECONDS_IN_MINUTE = 60;`
    - References to DOM elements that don't change: `const submitButton = document.getElementById('submit-btn');`
    - Functions (function expressions assigned to `const` are the norm): `const calculateTax = (amount: number): number => { ... };`
    - Configuration objects that should not be reassigned: `const API_CONFIG = { baseUrl: '/api', timeout: 5000 };`

[↑ Back to Top](#toc-container)


### Best Practices

- **Prioritize `const`:** Make `const` your default choice. Declare everything with `const` initially and only change it to `let` if you realize you need to reassign the variable. This encourages immutability and makes the code easier to reason about.

{% hint style="info" %}
Using `const` by default not only prevents accidental reassignment errors but also clearly signals to other developers (and your future self) that a particular value should not change.
{% endhint %}

- **`let` only when necessary:** Use `let` exclusively for variables whose value *needs* to change explicitly during their lifecycle.

[↑ Back to Top](#toc-container)


### Bad Practices

- **Overusing `let`:** Using `let` for variables that are never reassigned. This introduces noise and suggests unnecessary mutability, making it harder to understand the data flow.
  ```typescript
  // Bad: 'userName' is never reassigned, should be const
  let userName = getName();
  console.log(`Welcome, ${userName}`);
  ```
- **Assuming deep immutability with `const`:** Relying on `const` to prevent internal changes in objects or arrays. This can lead to errors if another part of the code modifies the content unexpectedly.


[↑ Back to Top](#toc-container)


---

## Type Inference vs. Explicit Annotations

One of TypeScript's most powerful features is its ability to *infer* types. However, it also allows us to be *explicit* through annotations when necessary or beneficial.

### Type Inference

In many cases, especially during variable initialization, TypeScript can deduce the type of a variable based on the value assigned to it. This makes the code more concise without sacrificing type safety.

#### Example: Type inference

```typescript
let quantity = 100; // TypeScript correctly infers 'number'
let message = "Processing..."; // TypeScript infers 'string'
let completed = false; // TypeScript infers 'boolean'

// Inference works well with simple objects and arrays
const point = { x: 10, y: 20 }; // Infers { x: number; y: number; }
const names = ["Ana", "Luis", "Elena"]; // Infers string[]

// It also infers the return type of simple functions
function add(a: number, b: number) { // Infers that it returns 'number'
  return a + b;
}
const sumResult = add(5, 3); // Infers 'number' for sumResult
```

[↑ Back to Top](#toc-container)


### Advantages of Type Inference

- **Conciseness:** Reduces the amount of code to write and read. `let name = "X";` is shorter than `let name: string = "X";`.
- **Readability (in simple cases):** For obvious initializations, inference avoids the visual noise of explicit annotations.
- **Maintainability:** If the initial value changes in a way that implies a type change (within reason), TypeScript will often adjust the inference.

[↑ Back to Top](#toc-container)


### Explicit Annotations

There are situations where inference is not possible, not desirable, or where being explicit improves the clarity and intent of the code. We use the colon (`:`) syntax followed by the type.

#### Example: Explicit annotations

```typescript
let userId: number; // Variable declared but not initialized. Annotation necessary.
userId = 12345;
// userId = "id-abc"; // Error: Type 'string' is not assignable to type 'number'.

let apiResponse: string | null; // Union type: can be string or null. Better to be explicit.
apiResponse = await fetchData();
if (apiResponse) { /* ... */ }

let userConfiguration: any; // Danger! Explicit 'any' annotation. Avoid if possible.
userConfiguration = getConfig(); // Disables type checking.

// Annotations on function parameters (ESSENTIAL)
function processOrder(orderId: string, quantity: number): boolean {
  console.log(`Processing order ${orderId} with ${quantity} units.`);
  // ... processing logic ...
  return true; // Return value explicitly stated as boolean
}

// Annotation for more complex or specific types
type Coordinates = { lat: number; lon: number };
let currentLocation: Coordinates;
currentLocation = { lat: 40.7128, lon: -74.0060 };

// Forcing a more specific type than inference
const element = document.getElementById("myButton"); // Infers HTMLElement | null
const myButton: HTMLButtonElement | null = document.getElementById("myButton") as HTMLButtonElement | null; // More specific
// Or using type assertion (carefully):
const myButtonAsserted = document.getElementById("myButton") as HTMLButtonElement;
```

[↑ Back to Top](#toc-container)


### When to Use Explicit Annotations

1.  **Declaration without Initialization:** If a variable is not initialized in its declaration, TypeScript cannot infer its type (often resulting in implicit `any`). Annotation is mandatory for type safety.
2.  **Functions (Parameters and Return):** Function parameters should **always** be annotated. Annotating the return type is a very good practice for clarity and ensuring the function fulfills its contract.
3.  **Complex Types (Union, Intersection, Generics):** When a variable can hold multiple types, or when using more advanced types, explicit annotation is crucial for understanding.
4.  **When Inference is Insufficient or Incorrect:** Sometimes, TypeScript infers a type that is too broad (e.g., `string | number` when we only want `number` initially but allow `string` later) or simply incorrect in complex contexts. Annotation clarifies the intent.
5.  **Object Shapes and API Boundaries:** When defining the shape of objects coming from an external API or forming part of a module's public API, explicit annotations (often via `interface` or `type`) are essential.
6.  **Clarity and Documentation:** In complex algorithms or critical business logic, annotations serve as self-verifying documentation.

#### Example: Clarifying intent with annotations

```typescript
// Scenario: Receiving data from an API that might be an object or null if not found
type UserData = { id: number; name: string };
let currentUser: UserData | null = null; // Inference alone might just be 'null'

async function loadUser(id: number) {
  const response = await fetch(`/api/users/${id}`);
  if (response.ok) {
    currentUser = await response.json(); // Assign the UserData object
  } else {
    currentUser = null; // Explicitly assign null
  }
}
```

[↑ Back to Top](#toc-container)


### Real Use Cases

- **Inference:**
    - Simple local variables with direct initialization: `let i = 0;`, `const msg = "Done";`.
    - Obvious constants: `const VAT = 0.21;`.
    - Simple inferred return types: `function isGreater(a: number, b: number) { return a > b; } // Infers boolean`.
- **Explicit Annotations:**
    - Defining the shape of API data: `interface Product { id: string; name: string; price: number; } let loadedProduct: Product | null;`
    - Parameters and return types of reusable or complex functions.
    - Variables that start as `null` or `undefined` but will later hold a specific type.
    - Using union types to represent different possible states or values: `type LoadingState = "idle" | "loading" | "success" | "error"; let state: LoadingState = "idle";`

[↑ Back to Top](#toc-container)


### Best Practices

- **Trust inference for the simple stuff:** Don't explicitly annotate types that TypeScript can easily and correctly infer (e.g., `let age = 30;`). It keeps the code clean.
- **Be explicit where it matters:** Use annotations for function signatures, complex types, uninitialized variables, and to clarify intent in complex logic.
- **Enable `noImplicitAny`:** This compiler option (`tsconfig.json`) is **fundamental**. It forces explicit annotations for anything TypeScript cannot infer, preventing the dangerous implicit `any` type.

{% hint style="success" %}
Setting `"noImplicitAny": true` in your `tsconfig.json` is one of the best practices for ensuring robust TypeScript usage.
{% endhint %}

- **Avoid explicit `any`:** The `any` type disables type checking. Use it only as a last resort (e.g., integrating with old JS, temporarily unknown complex types). Prefer `unknown` if you need a safe type for unknown values, as `unknown` forces you to check the type before using it.

[↑ Back to Top](#toc-container)


### Bad Practices

- **Redundant annotations:** `let name: string = "Ana";`. TypeScript infers `string` perfectly. This is just noise.
- **Abusing `any`:** Using `any` to quickly "fix" type errors. This defeats the purpose of TypeScript and hides potential bugs.

{% hint style="danger" %}
Every use of `any` should be carefully considered. It represents a "backdoor" in the type system and should be avoided whenever possible. If you need flexibility, consider union types, generics, or `unknown`.
{% endhint %}

- **Not annotating function parameters:** Letting parameters be implicitly `any` is a recipe for runtime errors that TypeScript could have caught at compile time.

[↑ Back to Top](#toc-container)


### Common Errors

- **Confusing `const` with deep immutability:** Already mentioned, but a very common mistake. Remember `const` only protects the variable's assignment.
- **Errors with `this` in functions and `let`/`const`:** While not directly typing-related, how functions are declared (especially methods in classes) can interact with `let`/`const` and the `this` context. Using arrow functions (`=>`) often helps preserve the expected context.
- **Incorrect use of Type Assertions (`as`):** Using `as Type` to force a type when you are not sure the value actually is of that type. Assertions tell the compiler "trust me, I know what I'm doing," but if you're wrong, you'll get runtime errors. It's safer to use type guards or validation.
  ```typescript
  // Dangerous if inputElement isn't actually an HTMLInputElement
  const inputValue = (document.getElementById('myInput') as HTMLInputElement).value;

  // Safer
  const inputElement = document.getElementById('myInput');
  if (inputElement instanceof HTMLInputElement) {
    const safeInputValue = inputElement.value; // Now it's safe to access .value
  }
  ```
- **Inadequate handling of `null`/`undefined` without `strictNullChecks`:** If `strictNullChecks` is disabled, `null` and `undefined` can be assigned to any type, hiding potential errors. Enabling this option and explicitly handling `null`/`undefined` cases (with union types `T | null` and checks) is much safer.

[↑ Back to Top](#toc-container)