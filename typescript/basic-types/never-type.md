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
- [Introduction: The `never` Type](#introduction-the-never-type)
  - [Key Difference: `never` vs `void`](#key-difference-never-vs-void)
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

## Introduction: The `never` Type

The `never` type in TypeScript is a special type representing values that can **never** occur or situations from which a function **never** returns normally. It is used to indicate that a function always throws an exception or enters an infinite loop.

Understanding `never` is crucial for the **static control flow analysis** performed by TypeScript. It allows the compiler to reason about code reachability, identify impossible states, and validate exhaustive checks, leading to more precise types and safer code, especially in advanced patterns like conditional types, type guards, and robust error handling.

### Key Difference: `never` vs `void`

It is fundamental to distinguish `never` from `void`. While both indicate the absence of a *useful* return value, their meaning differs:

| Feature           | `never`                                                       | `void`                                                     |
| :---------------- | :------------------------------------------------------------ | :--------------------------------------------------------- |
| **Meaning**       | The function **never** returns normally.                      | The function **returns** but without an explicit value.    |
| **Completion**    | Terminates abruptly (error) or doesn't terminate.             | Terminates normally.                                       |
| **Value**         | Represents the absence of *any* value.                        | Represents the absence of a *return* value.                |
| **Assignability** | Assignable to **any** type.                                   | Assignable only to `any`, `unknown`, and `void`.           |
| **Typical Use**   | Functions throwing errors, infinite loops, exhaustive checks. | Functions without `return`, callbacks not returning value. |

This distinction is vital for the compiler's control flow analysis.

[↑ Back to Top](#toc-container)

---

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
  // Throws an exception, stopping normal execution.
  throw new Error(message);
  // Any code here would be unreachable.
}

// Example usage in input validation for a union type.
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
    // 'never' is assignable to any type, including 'void' (implicit return).
    fail(`Unexpected data type encountered: ${typeof data}`);
    // console.log('This will never execute'); // Unreachable code detected by TS
  }
}

try {
  processData("example");
  processData(123.456);
  processData(false);
  // If, hypothetically, we passed an unhandled type (e.g., an object if the type were any),
  // the 'else' branch would execute, calling 'fail' and throwing the error.
  // processData({ kind: 'unexpected' } as any); // Uncomment to test the error
} catch (error: any) {
  // The error thrown by 'fail' would be caught here.
  console.error(`Caught an error: ${error.message}`);
}
```
In the `processData` example, the final `else` branch would only be reached if `data` were neither `string`, `number`, nor `boolean`. Given the `string | number | boolean` type definition, this situation is logically impossible under TypeScript's type system. The compiler recognizes this and infers the type of `data` within that branch as `never`. Calling `fail` here not only semantically handles the impossible case but also satisfies the compiler, as a function returning `never` can be used anywhere a return is expected, even `void`, because it guarantees that return point will never be reached.

[↑ Back to Top](#toc-container)

### Real and recommended use cases

#### Strict validation and critical failures

Ideal for validation functions that check essential preconditions for execution. If a critical validation fails (e.g., missing configuration, irrecoverable inconsistent state), a `never` function can halt execution immediately, preventing operations in an invalid state. This is common in application or module initialization.

#### Example: Configuration validation at startup

```typescript
interface ServerConfig {
  port: number;
  host?: string; // Optional, might have a default value
  apiKey: string; // Required for core functionality
  databaseUrl: string; // Required
}

// Helper function that always throws an error, returning 'never'
function configurationError(missingField: keyof ServerConfig, value?: any): never {
  throw new Error(`Invalid or missing configuration: Field '${missingField}' is invalid. ${value !== undefined ? `Received: ${value}` : ''}`);
}

// Assertion function that validates the configuration.
// Uses 'asserts' to refine the type if validation passes.
function assertConfigIsValid(config: Partial<ServerConfig>): asserts config is ServerConfig {
  if (config.port === undefined || config.port <= 0 || config.port > 65535) {
    configurationError('port', config.port); // Throws error -> never
  }
  if (config.apiKey === undefined || config.apiKey.trim() === '') {
    configurationError('apiKey'); // Throws error -> never
  }
  if (config.databaseUrl === undefined || !config.databaseUrl.startsWith('postgres://')) {
     configurationError('databaseUrl', config.databaseUrl); // Throws error -> never
  }
  // If the function reaches this point without throwing an error, TypeScript knows
  // that 'config' conforms to the 'ServerConfig' interface thanks to 'asserts'.
}

// Simulate configuration read from a file or environment variables
const initialConfig: Partial<ServerConfig> = {
  port: 8080,
  // apiKey intentionally missing to test the error
  databaseUrl: 'postgres://user:pass@host:5432/db'
};

