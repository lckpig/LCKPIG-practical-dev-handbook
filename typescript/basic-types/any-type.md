<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/typescript/basic-types/any-type)
<!-- MULTILANGUAJE MENU END -->

<!-- 
# The `any` type and its impact on code
- When to use `any` and its risks
- Safe alternatives with `unknown`
-->

<details id="toc-container">
<summary>Table of Contents</summary>

<!-- no toc -->
- [When to use `any` and its risks](#when-to-use-any-and-its-risks)
  - [Definition, Context, and Motivation](#definition-context-and-motivation)
  - [Analogy: The Universal Pass vs. The Mystery Box](#analogy-the-universal-pass-vs-the-mystery-box)
  - [Real and recommended use cases (with extreme caution)](#real-and-recommended-use-cases-with-extreme-caution)
  - [Important considerations and performance](#important-considerations-and-performance)
  - [Best practices (if using `any` is unavoidable)](#best-practices-if-using-any-is-unavoidable)
  - [Bad practices to avoid](#bad-practices-to-avoid)
  - [Common errors and pitfalls](#common-errors-and-pitfalls)
- [Safe alternatives with `unknown`](#safe-alternatives-with-unknown)
  - [Detailed explanation of `unknown`](#detailed-explanation-of-unknown)
  - [Comparison Table: `any` vs `unknown`](#comparison-table-any-vs-unknown)
  - [Safe use of `unknown`: Type Narrowing](#safe-use-of-unknown-type-narrowing)
  - [Real and recommended use cases for `unknown`](#real-and-recommended-use-cases-for-unknown)
  - [Important considerations and best practices with `unknown`](#important-considerations-and-best-practices-with-unknown)

</details>

# The `any` type and its impact on code

The `any` type in TypeScript represents an escape hatch from the static type system. It allows assigning any type of value to a variable, effectively disabling compiler checks for that specific variable. While it might seem like a quick fix for typing issues, its widespread use undermines the fundamental benefits of TypeScript, such as type safety and early error detection. Understanding its purpose, inherent risks, and alternatives is crucial for writing robust, maintainable, and safe TypeScript code.

## When to use `any` and its risks

Using `any` explicitly tells the TypeScript compiler to "disable" its type analysis for a particular variable or expression. This means you can assign any value to it and access any property or method on it without the compiler generating errors, even if those operations are invalid and will cause runtime failures.

### Definition, Context, and Motivation

`any` exists primarily for two reasons:

1.  **Interoperability with JavaScript:** It facilitates the integration of TypeScript code with existing JavaScript libraries that lack type information.
2.  **Gradual Migration:** It allows migrating JavaScript codebases to TypeScript progressively, using `any` as a temporary placeholder for parts of the code not yet typed.

The motivation behind `any` is not to encourage its indiscriminate use, but to provide flexibility in specific scenarios where the strict type system might be a temporary obstacle. However, this flexibility comes at a very high cost in terms of safety and maintainability.

#### Basic Example of Operation

```typescript
// Declare a variable with type any
let flexible: any = 123;

// Access a Number method, works correctly
console.log(flexible.toFixed(2)); // Output: "123.00"

// Reassign a string, allowed by any
flexible = "Hello World";

// Access a String method, works correctly
console.log(flexible.toUpperCase()); // Output: "HELLO WORLD"

// Reassign a boolean
flexible = true;

// Attempt to access a method that doesn't exist on boolean
// The compiler DOES NOT COMPLAIN due to 'any'
// console.log(flexible.toFixed(2)); 
// RUNTIME ERROR! -> TypeError: flexible.toFixed is not a function
```

### Analogy: The Universal Pass vs. The Mystery Box

To understand the fundamental difference between `any` and `unknown`, we can use an analogy:

*   **`any` is like a Universal Pass:** It allows you to enter anywhere and do anything (access any property, call any method) without anyone asking for identification or verifying if you have permission. It's convenient at first but incredibly insecure. You might try to do something you're not authorized for (call a non-existent method), and you'll only realize the problem when it's too late (runtime error).
*   **`unknown` is like a Closed Mystery Box:** You know there's *something* inside, but not what it is. You can receive the box (assign any value to `unknown`), but you cannot use what's inside (access properties or methods) until you open it and verify its contents (perform a type check). Once you've verified what's inside (e.g., confirmed it's a specific tool using `typeof` or `instanceof`), TypeScript allows you to use it safely, knowing what it is. It forces you to be explicit and careful, guaranteeing safety.

This analogy illustrates why `unknown` is the preferred option: it imposes a verification discipline that `any` completely ignores, preventing errors.

### Real and recommended use cases (with extreme caution)

Although the general recommendation is to avoid `any`, there are very specific situations where its use *might* be justified, always as a last resort and with full awareness of the risks.

1.  **Rapid and Exploratory Prototyping:** In very early stages of development, when data structures are uncertain or change constantly, `any` *could* be used temporarily to speed up experimentation. However, it is crucial to replace it with specific types as soon as the structure stabilizes.
2.  **Integration with APIs or Libraries without Types (no @types):** If working with a very old or poorly maintained JavaScript library that lacks type declarations (`.d.ts`) and no community definitions exist in DefinitelyTyped (`@types/library-name`), `any` might be the only viable way to interact with it. Even so, it's preferable to try creating basic definitions yourself or encapsulating the interaction in a specific module to limit the spread of `any`.

#### Example: Interface with External Library without Types

Suppose we use a `legacyAnalytics` library that has no types:

```typescript
// Assume a global 'legacyAnalytics' object is injected
declare const legacyAnalytics: any; 

function trackPageView(url: string, referrer?: string): void {
  try {
    // Call methods of the external library using 'any'
    // The compiler cannot verify if 'track' or 'setReferrer' exist
    // or if the arguments are correct.
    legacyAnalytics.setReferrer(referrer || document.referrer);
    legacyAnalytics.track('pageView', { url: url, timestamp: Date.now() });
    console.log(`Page view tracked for: ${url}`);
  } catch (error) {
    // Handling errors is crucial because 'any' offers no guarantees
    console.error("Error tracking page view with legacy system:", error);
    // Here we could implement a fallback or additional logging
  }
}

// Usage
trackPageView('/home');
trackPageView('/profile', '/home'); 
```

In this case, `any` allows interaction but requires explicit error handling (`try...catch`) because there are no compiler guarantees.

[↑ Back to Top](#toc-container)


### Important considerations and performance

The impact of `any` goes beyond simply disabling checks:

1.  **Total Loss of Type Safety:** This is the most serious consequence. The compiler's ability to reason about types and prevent common errors like typos in property names, calls to non-existent methods, or passing incorrect arguments is lost.
2.  **Hinders Refactoring:** Renaming a property or changing a function signature becomes dangerous. The compiler cannot detect all places where code using `any` might break due to the change.
3.  **Worsens Development Experience:** Intelligent autocompletion is lost, code navigation (go to definition, find references) becomes less reliable, and error detection in the editor (linting) is ineffective.
4.  **"Viral Effect":** The use of `any` tends to spread. If a function accepts or returns `any`, the code using it is often forced to use `any` as well, gradually contaminating the codebase.
5.  **Performance Impact (Indirect):** While `any` itself doesn't usually directly affect the performance of the generated JavaScript code, runtime errors that weren't caught due to `any` can cause crashes, slowdowns, or unexpected behavior in production. Additionally, the lack of clear types can hinder future optimizations.

{% hint style="danger" %}
**Critical Risk:** Abusing `any` is equivalent to giving up the advantages of TypeScript. The code becomes fragile, difficult to maintain, and prone to runtime errors. It should be considered an exceptional tool, not a standard solution.
{% endhint %}

[↑ Back to Top](#toc-container)


### Best practices (if using `any` is unavoidable)

1.  **Minimize Scope:** Use `any` in the most specific place possible. Avoid typing entire objects or function return values as `any` if only a small part is unknown.
2.  **Document Rigorously:** Add comments explaining *why* `any` is being used, what structure is expected (if anything is known), and if there are plans to replace it.
3.  **Validate at Runtime:** If a variable is `any`, add explicit checks (`if (typeof flexible === 'string')`, `if (flexible && flexible.prop)`) before using its properties or methods.
4.  **Consider Intermediate Types:** Sometimes, instead of `any`, you can define a partial interface or type with the known properties and leave the rest more open (although `unknown` is usually better for this).
5.  **Configure Linters:** Tools like ESLint with TypeScript plugins (`@typescript-eslint`) can be configured to warn against or prohibit the explicit use of `any`, helping to maintain discipline.

[↑ Back to Top](#toc-container)


### Bad practices to avoid

1.  **Using `any` out of Laziness:** Resorting to `any` to quickly silence compiler errors without understanding or solving the underlying typing problem.
2.  **Typing Function Parameters and Returns as `any`:** Unless absolutely unavoidable (like in the external library example), this destroys type safety at the function boundaries.
3.  **Assigning `any` to Specific Types without Validation:** `let myNumber: number = myAnyVariable;` This is dangerous and defeats the purpose of static typing.
4.  **Leaving Residual `any`:** Forgetting to replace `any` used during migration or prototyping once the correct types are known.
5.  **Ignoring Alternatives:** Not considering `unknown` or other safer typing techniques before resorting to `any`.

[↑ Back to Top](#toc-container)


### Common errors and pitfalls

-   **False Sense of Security:** Believing that because the code compiles, it's free of type errors, when `any` might be hiding latent issues.
-   **Unconscious Propagation:** An `any` variable can "sneak" into parts of the code that should be type-safe through assignments or function calls, degrading the overall typing quality.
-   **Typo Errors:** `myAny.existingProperty` vs `myAny.exstingProperty` (typo). The compiler won't detect the second case if `myAny` is `any`.
-   **Confusion with `Object` or `{}`:** `any` is much less restrictive than `Object` (which only allows access to `Object.prototype` methods) or `{}` (similar to `Object`). `any` allows *any* operation.

---

[↑ Back to Top](#toc-container)


## Safe alternatives with `unknown`

Introduced in TypeScript 3.0, `unknown` is the safe and preferred alternative to `any`. It represents a value whose type is not known at compile time, but unlike `any`, `unknown` imposes strict restrictions: **you cannot perform any operation on an `unknown` value until you have verified or asserted its specific type.**

### Detailed explanation of `unknown`

`unknown` is the safe "top type" in TypeScript's type hierarchy. This means any value can be assigned to a variable of type `unknown`, but an `unknown` variable cannot be assigned to any other variable with a specific type (except `any` and `unknown` itself) without an explicit type check.

### Comparison Table: `any` vs `unknown`

The following table summarizes the key differences between `any` and `unknown`:

| Feature               | `any`                               | `unknown`                                        |
| :-------------------- | :---------------------------------- | :----------------------------------------------- |
| **Type Safety**       | None (disables checks)              | High (forces type verification)                  |
| **Operations**        | Allows any operation (unsafe)       | No operations allowed without prior verification |
| **Assignment (To)**   | Can be assigned to any type         | Can only be assigned to `any` or `unknown`       |
| **Assignment (From)** | Accepts any type                    | Accepts any type                                 |
| **Recommended Use**   | Migration, JS Interop (last resort) | APIs, external data, uncertain types             |
| **Requirement**       | Flexibility (at the cost of safety) | Safety (requires checks)                         |
| **Alternative to**    | -                                   | `any`                                            |

#### Comparative Example: `any` vs `unknown`

```typescript
let variableAny: any = "I am a string";
let variableUnknown: unknown = "I am also a string";

// With 'any', we can operate directly (unsafe)
console.log(variableAny.toUpperCase()); // OK at compile time, but could fail if it weren't a string
// variableAny = 10;
// console.log(variableAny.toUpperCase()); // RUNTIME ERROR!

// With 'unknown', the compiler forces us to verify the type
// console.log(variableUnknown.toUpperCase()); // Compilation ERROR: Object is of type 'unknown'.

// Mandatory verification to use 'unknown'
if (typeof variableUnknown === 'string') {
  // Inside this block, TypeScript "knows" that variableUnknown is a string
  console.log(variableUnknown.toUpperCase()); // OK
}

// Assignment
let myString: string;

myString = variableAny; // OK: 'any' bypasses checks

// myString = variableUnknown; // Compilation ERROR: Type 'unknown' is not assignable to type 'string'.

// Safe assignment after verification
if (typeof variableUnknown === 'string') {
  myString = variableUnknown; // OK
}
```
The key difference is that `unknown` forces you to write code that handles type uncertainty safely, using type narrowing mechanisms.

[↑ Back to Top](#toc-container)


### Safe use of `unknown`: Type Narrowing

To use an `unknown` value, you must convince TypeScript of its actual type. This is achieved through several techniques:

1.  **`typeof` Checks:** Ideal for primitive types.
    ```typescript
    function processPrimitive(value: unknown) {
      if (typeof value === 'string') {
        console.log("String:", value.trim());
      } else if (typeof value === 'number') {
        console.log("Number:", value.toFixed(2));
      } else if (typeof value === 'boolean') {
        console.log("Boolean:", value ? "True" : "False");
      } else {
        console.log("Non-primitive type or null/undefined:", typeof value);
      }
    }
    processPrimitive("  Hello  ");
    processPrimitive(123.456);
    processPrimitive(true);
    processPrimitive(null); 
    processPrimitive({ a: 1 });
    ```

2.  **`instanceof` Checks:** Useful for verifying if a value is an instance of a specific class.
    ```typescript
    class Car { drive() { console.log("Vroom vroom"); } }
    class Bicycle { pedal() { console.log("Creak creak"); } }

    function moveVehicle(vehicle: unknown) {
      if (vehicle instanceof Car) {
        vehicle.drive(); // OK, TypeScript knows it's a Car
      } else if (vehicle instanceof Bicycle) {
        vehicle.pedal(); // OK, TypeScript knows it's a Bicycle
      } else {
        console.log("I don't know how to move this vehicle.");
      }
    }
    moveVehicle(new Car());
    moveVehicle(new Bicycle());
    moveVehicle({ type: "Skateboard" }); // Logs the else message
    ```

3.  **Custom Type Guards (User-Defined Type Guard Functions):** Functions that return a boolean (`parameterName is Type`) indicating if the argument is of a specific type. Essential for complex object shapes.
    ```typescript
    interface Circle { kind: "circle"; radius: number; }
    interface Square { kind: "square"; sideLength: number; }
    type Shape = Circle | Square;

    // Custom type guard for Circle
    function isCircle(shape: unknown): shape is Circle {
      return typeof shape === 'object' && shape !== null && (shape as Circle).kind === 'circle';
    }

    function getArea(shape: unknown): number {
      if (isCircle(shape)) {
        // TypeScript knows shape is Circle here
        return Math.PI * shape.radius ** 2;
      }
      // You would typically add more guards or error handling
      if (typeof shape === 'object' && shape !== null && (shape as Square).kind === 'square') {
          return (shape as Square).sideLength ** 2;
      }
      throw new Error("Unknown shape");
    }

    let myShape: unknown = { kind: "circle", radius: 5 };
    console.log(getArea(myShape)); // 78.53...

    myShape = { kind: "square", sideLength: 4 };
    console.log(getArea(myShape)); // 16
    ```

4.  **Type Assertions (`as Type`):** This tells the compiler "Trust me, I know this is of type `Type`." Use assertions **with extreme caution** only when you are absolutely certain about the type, as they bypass compiler checks and can lead to runtime errors if you're wrong. Usually, type guards are safer.
    ```typescript
    function processData(data: unknown) {
      // DANGEROUS if you're not 100% sure!
      const user = data as { name: string; id: number };
      console.log(`Processing user: ${user.name}`);
    }
    processData({ name: "Alice", id: 1 });
    // processData({ name: "Bob" }); // RUNTIME ERROR: Cannot read property 'name' of undefined if id missing!
    ```

[↑ Back to Top](#toc-container)


### Real and recommended use cases for `unknown`

`unknown` is the ideal choice in scenarios where type safety is paramount but the exact type isn't known beforehand:

1.  **Parsing External Data:** When receiving data from APIs (JSON), user input, `localStorage`, etc., the structure isn't guaranteed. Parse it into an `unknown` variable first, then validate and narrow the type.
    ```typescript
    async function fetchAndProcessUser() {
      const response = await fetch("/api/user");
      const data: unknown = await response.json(); // Fetch as unknown

      // Validate using a type guard
      if (isValidUser(data)) { // isValidUser is a custom type guard
        displayUser(data); // Now 'data' is known to be User
      } else {
        showError("Invalid user data received.");
      }
    }
    ```
2.  **Working with Mixed-Type Collections:** If you have an array or object that might contain values of truly disparate and unpredictable types.
3.  **Higher-Order Functions/Wrappers:** Functions that wrap other functions or operate on values without needing to know their specific type initially.
4.  **Replacing `any` in Migrations:** When migrating from JavaScript or improving existing TypeScript code, replacing `any` with `unknown` forces you to add the necessary type checks, improving safety incrementally.

[↑ Back to Top](#toc-container)


### Important considerations and best practices with `unknown`

*   **Embrace Type Narrowing:** The core principle of using `unknown` is performing type checks (`typeof`, `instanceof`, custom guards) before operating on the value.
*   **Prefer Type Guards over Assertions:** Custom type guard functions are generally safer and more reusable than type assertions (`as Type`).
*   **Handle the "Unknown" Case:** Ensure your logic includes a path for when the value doesn't match any of the expected types (e.g., throwing an error, returning a default value, logging a warning).
*   **`unknown` is Not a Replacement for Specific Types:** If you *know* the possible types a value can have, use a union type (`string | number`) instead of `unknown`. `unknown` is for when the type is truly indeterminate at that point in the code.

By preferring `unknown` over `any`, you maintain TypeScript's type safety while still handling values whose types aren't immediately known, leading to more robust and reliable applications.

[↑ Back to Top](#toc-container)

