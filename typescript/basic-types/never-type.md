<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/typescript/basic-types/never-type)
<!-- MULTILANGUAJE MENU END -->

<!--
# The `never` type for functions that don't return values

- Functions that throw errors (`throw`)
- Functions that never terminate (`while (true) {}`)
-->

<details id="toc-container">
<summary>Table of Contents</summary>

<!-- no toc -->
- [Functions that throw errors (`throw`)](#functions-that-throw-errors-throw)
  - [Definition and utility](#definition-and-utility)
  - [Real and recommended use cases](#real-and-recommended-use-cases)
  - [Important considerations](#important-considerations)
  - [Common errors and pitfalls](#common-errors-and-pitfalls)
  - [Good practices](#good-practices)
  - [Bad practices](#bad-practices)
- [Functions that never terminate (`while (true) {}`)](#functions-that-never-terminate-while-true)
  - [Definition and utility](#definition-and-utility-1)
  - [Real and recommended use cases](#real-and-recommended-use-cases-1)
  - [Important considerations](#important-considerations-1)
  - [Common errors and pitfalls](#common-errors-and-pitfalls-1)
  - [Good practices](#good-practices-1)
  - [Bad practices](#bad-practices-1)

</details>

# The `never` type for functions that don't return values

The `never` type in TypeScript is a special type representing values that can **never** occur. It fundamentally differs from `void`, which indicates the *intentional* absence of a return value (the function finishes but returns nothing), whereas `never` means the function **never reaches its endpoint** normally.

This scenario primarily occurs in two ways:
1.  The function always throws an exception (`throw`).
2.  The function enters an infinite loop that never terminates (e.g., `while (true) {}`).

Understanding `never` is crucial for the **static control flow analysis** performed by TypeScript. It allows the compiler to reason about code reachability, identify impossible states, and validate exhaustive checks, leading to more precise types and safer code, especially in advanced patterns like conditional types, type guards, and robust error handling.

[↑ Back to Top](#toc-container)


## Functions that throw errors (`throw`)

A primary and frequent application of `never` is in functions designed exclusively to signal an error and abruptly stop execution using `throw`. When TypeScript can statically determine that a function will *always* terminate by throwing an exception, it infers (or can be explicitly annotated) that its return type is `never`.

### Definition and utility

Throwing an exception is the standard mechanism for signaling serious or irrecoverable error conditions that prevent the normal continuation of the program. By assigning `never` to the return type of a function that always throws an error, TypeScript formalizes this guarantee: any code immediately following the invocation of such a function within the same logical block is considered **unreachable code**.

This information is valuable for the compiler because:
*   It allows detection of dead or logically inaccessible code.
*   It reinforces the validity of exhaustive checks in structures like `switch` or `if/else if/else`.
*   It helps prevent subtle errors by ensuring that certain code paths, under specific conditions (like an invalid state being detected), are never executed.

#### Example: Function that always throws an error

```typescript
// This function is designed solely to throw an error.
// Its return type is 'never' because it never returns a value normally.
function fail(message: string): never {
  throw new Error(message);
}

// Example usage in input validation
function processData(data: string | number | boolean): void {
  if (typeof data === 'string') {
    console.log(`Processing string: "${data.toUpperCase()}"`);
  } else if (typeof data === 'number') {
    console.log(`Processing number: ${data.toFixed(2)}`);
  } else if (typeof data === 'boolean') {
    console.log(`Processing boolean: ${data ? 'Yes' : 'No'}`);
  } else {
    // This branch is theoretically impossible!
    // We have covered all cases of the union (string | number | boolean).
    // TypeScript infers that 'data' here has the type 'never'.
    // Calling 'fail' is the correct way to handle this impossible state.
    fail(`Unexpected data type encountered: ${typeof data}`);
    // console.log('This will never execute'); // Unreachable code
  }
}

try {
  processData("example");
  processData(123.456);
  processData(false);
  // If we were to pass an unhandled type (although typing prevents this in TS),
  // the 'else' branch would execute, calling 'fail' and throwing the error.
} catch (error: any) {
  console.error(`Caught an error: ${error.message}`);
}
```
In `processData`, the final `else` branch would only be reached if `data` were neither `string`, `number`, nor `boolean`. Given the `string | number | boolean` typing, this situation is logically impossible. TypeScript recognizes this and infers `data` as `never` in that branch. Using `fail` here not only handles the impossible case but also satisfies the compiler, as `never` is assignable to any type (including the implicit `void` of `processData`).

[↑ Back to Top](#toc-container)


### Real and recommended use cases

#### Strict validation and critical failures

Ideal for validation functions that check essential preconditions for execution. If a critical validation fails (e.g., missing configuration, irrecoverable inconsistent state), a `never` function can halt execution immediately, preventing operations in an invalid state.

```typescript
interface ServerConfig {
  port: number;
  host?: string; // Optional
  apiKey: string; // Required
}

function assertConfigIsValid(config: Partial<ServerConfig>): asserts config is ServerConfig {
  if (config.port === undefined || config.port <= 0 || config.port > 65535) {
    fail(`Invalid configuration: Port must be between 1 and 65535. Received: ${config.port}`);
  }
  if (config.apiKey === undefined || config.apiKey.trim() === '') {
    fail("Invalid configuration: API Key is required and cannot be empty.");
  }
  // If it reaches here, the configuration has the required properties (port, apiKey)
  // 'asserts config is ServerConfig' acts as a type guard.
}

const initialConfig: Partial<ServerConfig> = { port: 8080 /* apiKey missing */ };

try {
  assertConfigIsValid(initialConfig);
  // If there was no error, TypeScript knows that 'initialConfig' is now 'ServerConfig'
  console.log(`Configuration validated. Starting server on port ${initialConfig.port} with key ${initialConfig.apiKey}.`);
} catch (error: any) {
  console.error(`Application startup failed: ${error.message}`);
  // We could terminate the process here: process.exit(1);
}

```
Here, `assertConfigIsValid` uses `fail` (which returns `never`) to stop upon encountering invalid configuration. The use of `asserts config is ServerConfig` is an advanced TypeScript feature that informs the compiler that if the function doesn't throw an error, the type of the `config` object is refined to `ServerConfig`.

#### Exhaustiveness Checks

A fundamental technique in TypeScript, especially with discriminated unions or `enum`. `never` is used in the `default` branch of a `switch` or the final `else` branch of an `if/else if` to ensure that all possible cases have been explicitly handled. If a new member is added to the union/enum and the `switch`/`if` is not updated, the compiler will detect an error because the value in the `default`/`else` branch will no longer be assignable to `never`.

```typescript
type Result<T, E> = { success: true; value: T } | { success: false; error: E };

function processResult<T, E>(result: Result<T, E>): string {
  if (result.success) {
    // TypeScript knows that 'result' here is { success: true; value: T }
    return `Operation succeeded with value: ${result.value}`;
  } else {
    // TypeScript knows that 'result' here is { success: false; error: E }
    return `Operation failed with error: ${result.error}`;
  }
  // If there were an additional 'else' branch here, 'result' would be 'never'.
  // Example with switch:
  // switch (result.success) {
  //   case true: return `Success: ${result.value}`;
  //   case false: return `Error: ${result.error}`;
  //   default:
  //     const exhaustiveCheck: never = result; // Error if 'result' is not 'never'
  //     return fail(`Unhandled result state: ${exhaustiveCheck}`);
  // }
}

const successResult: Result<string, Error> = { success: true, value: "Data fetched" };
const errorResult: Result<string, Error> = { success: false, error: new Error("Network timeout") };

console.log(processResult(successResult));
console.log(processResult(errorResult));
```

{% hint style="info" %}
Exhaustiveness checking using `never` is a crucial safety net. It transforms logical errors (forgetting to handle a case) into compile-time errors, facilitating safe maintenance and refactoring when adding new variants to types.
{% endhint %}

[↑ Back to Top](#toc-container)


### Important considerations

*   **Flow Interruption:** The main consequence of calling a `never` function is the immediate halt of the normal execution flow. Ensure this behavior is intentional and appropriate for the situation (critical errors, impossible states).
*   **Error Handling vs. Exceptions:** `never` functions that throw errors are part of the exception mechanism. They should be used to signal *failures* (bugs, irrecoverable states). For expected *operational* errors (invalid user input, resource not found), prefer returning values like `null`, `undefined`, a specific `Error` object, or using `Promise.reject`, allowing for more controlled handling by the client code.
*   **Explicit Typing:** Although TypeScript often infers `never`, explicitly annotating it (`function fail(): never { ... }`) improves readability and clearly documents the intent.
*   **Debugging:** When a `never` function throws an error, the stack trace will point to where the exception was thrown. This facilitates debugging critical errors.

[↑ Back to Top](#toc-container)


### Common errors and pitfalls

*   **Confusing `never` with `void`:** `void` means the function returns without a value, but it *returns*. `never` means it *never returns* (throws an error or infinite loop). Using `void` where `never` is needed (or vice versa) can lead to incorrect logic or errors not caught by the compiler.

{% hint style="warning" %}
**Don't use `throw` for normal control flow:** Exceptions and `never` functions are for exceptional or irrecoverable errors, not for regular business logic like simple validations or conditional decisions. Overusing `throw` makes code harder to follow and debug.
{% endhint %}

*   **Unreachable code not detected:** If a function *might* throw an error but doesn't *always* do so, its type won't be `never`. If you mistakenly assume it's `never`, you might have unreachable code that the compiler doesn't detect.
*   **Forgetting `catch`:** If a `never` function that throws an error is called outside a `try...catch` block, it will halt global execution (in Node.js, the process; in the browser, the script) if the error isn't caught at a higher level.

[↑ Back to Top](#toc-container)


### Good practices

*   **Reserve `never` (with `throw`) for irrecoverable errors:** Use it for assertion failures, critical configuration errors, or to mark states that should logically never be reached.
*   **Implement exhaustive checks:** This is one of the most valuable uses of `never`. Ensure your `switch` and `if/else` statements over discriminated unions end with a `default`/`else` branch that assigns to `never` or calls a `never` function.
*   **Document the reason:** If a function returns `never`, clearly explain why in its JSDoc (e.g., `@returns {never} Always throws an exception if...`).
*   **Create utility functions:** Define reusable helper functions like `fail(message: string): never` or `assertUnreachable(x: never): never` to centralize irrecoverable error logic.

[↑ Back to Top](#toc-container)


### Bad practices

*   **Handling expected errors with `throw`/`never`:** Don't use it for form validation, predictable network failures, etc. Use more informative return types (`Result<T, E>`, `Promise<T>`, `T | null`).
*   **Throwing generic errors:** Instead of `throw new Error("Error")`, throw specific errors (`throw new ConfigurationError(...)`) to facilitate differentiated catching and handling.

{% hint style="warning" %}
**Avoid code after a `never` call:** Don't place executable code after invoking a `never` function in the same logical block. It's confusing and will always be unreachable.
{% endhint %}

*   **Suppressing exhaustiveness check errors:** Don't ignore compiler errors in `default`/`else` branches with `never`. They indicate a missing case that needs handling.

[↑ Back to Top](#toc-container)


---

## Functions that never terminate (`while (true) {}`)

The second way for a function to have the `never` type is through an infinite loop from which there is no possible exit (like `while (true)` without `break`, `return`, or `throw` inside). This type of function never completes its execution and, therefore, never returns control to the calling point.

### Definition and utility

In certain specific contexts, such as implementing servers, daemon processes, or main event loops (game loops, certain reactive frameworks), a function might be designed to run indefinitely. TypeScript recognizes this infinite loop structure and, if it can determine there's no escape path, infers the return type `never`.

This indicates that the function is intended to **monopolize the execution thread** (if synchronous) or represent a **persistent process**.

#### Example: Function with an infinite loop

```typescript
// Function designed to run indefinitely, processing events.
function eventLoop(): never {
  console.log("Event loop started. Waiting for events...");
  while (true) {
    // 1. Wait for an event (simulated with a delay)
    // In a real case, this could be I/O, timers, etc.
    const startTime = Date.now();
    while(Date.now() - startTime < 2000) { /* Simulate busy waiting */ }

    // 2. Process the event (simulated)
    const eventData = `Event at ${new Date().toLocaleTimeString()}`;
    console.log(`Processing: ${eventData}`);

    // The loop continues indefinitely, there's never a 'return' or 'break'.
  }
  // This code is unreachable
  // console.log("Event loop finished.");
}

// Calling eventLoop() would block the current thread.
// We don't execute it directly here to avoid blocking the explanation.
// try {
//   eventLoop();
// } catch (error) {
//   // This catch wouldn't trigger unless the loop threw an error.
//   console.error("Event loop crashed:", error);
// }
// console.log("This line would never be reached if eventLoop() was called.");
```
The `eventLoop` function contains a `while (true)` without any exit condition. Therefore, TypeScript infers `never` as its return type.

[↑ Back to Top](#toc-container)


### Real and recommended use cases

#### Server/Daemon Entry Points

The main function that starts an HTTP server, a message queue worker, or any process intended to run continuously until terminated externally.

```typescript
import http from 'http'; // Example with Node.js module

function startHttpServer(port: number): never {
  const server = http.createServer((req, res) => {
    console.log(`Received request: ${req.method} ${req.url}`);
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello from the never-ending server!\n');
  });

  server.listen(port, () => {
    console.log(`Server listening on http://localhost:${port}`);
    // Although 'listen' returns, the 'startHttpServer' function conceptually
    // enters an "infinite" waiting state.
    // To make it explicitly 'never', we could add a loop.
  });

  // To be rigorously 'never', we could add a loop post-listen,
  // although in Node.js practice this isn't usually necessary.
  while(true) {
     // Keep the process alive explicitly (although listen already does)
     // Could perform status checks, etc.
     const start = Date.now();
     while(Date.now() - start < 60000) { /* Wait 1 minute */ }
     console.log("Server heartbeat...");
  }
  // Note: In real Node.js, 'listen' itself keeps the process alive.
  // The while(true) loop here is more illustrative of the 'never' concept.
}

// startHttpServer(3000); // Would start the server and never return.
```

#### Game Loops or Continuous Rendering

In graphics applications or games, a main loop might handle reading input, updating state, and rendering the screen repeatedly.

```typescript
function gameEngineLoop(): never {
  let frameCount = 0;
  console.log("Game Engine Initialized. Starting loop...");
  while (true) {
    const startTime = performance.now();

    // 1. Handle Input (e.g., keyboard, mouse)
    // const input = readGameInput();

    // 2. Update Game State
    // updateWorld(input, frameCount);

    // 3. Render Graphics
    // renderScene();

    const endTime = performance.now();
    const elapsedTime = endTime - startTime;
    console.log(`Frame ${frameCount++}: Rendered in ${elapsedTime.toFixed(2)} ms`);

    // FPS control (simple example, not very accurate)
    const targetFrameTime = 1000 / 60; // 60 FPS
    const waitTime = Math.max(0, targetFrameTime - elapsedTime);
    const waitStart = Date.now();
    while(Date.now() - waitStart < waitTime) { /* Wait */ }
  }
}
```

[↑ Back to Top](#toc-container)


### Important considerations

{% hint style="danger" %}
**CRITICAL! Blocking the Main Thread with Synchronous Loops:** A synchronous `while(true)` loop in a `never` function **freezes** the execution thread. In Node.js, this stops the event loop, preventing handling of new requests or I/O. In the browser, it freezes the UI. **Never use blocking synchronous infinite loops on the main thread.**
{% endhint %}

{% hint style="info" %}
**Prioritize Asynchronous Alternatives:** For servers, workers, or long-running tasks, asynchronous models (`async/await`, `Promises`, `EventEmitter`, Web Workers) are the standard in JS/TS. They allow concurrency and prevent blocking the main thread. A blocking `never` function is rarely the right solution.
{% endhint %}

*   **Intentionality:** Ensure the infinite loop is truly intentional and the only appropriate way to model the behavior (which is rare for synchronous blocking code).
*   **Termination:** A `never` function of this type only terminates if the process is stopped externally (e.g., `Ctrl+C`, `kill`) or if an unhandled error occurs *inside* the loop causing it to end.

{% hint style="warning" %}
**Avoid `Busy-Waiting`:** A `while(true)` loop without pauses (I/O, `setTimeout`, `await delay`) will consume 100% CPU. This is inefficient and harmful. Always introduce appropriate waiting mechanisms.
{% endhint %}

*   **Confusion with asynchronous processes:** Don't confuse a blocking synchronous `never` function with a long-running asynchronous process (like a typical Node.js server) that does *not* block the thread.

[↑ Back to Top](#toc-container)


### Good practices

*   **Limit to entry points:** Use `never` functions with synchronous infinite loops *only* at the main entry point of a process specifically designed to run that way (and preferably not on the main thread if concurrency is needed).

{% hint style="success" %}
**Favor Asynchronous Models:** For almost all persistent or long-running tasks in JS/TS (servers, workers, listeners), use `async/await`, `Promises`, `EventEmitter`, or Web Workers. They are more efficient and non-blocking.
{% endhint %}

*   **Include internal error handling:** If the loop must continue despite internal errors, implement `try...catch` inside the loop.
*   **Introduce waiting mechanisms:** Avoid `busy-waiting`. Use asynchronous I/O, `setTimeout`, `setInterval`, or appropriate waiting mechanisms to free up the thread and reduce CPU consumption.
*   **Document:** Explain why the function never returns and its implications (e.g., `@returns {never} Enters an infinite loop to process tasks. Blocks the thread.`).

[↑ Back to Top](#toc-container)


### Bad practices

{% hint style="danger" %}
**NEVER Block the Main Thread:** synchronous `while(true)` loops are extremely dangerous on the main thread of Node.js or the browser. It's the main bad practice to avoid with `never` and infinite loops.
{% endhint %}

*   **`Busy-waiting`:** Infinite loops that consume CPU without doing useful work or waiting efficiently.
*   **Not handling errors inside the loop:** Allowing internal errors to stop a process that should be resilient.
*   **Using it when a normal loop with an exit condition is more appropriate:** Don't force a `never` if the process has a natural termination condition.

[↑ Back to Top](#toc-container)