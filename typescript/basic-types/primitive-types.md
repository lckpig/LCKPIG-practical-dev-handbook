<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/typescript/basic-types/primitive-types)
<!-- MULTILANGUAJE MENU END -->

<!--
# Primitive Types in TypeScript

- `string`, `number`, `boolean`, `null`, `undefined`
- Differences between `null` and `undefined`
- Using `bigint` for operations with large numbers
-->

<details id="toc-container">
<summary>Table of Contents</summary>

<!-- no toc -->
- [The Fundamental Primitive Types: `string`, `number`, `boolean`](#the-fundamental-primitive-types-string-number-boolean)
  - [`string`](#string)
  - [`number`](#number)
  - [`boolean`](#boolean)
  - [Best Practices with `string`, `number`, `boolean`](#best-practices-with-string-number-boolean)
  - [Bad Practices](#bad-practices)
- [Absence of Value: `null` and `undefined`](#absence-of-value-null-and-undefined)
  - [`undefined`](#undefined)
  - [`null`](#null)
  - [Key Differences and When to Use Each](#key-differences-and-when-to-use-each)
  - [Best Practices with `null` and `undefined`](#best-practices-with-null-and-undefined)
  - [Bad Practices](#bad-practices-1)
- [Handling Large Numbers: `bigint`](#handling-large-numbers-bigint)
  - [`bigint`](#bigint)
  - [Important Considerations about `bigint`](#important-considerations-about-bigint)
  - [Best Practices with `bigint`](#best-practices-with-bigint)
  - [Bad Practices](#bad-practices-2)

</details>

# Primitive Types in TypeScript

TypeScript, being a superset of JavaScript, inherits its fundamental primitive data types. These types are the foundation upon which more complex data structures are built. Understanding them thoroughly is essential for writing robust and efficient TypeScript code.

## The Fundamental Primitive Types: `string`, `number`, `boolean`

These three types are likely the most used in any application. They represent text, numbers, and truth values, respectively.

### `string`

The `string` type is used to represent textual data. TypeScript, like JavaScript, uses single quotes (`'`), double quotes (`"`), or template literals (backticks `` ` ``) to delimit text strings.

#### Example: String Declaration

```typescript
let firstName: string = "Alice"; // Using firstName instead of nombre for clarity in English context
let greeting: string = 'Hello, ' + firstName;
let age: number = 30;
// Template literals allow interpolation of variables and expressions
let fullMessage: string = `Hello, my name is ${firstName} and I am ${age} years old.`;

console.log(firstName); // Output: Alice
console.log(greeting); // Output: Hello, Alice
console.log(fullMessage); // Output: Hello, my name is Alice and I am 30 years old.
```

[↑ Back to Top](#toc-container)


### `number`

The `number` type represents both integers and floating-point numbers. TypeScript uses the IEEE 754 standard to represent numbers, which implies certain peculiarities regarding precision and limits.

#### Example: Numerical Operations

```typescript
let quantity: number = 10;
let unitPrice: number = 49.99;
let total: number = quantity * unitPrice; // Multiplication
let discount: number = 0.1; // 10% discount
let finalPrice: number = total * (1 - discount);

console.log(total);         // Output: 499.9
console.log(finalPrice);   // Output: 449.91
```

[↑ Back to Top](#toc-container)


### `boolean`

The `boolean` type represents a logical value and can only have two values: `true` or `false`. It is fundamental for conditional logic and control flow.

#### Example: Using Booleans in Conditionals

```typescript
let isAdult: boolean = age >= 18; // age was previously defined as 30
let hasPermission: boolean = false;

if (isAdult && hasPermission) {
  console.log("Can drive.");
} else if (isAdult) {
  console.log("Is an adult, but does not have permission to drive.");
} else {
  console.log("Is not an adult.");
}
// Output: Is an adult, but does not have permission to drive.
```

[↑ Back to Top](#toc-container)


### Best Practices with `string`, `number`, `boolean`

*   **Consistency in Quotes**: Choose a type of quotes (single or double) and be consistent throughout your project to improve readability. Template literals are preferable when you need interpolation, multiline strings, or working with internationalization (i18n) by facilitating the insertion of variables into translated texts.
*   **Avoid `Number` vs `number`**: Always use the primitive type `number` instead of the `Number` object. The same applies to `string` vs `String` and `boolean` vs `Boolean`. Primitive types are more efficient and avoid unexpected behavior.
*   **Clarity in Booleans**: Name boolean variables descriptively, preferably as questions or statements (e.g., `isActive`, `hasPermission`, `isValid`).

[↑ Back to Top](#toc-container)


### Bad Practices

*   **Imprecise Comparisons**: Be careful when comparing floating-point numbers directly due to potential precision issues. Instead of `a === b`, consider `Math.abs(a - b) < smallThreshold`.

{% hint style="warning" %}
Direct comparison of floating-point numbers (`===`) can fail due to how they are represented internally. Using a small threshold (`epsilon`) for comparisons is a safer practice to avoid unexpected errors.
{% endhint %}

*   **Blindly Relying on Type Coercion**: JavaScript performs automatic type coercion in certain operations (e.g., `1 == '1'`). TypeScript helps prevent this, but it's good to be explicit and avoid relying on implicit coercion.

[↑ Back to Top](#toc-container)


---

## Absence of Value: `null` and `undefined`

These two types represent the absence of a value, but they have subtle differences in their semantics and usage.

### `undefined`

A variable that has not been assigned a value has the value `undefined`. It is also the value returned by functions that do not have an explicit `return` statement or that return `void`. It is generally considered as the "unintentional" or "not yet assigned" absence of value.

### `null`

`null` represents the intentional absence of an object value. When you assign `null` to a variable, you are explicitly indicating that this variable does not reference any object at that moment.

### Key Differences and When to Use Each

Although both represent the absence of value, their uses and meanings differ:

| Feature        | `undefined`                               | `null`                                              |
| -------------- | ----------------------------------------- | --------------------------------------------------- |
| **Meaning**    | Value not assigned (yet) or non-existent. | Value explicitly assigned as "no object".           |
| **Assignment** | Automatic for uninitialized variables.    | Must be explicitly assigned by the programmer.      |
| **`typeof`**   | `"undefined"`                             | `"object"` (historical quirk)                       |
| **Common Use** | Optional parameters, props not passed.    | API returns (object or nothing), resetting objects. |
| **Context**    | "Default" or unintentional absence.       | Intentional and significant absence.                |

*   **Intentionality**: `null` is intentionally assigned by the programmer to indicate "no value". `undefined` is often the default value of uninitialized variables or omitted function parameters.

{% hint style="warning" %}
The fact that `typeof null` returns `"object"` is a historical quirk of JavaScript that persists for compatibility reasons. It does not mean that `null` is a real object. Keep this in mind when checking types.
{% endhint %}

*   **Type**: `typeof null` returns `"object"` (a historical error in JavaScript). `typeof undefined` returns `"undefined"`.

*   **Common Usage**:
    *   Use `undefined` to check if a variable has been initialized or if an optional parameter was provided.
    *   Use `null` when you want to explicitly indicate that a variable that would normally contain an object currently has no value (e.g., in APIs that return an object or `null`).

#### Example: Differences between `null` and `undefined`

```typescript
let undefinedValue: undefined; // Automatically undefined
let nullValue: null = null;     // Explicit assignment of null
let userObject: { name: string } | null = null; // Can be an object or null

console.log(undefinedValue); // Output: undefined
console.log(typeof undefinedValue); // Output: undefined

console.log(nullValue); // Output: null
console.log(typeof nullValue); // Output: object (Be careful with this!)

// Use case: function with optional parameter
function greet(name?: string) {
  if (name === undefined) {
    console.log("Hello, guest!");
  } else {
    console.log(`Hello, ${name}!`);
  }
}

greet(); // Output: Hello, guest!
greet("Bob"); // Output: Hello, Bob!

// Use case: API that might return a user or nothing
function findUser(id: number): { name: string } | null {
  if (id === 1) {
    return { name: "Alice" };
  }
  return null; // Explicitly indicates not found
}

userObject = findUser(1);
console.log(userObject); // Output: { name: 'Alice' }
userObject = findUser(2);
console.log(userObject); // Output: null
```

[↑ Back to Top](#toc-container)


### Best Practices with `null` and `undefined`

*   **Enable `strictNullChecks`**: This TypeScript compiler option (`tsconfig.json`) forces explicit handling of cases where a variable might be `null` or `undefined`, preventing common runtime errors. It's one of TypeScript's most valuable features.

{% hint style="info" %}
Enabling `strictNullChecks` is **highly recommended** in any TypeScript project. Although it requires more explicit handling of nulls and undefineds, it drastically reduces runtime errors related to null values (`null pointer exceptions`).
{% endhint %}

*   **Prefer `undefined` for Optional Values**: For optional function parameters or object properties, `undefined` is generally more idiomatic in TypeScript/JavaScript.
*   **Be Explicit with `null`**: Use `null` when you want to clearly and deliberately indicate the absence of an object.

[↑ Back to Top](#toc-container)


### Bad Practices

*   **Using `null` Indiscriminately**: Avoid using `null` where `undefined` would be more appropriate (e.g., to simply indicate that a variable hasn't been initialized).
*   **Ignoring `strictNullChecks`**: Disabling this option or ignoring its warnings can lead to `Cannot read property 'x' of null/undefined` errors in production.

{% hint style="danger" %}
Ignoring or disabling `strictNullChecks` introduces a significant risk of runtime errors that are difficult to detect during development. It is a bad practice that compromises the type safety provided by TypeScript.
{% endhint %}

[↑ Back to Top](#toc-container)


---

## Handling Large Numbers: `bigint`

JavaScript (and therefore TypeScript) has limitations in the precision of large integers due to the use of the `number` format (64-bit IEEE 754). The `number` type can safely represent integers up to \(2^{53} - 1\) (accessible via `Number.MAX_SAFE_INTEGER`). To operate with larger integers, the `bigint` type was introduced.

### `bigint`

`bigint`s are created by appending an `n` to the end of an integer literal or by calling the `BigInt()` function.

#### Example: Using `bigint`

```typescript
const veryLargeNumber: bigint = 9007199254740991n; // Safe limit for number
const evenLargerNumber: bigint = 9007199254740992n; // Exceeds safe limit for number
const fromFunction: bigint = BigInt("12345678901234567890"); // Using BigInt function

console.log(veryLargeNumber + 1n); // bigint arithmetic requires 'n'
// console.log(veryLargeNumber + 1); // Error: Operator '+' cannot be applied to types 'bigint' and 'number'.

console.log(evenLargerNumber);
console.log(fromFunction);
```

[↑ Back to Top](#toc-container)


### Important Considerations about `bigint`

*   **Cannot Mix with `number`**: You cannot directly perform arithmetic operations between `bigint` and `number` types. You must explicitly convert one type to the other (with potential loss of precision if converting `bigint` to `number`).
*   **No Decimals**: `bigint` can only represent whole integers. Division operations will truncate any fractional part.
*   **Compatibility**: Not all JavaScript environments or older browsers support `bigint`. Check compatibility (e.g., via `tsconfig.json`'s `target` option or tools like Can I use...) if targeting older platforms.

#### Example: Mixing `number` and `bigint` (Error and Correction)

```typescript
let num: number = 100;
let big: bigint = 50n;

// console.log(num + big); // Error! Cannot mix types.

// Correct way: Explicit conversion
console.log(BigInt(num) + big); // Convert number to bigint -> 150n
console.log(num + Number(big)); // Convert bigint to number -> 150 (safe here, but risks precision loss for large bigints)
```

[↑ Back to Top](#toc-container)


### Best Practices with `bigint`

*   **Use When Necessary**: Use `bigint` only when dealing with integers that are guaranteed to exceed `Number.MAX_SAFE_INTEGER` or `Number.MIN_SAFE_INTEGER`.
*   **Be Explicit in Conversions**: When mixing with `number`, be deliberate about which type you convert to, understanding the implications (especially potential precision loss when converting `bigint` to `number`).
*   **Check Environment Support**: Ensure your target environment supports `bigint` if you plan to use it.

[↑ Back to Top](#toc-container)


### Bad Practices

*   **Unnecessary Use**: Using `bigint` for numbers that fit safely within the `number` range adds unnecessary complexity.
*   **Implicit Mixing**: Relying on implicit conversions or ignoring type errors when mixing `bigint` and `number`.
*   **Ignoring Precision Loss**: Converting large `bigint` values to `number` without considering the potential loss of precision.

[↑ Back to Top](#toc-container)