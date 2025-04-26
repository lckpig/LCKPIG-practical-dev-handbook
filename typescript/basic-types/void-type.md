<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/typescript/basic-types/void-type)
<!-- MULTILANGUAJE MENU END -->

<!--
# The `void` type and its use in functions
- Differences between `void` and `undefined` in returns
- Use in functions without explicit return
-->

<details id="toc-container">
<summary>Table of Contents</summary>

<!-- no toc -->
- [Differences between `void` and `undefined` in returns](#differences-between-void-and-undefined-in-returns)
  - [Definition and Purpose](#definition-and-purpose)
  - [Comparison Table: `void` vs `undefined` in Returns](#comparison-table-void-vs-undefined-in-returns)
  - [Typing Implications](#typing-implications)
  - [Important Considerations](#important-considerations)
  - [Best Practices](#best-practices)
  - [Bad Practices](#bad-practices)
- [Use in functions without explicit return](#use-in-functions-without-explicit-return)
  - [Type Inference for `void`](#type-inference-for-void)
  - [Common Use Cases](#common-use-cases)
  - [`async` and `Promise<void>`](#async-and-promisevoid)
  - [Important Considerations](#important-considerations-1)
  - [Best Practices](#best-practices-1)
  - [Bad Practices](#bad-practices-1)

</details>

# The `void` type and its use in functions

In TypeScript, the `void` type represents the **intentional absence of a return value** in a function. It is an explicit and semantic way to indicate that a function performs an action or side effect (such as modifying state, printing to the console, calling another API, etc.), but **is not designed to return a useful result** that the calling code should process or use. Understanding `void` is fundamental for writing clear, maintainable TypeScript code aligned with the language's philosophy, especially when differentiating it from `undefined`.

---

## Differences between `void` and `undefined` in returns

Although `void` and `undefined` may seem similar at first glance, and indeed a `void` function in underlying JavaScript returns `undefined`, in the context of TypeScript they have distinct purposes and communicate fundamentally different intentions. The choice between them affects code clarity and robustness.

### Definition and Purpose

-   **`void`**:
    -   Is used **exclusively** as the return type for functions that *intentionally* do not return any significant value.
    -   Its purpose is **semantic clarity**: it explicitly signals that the function executes for its side effects and that any value it might return (implicitly `undefined` in JS) **should not be used**.
    -   It is part of TypeScript's type system to improve expressiveness and safety.

-   **`undefined`**:
    -   Is both a **primitive type** and a **literal value** in JavaScript and TypeScript.
    -   Indicates the absence of an assigned value: an uninitialized variable, a non-existent object property, or the value returned by a function that has no explicit `return` (in JavaScript) or that explicitly returns `undefined`.
    -   Unlike `void` (which is only a return type), `undefined` is a *real* value that can be assigned to variables, compared, and used in expressions (although its direct use often indicates a potential error or unexpected state).


[↑ Back to Top](#toc-container)


### Comparison Table: `void` vs `undefined` in Returns

The following table summarizes the key differences between using `void` and `undefined` as a function return type in TypeScript:

| Feature              | `void`                                                               | `undefined`                                                                |
| :------------------- | :------------------------------------------------------------------- | :------------------------------------------------------------------------- |
| **Nature**           | Return type exclusive to functions.                                  | Primitive type and literal value.                                          |
| **Semantic Intent**  | Explicitly indicates the absence of a *useful* return value.         | Indicates the absence of an assigned value; can be an *expected* value.    |
| **Primary Purpose**  | Signal functions executed for their side effects.                    | Represent a missing or uninitialized value; can be a *significant* return. |
| **Main Use**         | Return type (`function fn(): void {}`).                              | Variable type (`let x: number                                              | undefined;`), value (`return undefined;`). |
| **Runtime Value**    | Function returns `undefined` implicitly in JavaScript.               | The literal value `undefined`.                                             |
| **Result Usage**     | **Discouraged**. `void` typing indicates not to use the result.      | **Allowed and expected**. Code must handle the possible `undefined` value. |
| **Clarity**          | High. Clearly states the intention of not returning anything useful. | Lower if used merely to indicate "no return," can be ambiguous.            |
| **JS Compatibility** | Allows `return undefined;` for compatibility (bad practice in TS).   | Standard JS behavior for functions without `return`.                       |
| **Alternatives**     | None (for this specific intention).                                  | `null`, union types (`T                                                    | null`), throwing errors, Result Types.     |


[↑ Back to Top](#toc-container)


### Typing Implications

The main difference lies in the **intention** they communicate to the compiler and other developers:

-   **`function myFuncion(): void { ... }`**: This signature unequivocally declares: "This function performs an action, but don't expect a useful value from it. If you try to use its result, you are probably making a mistake."
    -   Although you could technically `return undefined;` inside a `void` function (for historical compatibility with JavaScript), **doing so is confusing and considered bad practice in TypeScript**. It obscures the intention of `void`.

        {% hint style="warning" %}
        Avoid `return undefined;` in functions typed as `void` in TypeScript. Use simply `return;` for early exits if needed.
        {% endhint %}

    -   TypeScript *allows* assigning the result of a `void` function to a variable (e.g., `const res = myFuncion();`), but the inferred type for `res` will be `void`, and its runtime value will be `undefined`. However, the `void` declaration itself **strongly discourages** this usage. The compiler won't stop you directly, but stricter linting tools or code reviews should flag it as a potential logic error.

-   **`function otherFunction(): T | undefined { ... }`**: This signature indicates that the function *may* return a value of type `T`, but it *may also* return `undefined` as a *significant* and expected result in certain scenarios (e.g., a search function returning the found object or `undefined` if not found). Here, `undefined` is a legitimate value that the calling code must handle explicitly (e.g., with an `if (result !== undefined) { ... }` check).


[↑ Back to Top](#toc-container)


#### Examples

```typescript
// Function explicitly typed as void: Performs action, returns nothing useful.
function showMessage(message: string): void {
  console.log(message);
  // No return statement, or just `return;` for early exit
}

// TypeScript infers void if there's no explicit return that returns a value
function greet() { // Inferred as (): void
  console.log("Hello World");
}

// Function that DOES return a value that MAY be undefined
function findUser(id: number): { name: string } | undefined {
  if (id === 1) {
    return { name: "Alice" };
  }
  // If not found, returns undefined explicitly as a meaningful value
  return undefined;
}
const user = findUser(2);
if (user !== undefined) {
  console.log(user.name);
} else {
  console.log("User not found.");
}

// Function explicitly returning undefined (often less clear than void or T | null)
function returnsUndefinedLiteral(): undefined {
  // Although valid, using `undefined` as an explicit return type
  // is often less common and clear than using `void` for "no return"
  // or `T | null` for "optional value".
  return undefined;
}
const undResult = returnsUndefinedLiteral();
console.log(undResult); // Output: undefined

// Using the result of a void function (discouraged and generally useless)
const voidResult = showMessage("Testing void");
console.log(voidResult); // Output: undefined. Although assigned, `void` indicates we shouldn't use this value.
```


[↑ Back to Top](#toc-container)


### Important Considerations

-   **Code Clarity**: Using `void` significantly improves code readability by explicitly communicating the author's intent: "this function isn't meant to return a consumable value." This avoids ambiguity about whether the absence of a `return` was intentional or an oversight.
-   **Intent vs. Implementation**: In JavaScript, a function without a `return` returns `undefined`. TypeScript respects this at runtime, but the `void` type adds a semantic layer at compile time to guide the developer on how the function *should* be used.
-   **JavaScript Compatibility**: TypeScript's permissiveness in allowing `return undefined;` in `void` functions exists primarily for compatibility with existing JavaScript code that might be migrated to TypeScript. However, in new and idiomatic TypeScript code, this practice should be avoided in favor of `return;` or no `return` statement.
-   **Alternatives to `undefined` as a Return Value**: If a function needs to explicitly indicate the absence of a valid result (rather than simply not having a result), directly returning `undefined` is often less preferable than other more semantic and robust alternatives:
    -   **`null`**: Traditionally used in many languages to indicate "intentional absence of an object" or "not found." `function search(): T | null`.
    -   **Throwing an Error**: If the absence of the value represents an exceptional condition or error. `function getConfig(): Config { throw new Error("..."); }`.
    -   **Specific Union Types**: Returning an object with a status, like `{ found: false } | { found: true, value: T }`.
    -   **Result Types**: A common pattern (often implemented with libraries) that encapsulates success (`Ok<T>`) or failure (`Err<E>`).


[↑ Back to Top](#toc-container)


### Best Practices

{% hint style="success" %}
Explicitly type functions whose primary purpose is side effects and do not intentionally return a value as `void`, even if TypeScript could infer it. This reinforces the code's intent.
{% endhint %}

-   Use `return;` (without a value) for early exits from `void` functions when conditional logic requires it.
-   Enable the `--noImplicitReturns` compiler option in `tsconfig.json`. This helps ensure that all code paths in functions that *are supposed* to return a value (i.e., not typed as `void` or `any`) actually do so, preventing errors due to oversight.
-   When a function *needs* to indicate the absence of a value as a *valid* result, prefer `T | null` over `T | undefined`, as `null` is often semantically clearer for "intentional absence," while `undefined` is often associated with uninitialized states or errors.


[↑ Back to Top](#toc-container)


### Bad Practices

{% hint style="warning" %}
Declaring a function as `void` and then assigning its result to a variable with the intention of using that (`undefined`) value. This directly contradicts the semantics of `void` and likely indicates a misunderstanding or logic error.
{% endhint %}

{% hint style="danger" %}
Using `undefined` as the explicit return type (`function fn(): undefined`) when the true intention is to indicate that the function has no meaningful return value. In this case, `void` should always be used.
{% endhint %}

-   Returning `undefined` explicitly (`return undefined;`) from a function typed as `void`. Although technically allowed, it is confusing and goes against TypeScript conventions.
-   Relying on the implicitly returned `undefined` value from a `void` function in the calling code.


[↑ Back to Top](#toc-container)



---

## Use in functions without explicit return

TypeScript plays an important role in handling functions that lack an explicit `return` statement that returns a value, or that only use `return;` without a value.

### Type Inference for `void`

When you define a function in TypeScript and do not include any `return` statement that returns a value, or if you only use `return;` for an early exit, the TypeScript compiler is smart enough to **automatically infer** that the return type of that function is `void`.

This behavior is a semantic improvement over pure JavaScript. While in JavaScript a function without a `return` simply returns the value `undefined`, the inference of `void` in TypeScript adds a layer of intent: the compiler understands that no useful return value is expected from this function.


[↑ Back to Top](#toc-container)


#### Examples

```typescript
// TypeScript infers the return type as (): void
function processData(data: any[]) {
  console.log("Starting data processing...");
  // Simulate some work with the data
  data.forEach((item, index) => {
    console.log(`Element ${index}:`, item);
  });
  console.log("Processing complete.");
  // No 'return' statement returning a value
}

// The same applies to arrow functions
const logEvent = (event: string, timestamp: Date) => { // Infers (event: string, timestamp: Date) => void
  console.log(`[${timestamp.toISOString()}] Event logged: ${event}`);
  // No 'return'
};

// Even with an empty return, it remains void
function validateInput(input: string | null): void { // Explicit typing (good practice), but would infer void
  if (input === null || input.trim() === '') {
    console.error("Error: Input cannot be empty.");
    // Early exit without returning a value
    return;
  }
  // Continue processing if input is valid
  console.log("Valid input received:", input);
}

// Calling the functions
processData([1, 'test', true]);
logEvent('User login', new Date());
validateInput("Correct data");
validateInput(null); // Triggers the early return
```


[↑ Back to Top](#toc-container)


### Common Use Cases

`void` functions are extremely common and fundamental, primarily used for operations performed for their **side effects**:

-   **Event Handlers**: Functions responding to user interactions (clicks, key presses, form submissions) or system events (timers, network responses). Their job is to update application state, modify the UI, call other functions, etc., not to return a value to the event system.
    ```typescript
    button.addEventListener('click', (event: MouseEvent): void => {
      console.log('Button clicked!');
      updateAnalytics(event);
    });
    ```
-   **Logging and Monitoring**: Functions dedicated to recording information about the application's state or errors (to console, files, external services).
    ```typescript
    function logError(message: string, error?: Error): void {
      console.error("Error occurred:", message, error);
      // sendToMonitoringService(message, error);
    }
    ```
-   **State Mutators**: Functions that directly modify external variables, object properties, or data structures (though often better encapsulated).
    ```typescript
    let counter = 0;
    function incrementCounter(): void {
      counter++;
      updateUICounter(counter);
    }
    ```
-   **DOM Manipulation**: Functions that directly change HTML element attributes, styles, or content.
    ```typescript
    function updateElementText(elementId: string, text: string): void {
      const element = document.getElementById(elementId);
      if (element) {
        element.textContent = text;
      }
    }
    ```
-   **Initialization Routines**: Code that sets up parts of the application or configures libraries.
    ```typescript
    function initializeMap(containerId: string, options: MapOptions): void {
      // Logic to initialize a map library in the specified container...
      console.log(`Map initialized in #${containerId}`);
    }
    ```

[↑ Back to Top](#toc-container)


### `async` and `Promise<void>`

An important nuance arises with `async` functions:

-   An `async` function **always** returns a `Promise`.
-   If an `async` function is declared with a return type of `void` (`async function fn(): Promise<void> { ... }`), or if it doesn't explicitly return a value, it implicitly returns `Promise<undefined>` at runtime.
-   However, TypeScript uses `Promise<void>` to signify that the *resolved value* of the promise is not intended to be used. While the promise itself can be awaited (to ensure the async operation completes), the value obtained upon resolution should be disregarded.

```typescript
// Async function explicitly returning nothing (infers Promise<void>)
async function performAsyncAction(data: any) {
  console.log("Starting async action...");
  await someAsyncTask(data);
  console.log("Async action completed.");
  // No explicit return
}

// Explicitly typed as Promise<void>
async function logDataToServer(logEntry: string): Promise<void> {
  try {
    const response = await fetch('/api/log', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ entry: logEntry })
    });
    if (!response.ok) {
      console.error("Failed to log data to server");
    }
    // We don't care about the response body, just that it finished.
    // No return needed.
  } catch (error) {
    console.error("Network error while logging:", error);
  }
}

async function main() {
  console.log("Before async action");
  const result = await performAsyncAction({ id: 1 }); 
  // 'result' will be 'undefined' at runtime.
  // The Promise<void> type signals we shouldn't rely on this result.
  console.log("Result of void async action:", result); // Output: undefined
  
  await logDataToServer("User logged in");
  console.log("After async action");
}

main();

// Placeholder for async task
async function someAsyncTask(data: any): Promise<void> { 
  return new Promise(resolve => setTimeout(() => { 
      console.log("Async task simulation finished for:", data); 
      resolve(); 
  }, 500)); 
}
```

{% hint style="info" %}
Using `Promise<void>` clearly communicates that an asynchronous operation is performed for its side effects, and while you might `await` it to ensure completion, you should not expect a meaningful resolved value.
{% endhint %}

[↑ Back to Top](#toc-container)


### Important Considerations

-   **Inference vs. Explicitness:** While TypeScript correctly infers `void`, explicitly typing functions intended for side effects as `: void` can improve code clarity and act as self-documentation, reinforcing the function's purpose.
-   **Callback Functions:** When passing callbacks to functions or methods (like `forEach`, event listeners), even if the callback *does* return a value, if the *caller* doesn't use that return value, TypeScript often allows it for flexibility. However, the *intended* type for many callbacks is often `void` (e.g., `(item: T) => void` for `forEach`).
    ```typescript
    const numbers = [1, 2, 3];
    // forEach expects a callback of type (value: number, index: number, array: number[]) => void
    numbers.forEach((num) => {
      console.log(num);
      // return num * 2; // <-- This return value is ignored by forEach.
    });
    ```

[↑ Back to Top](#toc-container)


### Best Practices

*   Explicitly annotate functions with `: void` when their primary purpose is side effects, even if inference works, to clearly state intent.
*   Understand that `async function(): Promise<void>` means the promise resolves with an unusable value (`undefined`), useful for fire-and-forget async operations or when only completion matters.
*   Use `return;` for early exits in `void` functions.

[↑ Back to Top](#toc-container)


### Bad Practices

*   Assigning the result of a `void` function to a variable and attempting to use it.
*   Writing functions with side effects that also return a meaningful value – this can be confusing. If a value needs to be returned, reconsider if `void` is the correct return type.
*   Returning actual values (other than `undefined`) from a function explicitly typed as `void` (TypeScript will usually flag this as an error).
    ```typescript
    function incorrectVoid(): void {
      // return 5; // Error: Type 'number' is not assignable to type 'void'.
    }
    ```

By using `void` correctly, particularly in functions without explicit returns, you leverage TypeScript's type system to write clearer, safer, and more intentional code.

[↑ Back to Top](#toc-container)

