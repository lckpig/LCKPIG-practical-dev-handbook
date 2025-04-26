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
  - [Real and recommended use cases (with extreme caution)](#real-and-recommended-use-cases-with-extreme-caution)
  - [Important considerations and performance](#important-considerations-and-performance)
  - [Best practices (if using `any` is unavoidable)](#best-practices-if-using-any-is-unavoidable)
  - [Bad practices to avoid](#bad-practices-to-avoid)
  - [Common errors and pitfalls](#common-errors-and-pitfalls)
- [Safe alternatives with `unknown`](#safe-alternatives-with-unknown)
  - [Detailed explanation of `unknown`](#detailed-explanation-of-unknown)
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

This example illustrates the fundamental danger of `any`: it hides potential errors that will only manifest during program execution, nullifying TypeScript's main promise of detecting errors at compile time.

[↑ Back to Top](#toc-container)


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
    moveVehicle("scooter"); 
    ```

3.  **Custom Type Guards:** Functions that return a type predicate (`parameter is DesiredType`). They allow creating complex and reusable validation logic.
    ```typescript
    interface User { id: number; name: string; }
    interface Product { sku: string; price: number; }

    // Type guard for User
    function isUser(obj: unknown): obj is User {
      return (
        typeof obj === 'object' && // It's an object
        obj !== null &&            // It's not null
        'id' in obj && typeof obj.id === 'number' && // Has numeric 'id'
        'name' in obj && typeof obj.name === 'string' // Has string 'name'
      );
    }

    function showName(entity: unknown) {
      if (isUser(entity)) {
        console.log("Username:", entity.name.toUpperCase()); // OK, it's a User
      } else {
        console.log("The provided entity is not a valid user.");
      }
    }

    showName({ id: 1, name: "Ana" });
    showName({ sku: "XYZ", price: 99.9 }); 
    showName("Juan");
    ```

4.  **Type Assertions (`as Type`):** This is the least safe way, similar to `any` but localized. You tell the compiler "trust me, I know this value is of type `Type`". It should be used with extreme caution, only when you are absolutely sure of the type and other techniques are not applicable. Abusing them invalidates the safety `unknown` provides.
    ```typescript
    function getLength(data: unknown): number | undefined {
      // DANGEROUS! If 'data' is not an array or string, this will fail at runtime.
      // Only use if there's a very strong external guarantee about the type.
      if (typeof (data as any[] | string).length === 'number') { 
         return (data as any[] | string).length;
      }
      return undefined;
    }

    console.log(getLength("hello")); // 5
    console.log(getLength([1, 2, 3])); // 3
    // console.log(getLength({})); // RUNTIME ERROR! Cannot read properties of undefined (reading 'length') if not handled well
    ```

{% hint style="warning" %}
Type assertions (`as Type`) are a "lie" to the compiler. They disable type checking for that specific operation. Use them as an absolute last resort, preferably encapsulated in safe validation functions (type guards). Abusing them introduces the same insecurity as `any`.
{% endhint %}

[↑ Back to Top](#toc-container)


### Real and recommended use cases for `unknown`

`unknown` is the correct choice whenever you work with data whose type you cannot guarantee at compile time:

1.  **Data from External APIs or Storage:** When receiving JSON from an API, reading from `localStorage`, or interacting with any external data source, the initial type should be `unknown`. Then, it must be validated and transformed into a specific type.
    ```typescript
    interface UserSettings { theme: 'light' | 'dark'; notifications: boolean; }

    // Robust type guard for UserSettings
    function isUserSettings(obj: unknown): obj is UserSettings {
        if (typeof obj !== 'object' || obj === null) return false;
        const config = obj as Record<string, unknown>; // Temporary safe assertion within the guard
        return (
            (config.theme === 'light' || config.theme === 'dark') &&
            typeof config.notifications === 'boolean'
        );
    }

    async function loadSettings(): Promise<UserSettings> {
        const rawData: unknown = await fetch('/api/user-config').then(res => res.json());

        if (isUserSettings(rawData)) {
            // Validation successful, we can return the specific type
            return rawData;
        } else {
            console.error("Invalid configuration data received:", rawData);
            // Return a default configuration or throw an error
            return { theme: 'light', notifications: true }; 
        }
    }
    ```

{% hint style="info" %}
Libraries like `zod` or `io-ts` are excellent for defining schemas and validating `unknown` data declaratively and safely, greatly simplifying this process.
{% endhint %}

2.  **High-Level Generic Functions:** Functions that operate on various data types but need to inspect them safely.
3.  **Mixed-Type Containers (with Caution):** If you need a structure that can hold values of very different types, `unknown[]` or `Record<string, unknown>` are safer than `any[]` or `Record<string, any>`, as they force type verification when extracting an element.
4.  **Safe Refactoring from `any`:** Systematically replacing `any` with `unknown` in a codebase is an excellent step towards improving safety. It will force the addition of necessary type checks that were previously missing.

[↑ Back to Top](#toc-container)


### Important considerations and best practices with `unknown`

-   **Always Prefer `unknown` over `any`:** `unknown` is the default choice when the type is uncertain. `any` should be the absolute exception.
-   **Implement Robust Validations:** The usefulness of `unknown` directly depends on the quality of the type checks (type guards, `typeof`, `instanceof`). Be thorough.
-   **Avoid Type Assertions (`as`) as a Shortcut:** Resist the temptation to use `as Type` to silence `unknown` errors. This defeats the purpose of using `unknown`. Invest time in writing correct type guards.
-   **Combine with Generics:** In functions, generics (`<T>`) can often be used instead of `unknown` if the function needs to operate on a specific but *a priori* unknown type, preserving the original type. `unknown` is better when you truly know nothing about the type or need to handle multiple possibilities explicitly.

Adopting `unknown` instead of `any` represents a fundamental shift towards safer and more explicit TypeScript programming. It forces you to confront type uncertainty in a controlled manner, resulting in more reliable, predictable, and maintainable code in the long run.

[↑ Back to Top](#toc-container)