try {
  assertConfigIsValid(initialConfig);
  // If there was no error, TypeScript knows 'initialConfig' is now 'ServerConfig'
  // and we can access its properties safely.
  console.log(`Configuration validated successfully.`);
  console.log(`Starting server on port ${initialConfig.port}...`);
  console.log(`Using API Key: ${initialConfig.apiKey.substring(0, 3)}...`); // Safe access
  console.log(`Connecting to DB: ${initialConfig.databaseUrl}`); // Safe access

} catch (error: any) {
  // Catches the error thrown by configurationError
  console.error(`[FATAL] Application startup failed due to configuration error: ${error.message}`);
  // In a real scenario, we might terminate the process gracefully here.
  // process.exit(1);
}
```
Here, `assertConfigIsValid` uses a helper function `configurationError` (which returns `never`) to stop upon encountering invalid configuration. The use of `asserts config is ServerConfig` is a key feature informing the compiler: if this function returns normally (i.e., doesn't throw an error), then the type of the `config` object should be refined to `ServerConfig` in the code following the call. This avoids the need for redundant checks after validation.

#### Exhaustiveness Checks

A fundamental technique in TypeScript, especially with discriminated unions or `enum`. `never` is used in the `default` branch of a `switch` or the final `else` branch of an `if/else if` to ensure that all possible cases have been explicitly handled. If a new member is added to the union/enum and the `switch`/`if` is not updated, the compiler will detect an error because the value in the `default`/`else` branch will no longer be assignable to `never`.

#### Example: Processing different event types

```typescript
// Discriminated union to represent different event types
type AppEvent =
  | { type: 'USER_LOGIN'; userId: string; timestamp: Date }
  | { type: 'USER_LOGOUT'; userId: string; timestamp: Date }
  | { type: 'ITEM_ADDED'; itemId: string; quantity: number; timestamp: Date };
  // | { type: 'ORDER_PLACED'; orderId: string; timestamp: Date }; // If we uncomment this, the switch will fail

// Helper function to handle impossible cases
function assertUnreachable(x: never): never {
  throw new Error(`Unhandled case: ${JSON.stringify(x)}`);
}

function handleEvent(event: AppEvent): void {
  console.log(`Handling event type: ${event.type} at ${event.timestamp.toISOString()}`);

  switch (event.type) {
    case 'USER_LOGIN':
      // TypeScript knows 'event' here is { type: 'USER_LOGIN'; ... }
      console.log(`User ${event.userId} logged in.`);
      // Specific login logic...
      break;

    case 'USER_LOGOUT':
      // TypeScript knows 'event' here is { type: 'USER_LOGOUT'; ... }
      console.log(`User ${event.userId} logged out.`);
      // Specific logout logic...
      break;

    case 'ITEM_ADDED':
      // TypeScript knows 'event' here is { type: 'ITEM_ADDED'; ... }
      console.log(`${event.quantity} of item ${event.itemId} added.`);
      // Specific item addition logic...
      break;

    // The 'default' branch ensures all cases are handled.
    default:
      // If we have handled all cases of 'AppEvent', TypeScript infers
      // that 'event' in this branch has the type 'never'.
      // If we added a new type to 'AppEvent' without adding a 'case',
      // 'event' would no longer be 'never', and the assignment to 'exhaustiveCheck' would fail.
      const exhaustiveCheck: never = event;
      // We can call a 'never' function to throw an explicit error.
      assertUnreachable(exhaustiveCheck);
  }
}

// Example events
const loginEvent: AppEvent = { type: 'USER_LOGIN', userId: 'user123', timestamp: new Date() };
const addItemEvent: AppEvent = { type: 'ITEM_ADDED', itemId: 'itemABC', quantity: 5, timestamp: new Date() };

handleEvent(loginEvent);
handleEvent(addItemEvent);

// If 'ORDER_PLACED' is added to AppEvent without updating the switch,
// passing an event of that type would execute the default branch,
// TypeScript would detect that 'event' is not 'never', and the call
// to assertUnreachable(event) would cause a compile-time error.
```

{% hint style="info" %}
Exhaustiveness checking using `never` is a crucial safety net during development and maintenance. It transforms logical errors (forgetting to handle a new case) into compile-time detectable errors, facilitating safe refactoring and evolution of the application's data types.
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
    while(Date.now() - startTime < 2000) { /* Simulate busy waiting or async work */ }
    
    // 2. Process the event (simulated)
    const eventData = { type: Math.random() > 0.5 ? 'A' : 'B', value: Math.random() };
    console.log(`Processing event: ${JSON.stringify(eventData)}`);
    
    // No 'break', 'return', or 'throw' - the loop never exits.
  }
  // Code here is unreachable
}

// Calling this function will block the main thread (if synchronous)
// or run indefinitely (if asynchronous operations were used inside).
console.log("Preparing to start the loop...");
// eventLoop(); // Uncommenting this will block further execution
// console.log("This message will never be printed if eventLoop is called.");
```

