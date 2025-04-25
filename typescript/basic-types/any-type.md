<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/typescript/basic-types/any-type)
<!-- MULTILANGUAJE MENU END -->

<!-- 
# The `any` type and its impact on code
- When to use `any` and its risks
- Safe alternatives with `unknown`
-->

<details>
<summary>Table of Contents</summary>

<!-- no toc -->
- [When to use `any` and its risks](#when-to-use-any-and-its-risks)
  - [Definition, Context, and Motivation](#definition-context-and-motivation)
  - [Real and recommended use cases (with extreme caution)](#real-and-recommended-use-cases-with-extreme-caution)
  - [Important considerations and performance](#important-considerations-and-performance)
  - [Best practices (if using `any` is unavoidable)](#best-practices-if-using-any-is-unavoidable)
  - [Bad practices to avoid](#bad-practices-to-avoid)
  - [Common errors and usual pitfalls](#common-errors-and-usual-pitfalls)
- [Safe alternatives with `unknown`](#safe-alternatives-with-unknown)
  - [Detailed explanation of `unknown`](#detailed-explanation-of-unknown)
  - [Safe use of `unknown`: Type Narrowing](#safe-use-of-unknown-type-narrowing)
  - [Real and recommended use cases for `unknown`](#real-and-recommended-use-cases-for-unknown)
  - [Important considerations and best practices with `unknown`](#important-considerations-and-best-practices-with-unknown)

</details>

# The `any` type and its impact on code

The `any` type in TypeScript represents an escape hatch from the static type system. It allows assigning any type of value to a variable, effectively disabling the compiler's checks for that specific variable. While it might seem like a quick fix for typing issues, its widespread use undermines the fundamental benefits of TypeScript, such as type safety and early error detection. Understanding its purpose, inherent risks, and alternatives is crucial for writing robust, maintainable, and safe TypeScript code.

## When to use `any` and its risks

Using `any` explicitly tells the TypeScript compiler to "turn off" its type analysis for a particular variable or expression. This means you can assign any value to it and access any property or method on it without the compiler generating errors, even if those operations are invalid and will cause runtime failures.

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

// Try to access a method that doesn't exist on boolean
// The compiler DOES NOT COMPLAIN due to 'any'
// console.log(flexible.toFixed(2)); 
// RUNTIME ERROR! -> TypeError: flexible.toFixed is not a function
```

This example illustrates the fundamental danger of `any`: it hides potential errors that will only manifest during program execution, negating TypeScript's primary promise of detecting errors at compile time.

### Real and recommended use cases (with extreme caution)

Although the general recommendation is to avoid `any`, there are very specific situations where its use *might* be justified, always as a last resort and with full awareness of the risks.

1.  **Rapid and Exploratory Prototyping:** In very early development stages, when the data structure is uncertain or constantly changing, `any` *could* be used temporarily to speed up experimentation. However, it's crucial to replace it with specific types as soon as the structure stabilizes.
2.  **Integration with APIs or Libraries without Types (without @types):** If working with a very old or poorly maintained JavaScript library that lacks type declarations (`.d.ts`) and community definitions in DefinitelyTyped (`@types/library-name`) don't exist, `any` might be the only viable way to interact with it. Even so, it's preferable to try creating basic definitions yourself or encapsulate the interaction in a specific module to limit the spread of `any`.

#### Example: Interface with External Library without Types

Suppose we use a `legacyAnalytics` library that has no types:

```typescript
// Assume a globally existing 'legacyAnalytics' object is injected
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
    // It's crucial to handle errors because 'any' offers no guarantees
    console.error("Error tracking page view with legacy system:", error);
    // Here we could implement a fallback or additional logging
  }
}

// Usage
trackPageView('/home');
trackPageView('/profile', '/home'); 
```

In this case, `any` allows interaction but requires explicit error handling (`try...catch`) because there are no compiler guarantees.

### Important considerations and performance

The impact of `any` goes beyond simply disabling checks:

1.  **Total Loss of Type Safety:** This is the most serious consequence. The compiler's ability to reason about types and prevent common errors like typos in property names, calls to non-existent methods, or passing incorrect arguments is lost.
2.  **Hinders Refactoring:** Renaming a property or changing a function signature becomes dangerous. The compiler cannot detect all places where code using `any` might break due to the change.
3.  **Worsens Developer Experience:** Smart autocompletion is lost, code navigation (go to definition, find references) becomes less reliable, and error detection in the editor (linting) is ineffective.
4.  **"Viral Effect":** The use of `any` tends to spread. If a function accepts or returns `any`, the code using it is often forced to use `any` as well, gradually contaminating the codebase.
5.  **Impact on Performance (Indirect):** While `any` itself doesn't usually directly affect the performance of the generated JavaScript code, runtime errors undetected due to `any` can cause crashes, slowdowns, or unexpected behavior in production. Furthermore, the lack of clear types can hinder future optimizations.

{% hint style="danger" %}
**Critical Risk:** Abusing `any` is equivalent to giving up the advantages of TypeScript. The code becomes fragile, difficult to maintain, and prone to runtime errors. It should be considered an exceptional tool, not a standard solution.
{% endhint %}

### Best practices (if using `any` is unavoidable)

1.  **Minimize Scope:** Use `any` in the most specific place possible. Avoid typing entire objects or function return values as `any` if only a small part is unknown.
2.  **Document Rigorously:** Add comments explaining *why* `any` is being used, what structure is expected (if anything is known), and if there are plans to replace it.
3.  **Validate at Runtime:** If a variable is `any`, add explicit checks (`if (typeof flexible === 'string')`, `if (flexible && flexible.prop)`) before using its properties or methods.
4.  **Consider Intermediate Types:** Sometimes, instead of `any`, you can define a partial interface or type with known properties and leave the rest more open (though `unknown` is usually better for this).
5.  **Configure Linters:** Tools like ESLint with TypeScript plugins (`@typescript-eslint`) can be configured to warn or prohibit the explicit use of `any`, helping maintain discipline.

### Bad practices to avoid

1.  **Using `any` out of Laziness:** Resorting to `any` to quickly silence compiler errors without understanding or fixing the underlying typing problem.
2.  **Typing Function Parameters and Returns as `any`:** Unless absolutely unavoidable (like in the external library example), this destroys type safety at the function boundaries.
3.  **Assigning `any` to Specific Types without Validation:** `let myNumber: number = myAnyVariable;` This is dangerous and defeats the purpose of static typing.
4.  **Leaving Residual `any`:** Forgetting to replace `any` used during migration or prototyping once the correct types are known.
5.  **Ignoring Alternatives:** Not considering `unknown` or other safer typing techniques before resorting to `any`.

### Common errors and usual pitfalls

-   **False Sense of Security:** Believing that because the code compiles, it's free of type errors, when `any` might be hiding latent problems.
-   **Unconscious Propagation:** An `any` variable can "sneak" into parts of the code that should be type-safe through assignments or function calls, degrading the overall typing quality.
-   **Typo Errors:** `myAny.existingProperty` vs `myAny.exisitingProperty` (typo). The compiler won't catch the second case if `myAny` is `any`.
-   **Confusion with `Object` or `{}`:** `any` is much less restrictive than `Object` (which only allows access to methods from `Object.prototype`) or `{}` (similar to `Object`). `any` allows *any* operation.

---

## Safe alternatives with `unknown`

Introduced in TypeScript 3.0, `unknown` is the safe and preferred alternative to `any`. It represents a value whose type is not known at compile time, but unlike `any`, `unknown` imposes strict restrictions: **you cannot perform any operation on an `unknown` value until you have checked or asserted its specific type.**

### Detailed explanation of `unknown`

`unknown` is the safe "top type" in TypeScript's type hierarchy. This means any value can be assigned to a variable of type `unknown`, but an `unknown` variable cannot be assigned to any other variable with a specific type (except `any` and `unknown` itself) without an explicit type check.

#### Comparative Example: `any` vs `unknown`

```typescript
let variableAny: any = "I am a string";
let variableUnknown: unknown = "I am also a string";

// With 'any', we can operate directly (unsafe)
console.log(variableAny.toUpperCase()); // OK at compile time, but could fail if not a string
// variableAny = 10;
// console.log(variableAny.toUpperCase()); // RUNTIME ERROR!

// With 'unknown', the compiler forces us to check the type
// console.log(variableUnknown.toUpperCase()); // Compilation ERROR: Object is of type 'unknown'.

// Mandatory check to use 'unknown'
if (typeof variableUnknown === 'string') {
  // Inside this block, TypeScript "knows" that variableUnknown is a string
  console.log(variableUnknown.toUpperCase()); // OK
}

// Assignment
let myString: string;

myString = variableAny; // OK: 'any' bypasses checks

// myString = variableUnknown; // Compilation ERROR: Type 'unknown' is not assignable to type 'string'.

// Safe assignment after check
if (typeof variableUnknown === 'string') {
  myString = variableUnknown; // OK
}
```
The key difference is that `unknown` forces you to write code that handles type uncertainty safely, using type narrowing mechanisms.

### Safe use of `unknown`: Type Narrowing

To use an `unknown` value, you must convince TypeScript of its actual type. This is achieved through several techniques:

1.  **Checks with `typeof`:** Ideal for primitive types.
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

2.  **Checks with `instanceof`:** Useful for checking if a value is an instance of a specific class.
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
        obj !== null &&            // Not null
        'id' in obj && typeof obj.id === 'number' && // Has numeric 'id'
        'name' in obj && typeof obj.name === 'string' // Has string 'name'
      );
    }

    function showName(entity: unknown) {
      if (isUser(entity)) {
        // Inside this block, entity is known to be User
        console.log("User Name:", entity.name); 
      } else {
        console.log("The entity is not a user.");
      }
    }
    
    showName({ id: 1, name: "Alice" });
    showName({ sku: "TSHIRT-XL", price: 19.99 });
    showName("Bob");
    ```

4.  **Assertion Functions:** Functions that throw an error if the type condition isn't met. They inform the compiler that the type is guaranteed after the function call.
    ```typescript
    function assertIsString(value: unknown, name = 'value'): asserts value is string {
      if (typeof value !== 'string') {
        throw new Error(`${name} must be a string. Received: ${typeof value}`);
      }
    }

    function processAssertedString(input: unknown) {
      assertIsString(input, 'input');
      // After this line, TypeScript knows 'input' is a string
      console.log(input.toUpperCase()); 
    }

    processAssertedString("hello");
    try {
      processAssertedString(123); 
    } catch(e: any) {
      console.error(e.message); // Output: input must be a string. Received: number
    }
    ```

5.  **Type Assertions (`as Type`):** This is the least safe method, similar to casting in other languages. You tell the compiler "trust me, I know this is `Type`". Use it sparingly, only when you are absolutely certain about the type and other methods aren't feasible, as it bypasses compiler checks.
    ```typescript
    function processWithAssertion(data: unknown) {
        // Use only if you are 100% sure data is a string!
        const upperData = (data as string).toUpperCase();
        console.log(upperData);
    }
    processWithAssertion("i am sure");
    // processWithAssertion({ message: "not a string" }); // RUNTIME ERROR!
    ```

### Real and recommended use cases for `unknown`

`unknown` shines in situations where you receive data whose type cannot be guaranteed at compile time:

1.  **Parsing JSON Data:** `JSON.parse()` returns `any` by default (for historical reasons), but it's much safer to immediately assign it to `unknown` and then validate the structure.
    ```typescript
    const jsonData = '{"name": "Bob", "age": 30}';
    
    let parsedData: unknown;
    try {
        parsedData = JSON.parse(jsonData);
    } catch (error) {
        console.error("Invalid JSON:", error);
        // Handle error, maybe return default data
        return; 
    }

    // Now, validate 'parsedData' before using it
    interface Person { name: string; age: number; }

    function isPerson(obj: unknown): obj is Person {
        return (
            typeof obj === 'object' && obj !== null &&
            'name' in obj && typeof obj.name === 'string' &&
            'age' in obj && typeof obj.age === 'number'
        );
    }

    if (isPerson(parsedData)) {
        console.log(`Parsed Person: ${parsedData.name}, Age: ${parsedData.age}`);
    } else {
        console.error("Parsed data is not a valid Person object.");
    }
    ```

2.  **Data from APIs:** When fetching data from external APIs, the response type isn't guaranteed by the TypeScript compiler. Using `unknown` forces you to validate the response before processing it.
    ```typescript
    async function fetchUserData(userId: number): Promise<unknown> {
        try {
            const response = await fetch(`/api/users/${userId}`);
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            // response.json() technically returns Promise<any>, cast to unknown
            return await response.json() as unknown; 
        } catch (error) {
            console.error("Failed to fetch user data:", error);
            return undefined; // Or handle error differently
        }
    }

    async function displayUser(id: number) {
        const userData = await fetchUserData(id);
        
        // Validate using a type guard before use
        if (isUser(userData)) { 
            console.log(`User found: ${userData.name}`);
        } else {
            console.log(`User data for ID ${id} is invalid or not found.`);
        }
    }

    displayUser(1);
    ```

3.  **User Input:** Data coming directly from user input (e.g., form fields, command-line arguments) is inherently untyped and should be treated as `unknown` until validated.

4.  **Working with `localStorage`/`sessionStorage`:** These APIs store data as strings. When retrieving and parsing, the result should be `unknown`.

### Important considerations and best practices with `unknown`

1.  **Prefer `unknown` over `any`:** Whenever you have a value whose type isn't known at compile time, `unknown` should be your default choice. Resort to `any` only if `unknown` proves genuinely impractical (a rare case).
2.  **Perform Type Checks Immediately:** Don't pass `unknown` values around deep into your application. Check the type as close to the source of the untyped value as possible.
3.  **Use Specific Type Guards:** Create reusable type guard functions for your complex types to keep validation logic clean and DRY (Don't Repeat Yourself).
4.  **Handle the "Else" Case:** When using `if`/`else if` chains for type narrowing, always include a final `else` block to handle cases where the value doesn't match any expected type.
5.  **Avoid Type Assertions (`as Type`) if Possible:** While sometimes necessary, they subvert the type system. Prefer `typeof`, `instanceof`, or custom type guards whenever feasible. If you must use an assertion, be absolutely sure of the type and consider adding runtime checks as a safety net if possible.

By embracing `unknown` and its requirement for explicit type checks, you maintain type safety throughout your codebase, even when dealing with data from untyped sources, leading to more robust and reliable applications.
