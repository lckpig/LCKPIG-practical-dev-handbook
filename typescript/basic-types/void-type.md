<!-- MULTILANGUAJE MENU START -->
[ES](https://lckpig.gitbook.io/practical-dev-handbook/typescript/basic-types/void-type) | EN
<!-- MULTILANGUAJE MENU END -->

<!--
# The `void` type and its use in functions
- Differences between `void` and `undefined` in returns
- Use in functions without explicit return
-->

<details id="toc-container">
<summary>Table of Contents</summary>

<!-- no toc -->
- [Differences between void and undefined in returns](#differences-between-void-and-undefined-in-returns)
  - [Definition and Purpose](#definition-and-purpose)
  - [Typing Implications](#typing-implications)
  - [Important Considerations](#important-considerations)
  - [Best Practices](#best-practices)
  - [Bad Practices](#bad-practices)
- [Use in functions without explicit return](#use-in-functions-without-explicit-return)
  - [Type Inference for void](#type-inference-for-void)
  - [Common Use Cases](#common-use-cases)
  - [async and Promise](#async-and-promise)
  - [Important Considerations](#important-considerations-1)
  - [Best Practices](#best-practices-1)
  - [Bad Practices](#bad-practices-1)

</details>

# The `void` type and its use in functions

In TypeScript, the `void` type represents the **intentional absence of a return value** in a function. It is an explicit and semantic way to indicate that a function performs an action or side effect (such as modifying state, printing to the console, calling another API, etc.), but **is not designed to return a useful result** that the calling code should process or use. Understanding `void` is fundamental for writing clear, maintainable TypeScript code aligned with the language's philosophy, especially when differentiating it from `undefined`.

---

## Differences between `void` and `undefined` in returns

Although `void` and `undefined` may seem similar at first glance, and indeed a `void` function in underlying JavaScript returns `undefined`, in the context of TypeScript they have distinct purposes and communicate fundamentally different intentions. The choice between them affects code clarity and robustness.

[↑ Back to Top](#toc-container)


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


### Typing Implications

The main difference lies in the **intention** they communicate to the compiler and other developers:

-   **`function myFuncion(): void { ... }`**: This signature unequivocally declares: "This function performs an action, but don't expect a useful value from it. If you try to use its result, you are probably making a mistake."
    -   Although you could technically `return undefined;` inside a `void` function (for historical compatibility with JavaScript), **doing so is confusing and considered bad practice in TypeScript**. It obscures the intention of `void`.
    -   TypeScript *allows* assigning the result of a `void` function to a variable (e.g., `const res = myFuncion();`), but the inferred type for `res` will be `void`, and its runtime value will be `undefined`. However, the `void` declaration itself **strongly discourages** this usage. The compiler won't stop you directly, but stricter linting tools or code reviews should flag it.

-   **`function otherFunction(): T | undefined { ... }`**: This signature indicates that the function *may* return a value of type `T`, but it *may also* return `undefined` as a *significant* and expected result in certain scenarios (e.g., a search function returning the found object or `undefined` if not found). Here, `undefined` is a legitimate value that the calling code must handle.

[↑ Back to Top](#toc-container)


#### Examples

```typescript
// Function explicitly typed as void: Performs an action, returns nothing useful.
function showMessage(message: string): void {
  console.log(message);
  // No return statement, or just `return;`
}

// TypeScript infers void if there's no return
function greet() { // Inferred as (): void
  console.log("Hello World");
}

// Function that implicitly returns undefined (JavaScript style)
function noExplicitReturnJS() {
  // No return
}
const jsResult = noExplicitReturnJS();
console.log(jsResult); // Output: undefined

// Function that explicitly returns undefined
function returnsUndefined(): undefined {
  return undefined;
}
const undResult = returnsUndefined();
console.log(undResult); // Output: undefined

// Using the result of a void function (discouraged)
const voidResult = showMessage("Testing void");
console.log(voidResult); // Output: undefined

// Alternatives to explicit `undefined`: If a function needs to indicate the absence of a valid result,
// it's often better to use `null`, throw an error, or return a union type like `T | null`
// or use Result types, rather than returning `undefined` explicitly,
// as `undefined` is often associated more with errors or uninitialized states.
```

### Important Considerations

-   **Clarity**: Using `void` improves code readability by making the intention of not returning a value clear. It avoids ambiguity about whether the absence of a `return` was intentional or an oversight.
-   **JavaScript Compatibility**: TypeScript's permissiveness in allowing `return undefined;` in `void` functions exists primarily for compatibility with existing JavaScript code. However, in new TypeScript code, it's best avoided.
-   **Alternatives to explicit `undefined`**: If a function needs to indicate the absence of a valid result, it's often better to use `null`, throw an error, or return a union type like `T | null` or use Result types instead of explicitly returning `undefined`, as `undefined` is usually associated more with errors or uninitialized states.

[↑ Back to Top](#toc-container)


### Best Practices

{% hint style="success" %}
Explicitly type functions intended for side effects without a useful return value as `void`, even if TypeScript could infer it. It improves intent.
{% endhint %}

-   Use `return;` for early exits in `void` functions when necessary for conditional logic.
-   Enable the `--noImplicitReturns` option in `tsconfig.json` to ensure that all functions expected to return a value do so explicitly in all code paths.

[↑ Back to Top](#toc-container)


### Bad Practices

{% hint style="warning" %}
Declaring a function as `void` and then assigning its result to a variable for use. This ignores the type's intent.
`const res = voidFunction(); /* res is undefined */ console.log(res); // <-- Bad practice`
{% endhint %}

{% hint style="warning" %}
Using `undefined` as the return type when the intention is simply to indicate no return value (use `void` in that case).
`function action(): undefined { return undefined; }` is worse than `function action(): void {}`.
{% endhint %}

-   Regularly returning `undefined` explicitly as a signal for "no result," instead of using more expressive alternatives like `null`, union types, or errors.
-   Confusing the `void` return type with the `void` operator in JavaScript: The `void` operator (e.g., `void 0` or `void(expression)`) evaluates an expression and always returns `undefined`. Although conceptually related (both deal with the absence of a useful value), they are distinct. The `void` type is a TypeScript construct for function signatures.

[↑ Back to Top](#toc-container)


---

## Use in functions without explicit return

TypeScript plays an important role in handling functions that lack an explicit `return` statement or only use `return;` without a value.

[↑ Back to Top](#toc-container)


### Type Inference for void

When a function in TypeScript (declared with `function` or as an arrow function) **has no `return` statement that returns a value**, or **only contains `return;`**, the TypeScript compiler **automatically infers** its return type as `void`.

This is a key difference from pure JavaScript, where such a function would simply return `undefined`. TypeScript's inference of `void` adds a layer of safety and semantic clarity, making the intention of not returning a useful value explicit.

[↑ Back to Top](#toc-container)


#### Examples

```typescript
// TypeScript infers (): void because there's no return with a value
function processData(data: any[]) {
  console.log("Processing...");
  data.forEach(item => console.log(item));
  // No return
}

// Arrow function also infers void
const logEvent = (event: string) => { // Infers (event: string) => void
  console.log(`Event logged: ${event}`);
  // No return
};

// Function with empty return also infers void
function validateInput(input: string): void { // Explicit typing (good practice)
  if (!input || input.trim() === '') {
    console.error("Invalid or empty input");
    return; // Early exit, consistent with void
  }
  console.log("Valid input:", input);
  // No return at the end, consistent with void
}
```

### Common Use Cases

`void` functions are extremely common and fundamental in many applications, especially where side effects are primary:

-   **Event Handlers**: Functions responding to user interactions (clicks, keys, form submissions) or system events (timers, network responses). Their job is to update application state, modify the UI, etc., not return a value to the event system.
    ```typescript
    // Example: Event listener on a button
    const myButton = document.getElementById('myButton');
    if (myButton) {
        myButton.addEventListener('click', (event: MouseEvent): void => {
          event.preventDefault(); // Side effect: prevents default behavior
          console.log('Button clicked!'); // Side effect: log
          // Logic to update UI... (side effect)
        });
    }
    ```
-   **Logging and Monitoring**: Functions dedicated to recording information (to console, files, external services) about the application's state or errors.
    ```typescript
    // Example: Simple logger
    function logError(error: Error, context?: Record<string, any>): void {
      console.error(`[ERROR]`, error.message, context ? JSON.stringify(context) : '');
      // Could also send 'error' and 'context' to an external monitoring service.
    }
    ```
-   **Direct DOM Manipulation**: Functions that change attributes, styles, or content of HTML elements directly.
    ```typescript
    // Example: Update a counter in the UI
    function updateCounterUI(newValue: number): void {
      const counterElement = document.getElementById('counter');
      if (counterElement) {
        counterElement.textContent = newValue.toString(); // Modifies the DOM
      } else {
        console.warn("Element 'counter' not found"); // Another side effect
      }
    }
    ```
-   **Configuration or Initialization Functions**: Code that runs once (or periodically) to prepare or modify the application's environment (e.g., initializing a library, establishing a connection).
    ```typescript
    // Example: Initialize a service
    function initializeAnalyticsService(apiKey: string): void {
      // Internal logic to configure the analytics service with the apiKey...
      console.log("Analytics service initialized.");
    }
    ```
-   **State Mutation**: Functions that directly modify external variables or data structures (although this is often better encapsulated).
     ```typescript
     // Example: Resetting a global state (simplified)
     let globalCounter = 10;
     function resetCounter(): void {
         globalCounter = 0; // Direct mutation
     }
     ```

[↑ Back to Top](#toc-container)


### async and Promise<void>

When working with asynchronous functions (`async`), the interaction with `void` is slightly different but follows the same philosophy.

-   An `async` function **always** returns a `Promise`.
-   If an `async` function is intended to perform an asynchronous operation but not return a useful value upon completion, its return type should be `Promise<void>`.
-   This indicates that the promise will resolve (eventually), but it will not resolve with a meaningful value. Code `await`ing this promise will get `undefined` when it resolves, but the `Promise<void>` signature communicates that this value should not be used.

[↑ Back to Top](#toc-container)


#### Example `async/Promise<void>`

```typescript
// Async function performing an operation (e.g., saving to DB) but not returning data
async function saveUser(name: string): Promise<void> {
  console.log(`Attempting to save user: ${name}`);
  try {
    // Simulate an asynchronous save operation
    await new Promise(resolve => setTimeout(resolve, 1000));
    console.log(`User ${name} saved successfully.`);
    // No return with value, the promise resolves without a value (implicitly undefined)
  } catch (error) {
    console.error("Error saving:", error);
    // We could re-throw the error or handle it here
    throw error; // Optional, if we want the promise to be rejected
  }
}

async function mainProcess(): Promise<void> {
    try {
        const result = await saveUser("Carlos");
        // 'result' here will be 'undefined' at runtime.
        // The Promise<void> signature tells us we shouldn't rely on this value.
        console.log("Save operation completed.");
    } catch (error) {
        console.error("Main process failed:", error);
    }
}

mainProcess();
```

### Important Considerations

{% hint style="info" %}
**`--noImplicitReturns` Option**: Enabled in `tsconfig.json`, this option is excellent practice. It forces **all** code paths within a function *expected* to return a value (i.e., not typed as `void` or `any`) to actually do so with a `return` statement. It helps prevent logical errors where a `return` is forgotten in some conditional branch. Functions correctly typed as `void` (or `Promise<void>`) do not trigger this rule, as their intent is precisely not to return an explicit value.
{% endhint %}

-   **Callback Functions and `void`**: When defining types for functions that accept callbacks, using `void` as the callback's return type is crucial if the receiving function will not (or should not) use any value the callback might return.
    ```typescript
    // The 'processItems' function executes a callback for each item,
    // but does not use any value the callback might return.
    function processItems<T>(items: T[], callback: (item: T) => void): void {
      items.forEach(item => {
        console.log("Processing item:", item);
        callback(item); // We call the callback, ignoring its potential return.
      });
    }

    // Example usage: The callback returns a number, but 'processItems' ignores it.
    processItems([1, 2, 3], (num) => {
      console.log(`Callback executed for ${num}`);
      return num * 2; // This returned value (2, 4, 6) is ignored by processItems.
    });
    ```
    This TypeScript feature allows functions that return values to be assignable to callback types expecting `void`, as long as the caller doesn't use the result.

[↑ Back to Top](#toc-container)


### Best Practices

-   Explicitly type functions intended for side effects without a useful return value as `void` (or `Promise<void>` for `async`). Even if TypeScript could infer it, explicitness improves code readability and intent.
-   Use `return;` for early exits in `void` functions when logic requires it (e.g., failed validation).
-   Enable and adhere to the `--noImplicitReturns` option in your `tsconfig.json`.

[↑ Back to Top](#toc-container)


### Bad Practices

-   **Inference Error**: Writing a function intended to be `void` but accidentally including a `return` statement with a value in some code path, without explicitly typing the return. TypeScript might infer a different type (e.g., `string | void`) and hide a logical error if `--noImplicitReturns` is not enabled.
    ```typescript
    // Potential issue without explicit typing or --noImplicitReturns
    function processOrPrint(value: number) { // Infers number | void !!
        if (value > 10) {
            return value * 2; // Returns a number
        }
        console.log("Low value:", value); // Returns nothing (void)
    }
    // This can lead to errors if assumed to always be void or always number.
    ```
-   **`async function(): void`**: **Avoid** typing an `async` function directly as `: void`. It must always return a `Promise`. Use `: Promise<void>` if there's no resolution value. Typing as `: void` can hide unhandled promises and lead to unexpected behavior.
-   **Ignoring `void` in Callbacks**: Incorrectly assuming a callback typed as `() => void` will always return `undefined`. It could return any value, but the signature indicates that value must be ignored by the invoking code.

[↑ Back to Top](#toc-container)