[↑ Back to Top](#toc-container)

### Real and recommended use cases

#### Implementing Background Services or Daemons

Processes designed to run continuously in the background, monitoring systems, listening for connections, or performing periodic tasks.

```typescript
// Simplified example of a background monitoring service
async function monitorSystemHealth(): never {
  console.log("System health monitor started.");
  while (true) {
    try {
      // Check system resources (CPU, memory, disk)
      const cpuUsage = await checkCpu(); // Assume async check
      const memoryUsage = await checkMemory();
      console.log(`Current health - CPU: ${cpuUsage}%, Memory: ${memoryUsage}%`);

      if (cpuUsage > 90 || memoryUsage > 85) {
        // Send alert (but don't stop the monitor)
        sendAlert("High system resource usage detected!");
      }

      // Wait for the next check interval
      await sleep(60000); // Wait 1 minute (assume async sleep)
    } catch (error) {
      console.error("Error during health check:", error);
      // Log the error but continue monitoring
      await sleep(5000); // Shorter wait after an error
    }
  }
}

// Placeholder functions for simulation
async function checkCpu(): Promise<number> { return Math.random() * 100; }
async function checkMemory(): Promise<number> { return Math.random() * 100; }
function sendAlert(message: string): void { console.warn(`ALERT: ${message}`); }
async function sleep(ms: number): Promise<void> { return new Promise(resolve => setTimeout(resolve, ms)); }

// To start the monitor (would typically run in its own process/thread)
// monitorSystemHealth();
```

#### Main Loops in Applications

Common in game development (the game loop), certain types of servers, or reactive systems where the application continuously waits for and processes input or events.

```typescript
// Conceptual game loop
function gameLoop(): never {
  let lastFrameTime = performance.now();
  while (true) {
    const currentTime = performance.now();
    const deltaTime = (currentTime - lastFrameTime) / 1000; // Time in seconds

    // 1. Process Input
    processInput();

    // 2. Update Game State
    updateGameState(deltaTime);

    // 3. Render Frame
    renderFrame();

    lastFrameTime = currentTime;
    // Request next frame (in browser environments)
    // await requestAnimationFramePromise(); // Assumes an async wrapper
    // Or just continue loop for simple cases / non-browser
  }
}

// Placeholder functions
function processInput(): void { /* Handle keyboard/mouse */ }
function updateGameState(dt: number): void { /* Move characters, physics */ }
function renderFrame(): void { /* Draw graphics */ }

// startGameLoop(); // This would take over the execution thread
```

[↑ Back to Top](#toc-container)

### Important considerations

*   **Blocking Behavior (Synchronous):** A synchronous infinite loop (`while (true)` without `await` inside) will completely block the execution thread it runs on. In Node.js or single-threaded browser environments, this will make the application unresponsive. Use this pattern only in dedicated worker threads, separate processes, or when the blocking nature is the intended behavior (rarely the case in typical web/Node.js apps).
*   **Asynchronous Loops:** Infinite loops containing `await` (like in the `monitorSystemHealth` example) yield control back to the event loop while waiting for the asynchronous operation to complete. This prevents blocking but still represents a process that runs indefinitely.
*   **Resource Consumption:** Infinite loops, even asynchronous ones, consume resources. Ensure they are necessary and implement appropriate delays (`sleep`, `setTimeout`) or event-driven logic to avoid unnecessary CPU usage.
*   **Termination:** Functions typed as `never` due to infinite loops provide no built-in way to terminate gracefully *from within*. Termination must usually be handled externally (e.g., killing the process, signaling from another thread/process, specific framework shutdown hooks).

[↑ Back to Top](#toc-container)

### Common errors and pitfalls

*   **Accidental Infinite Loops:** Creating an infinite loop unintentionally due to a logical error in the loop condition or lack of a `break`/`return`. TypeScript might infer `never` here, but the behavior is likely a bug.
*   **Blocking the Main Thread:** Running a synchronous infinite loop on the main thread of a Node.js application or in a browser UI thread, causing unresponsiveness.
*   **Lack of Error Handling:** If operations inside the loop can fail, not including `try...catch` can cause the entire loop/process to crash unexpectedly.

[↑ Back to Top](#toc-container)

### Good practices

*   **Use Only When Necessary:** Reserve infinite loops for genuine long-running processes like servers, daemons, or main event loops.
*   **Prefer Asynchronous Loops:** If the loop involves I/O or waiting, use `async/await` to avoid blocking.
*   **Include Error Handling:** Wrap potentially failing operations inside the loop with `try...catch` to ensure the loop can continue even if errors occur.
*   **Implement Delays/Yielding:** Use `await sleep(...)`, `setTimeout`, or event listeners to prevent the loop from consuming 100% CPU.
*   **Consider Termination:** While the function type is `never`, think about how the containing process *can* be terminated gracefully if needed (external signals, IPC).

[↑ Back to Top](#toc-container)

### Bad practices

*   **Using infinite loops for tasks that should terminate:** If a task has a defined end condition, use a standard loop (`for`, `while` with a condition) or recursion.
*   **Synchronous blocking loops on main threads:** This is almost always incorrect in UI or server applications.
*   **Infinite loops without delays or yielding:** Leads to unnecessary resource consumption.

Understanding both facets of `never`—functions that throw and functions that loop infinitely—allows you to accurately model control flow, leverage TypeScript's exhaustiveness checking, and write safer, more predictable code.

[↑ Back to Top](#toc-container)