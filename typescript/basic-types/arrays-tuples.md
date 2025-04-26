<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/typescript/basic-types/arrays-tuples)
<!-- MULTILANGUAJE MENU END -->

<!--
# Arrays and Tuples in TypeScript
- Array declaration (`number[]`, `Array<string>`)
- Using tuples (`[string, number]`)
- Tuples with labels (`[id: number, name: string]`)
-->

<details id="toc-container">
<summary>Table of Contents</summary>

<!-- no toc -->
- [Array Declaration (`number[]`, `Array<string>`)](#array-declaration-number-arraystring)
  - [Definition and Syntax](#definition-and-syntax)
  - [Real and Recommended Use Cases](#real-and-recommended-use-cases)
  - [Important Considerations](#important-considerations)
  - [Best Practices](#best-practices)
  - [Bad Practices](#bad-practices)
  - [Common Errors and Pitfalls](#common-errors-and-pitfalls)
- [Using Tuples (`[string, number]`)](#using-tuples-string-number)
  - [Definition and Syntax](#definition-and-syntax-1)
  - [Advanced Syntax Features](#advanced-syntax-features)
  - [Comparison: Arrays vs. Tuples](#comparison-arrays-vs-tuples)
  - [Real and Recommended Use Cases](#real-and-recommended-use-cases-1)
  - [Important Considerations](#important-considerations-1)
  - [Best Practices](#best-practices-1)
  - [Bad Practices](#bad-practices-1)
  - [Common Errors and Pitfalls](#common-errors-and-pitfalls-1)
- [Tuples with Labels (`[id: number, name: string]`)](#tuples-with-labels-id-number-name-string)
  - [Definition and Syntax](#definition-and-syntax-2)
  - [Advantages and Use Cases](#advantages-and-use-cases)
  - [Important Considerations](#important-considerations-2)
  - [Best Practices](#best-practices-2)
  - [Bad Practices](#bad-practices-2)
  - [Common Errors and Pitfalls](#common-errors-and-pitfalls-2)

</details>

# Arrays and Tuples in TypeScript

TypeScript, being a superset of JavaScript, inherits its fundamental data structures but enhances them with a robust static type system. Among the most commonly used data collections are **arrays** and **tuples**. Although both allow storing multiple values, their purpose, structure, and behavior differ significantly. Understanding these differences is crucial for modeling data accurately, leveraging the type safety offered by TypeScript, and writing cleaner, more predictable, and maintainable code, especially in complex applications where data integrity is paramount.

---

## Array Declaration (`number[]`, `Array<string>`)

Arrays in TypeScript, just like in JavaScript, represent ordered collections of elements that can be accessed using a numeric index (base 0). The distinctive feature TypeScript provides is the ability to **specify the data type that the array can contain**. This type restriction is checked at compile time, helping prevent an entire class of errors related to handling data of unexpected types. Arrays are ideal for lists where all elements share the same type and whose size can vary dynamically during program execution.

### Definition and Syntax

TypeScript offers two main and equivalent syntaxes for declaring arrays:

1.  **Bracket Syntax (`T[]`)**: This is the most common and concise form. The element type (`T`) is specified, followed by empty brackets (`[]`).
    *   `let numbers: number[];` // Declares an array that will only contain numbers.
    *   `let names: string[];` // Declares an array that will only contain strings.
2.  **Generic Syntax (`Array<T>`)**: Uses the generic `Array` type provided by TypeScript, specifying the element type (`T`) within angle brackets (`<>`).
    *   `let readings: Array<number>;` // Equivalent to `number[]`.
    *   `let identifiers: Array<string>;` // Equivalent to `string[]`.

The choice between `T[]` and `Array<T>` is primarily a matter of stylistic preference, although the `T[]` syntax is more prevalent in the TypeScript community.

**Array Syntax Comparison Table**

| Feature         | `T[]` Syntax                 | `Array<T>` Syntax                       | Example                       | Common Preference         |
| --------------- | ---------------------------- | --------------------------------------- | ----------------------------- | ------------------------- |
| **Format**      | Type followed by `[]`        | `Array` type with generic `<T>`         | `number[]` vs `Array<number>` | `T[]`                     |
| **Conciseness** | More concise                 | Slightly more verbose                   | -                             | `T[]`                     |
| **Clarity**     | Very clear for simple types  | Can be clearer with complex/union types | `(string                      | null)[]` vs `Array<string | null>` | Subjective |
| **Usage**       | More common in the community | Less common but valid                   | -                             | `T[]`                     |

#### Detailed Examples

```typescript
// ===== Basic Declaration and Initialization =====

// Using T[] (preferred for conciseness)
let examScores: number[] = [85, 92, 78, 99]; // Initialized array of numbers
let participantNames: string[] = ["Ana", "Luis", "Elena"]; // Array of strings
let tasksCompleted: boolean[] = [true, false, true, true]; // Array of booleans

// Using Array<T>
let sensorMeasurements: Array<number> = [12.5, 13.1, 12.8]; // Array of numbers
let productCategories: Array<string> = ["Electronics", "Clothing", "Home"]; // Array of strings

// ===== Arrays with Union Types =====
// Allows elements of specific mixed types
let mixedIdentifiers: (string | number)[] = ["ID-123", 456, "ID-789", 101];
// let mixedError: (string | number)[] = [true]; // Error: Type 'boolean' is not assignable to type 'string | number'.

// ===== Arrays of Objects =====
// Very common for lists of structured data
interface User {
  id: number;
  name: string;
  active: boolean;
}

let userList: User[] = [
  { id: 1, name: "Carlos", active: true },
  { id: 2, name: "Marta", active: false },
  { id: 3, name: "Javier", active: true }
];

// Accessing properties
console.log(userList[0].name); // Output: Carlos

// ===== Multidimensional Arrays =====
// Arrays containing other arrays
let numberMatrix: number[][] = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

console.log(numberMatrix[1][1]); // Output: 5 (accesses the second array, then the second element)

// ===== The 'any' type (TO AVOID) =====
// Allows any type, losing type safety
let untypedData: any[] = [1, "hello", true, { key: "value" }, null];
// No compile-time errors, but potential runtime errors
// untypedData[0].toUpperCase(); // Runtime error: toUpperCase is not a function
```

[↑ Back to Top](#toc-container)


### Real and Recommended Use Cases

Arrays are a versatile tool with applications in numerous development scenarios:

*   **Managing Lists of Entities:** Storing and manipulating collections of similar objects, such as a list of registered users, products in a shopping cart, comments on a post, etc. Type restriction ensures that all objects in the list share a common structure (defined by an interface or class).
*   **Results from Database Queries or APIs:** Responses from REST APIs or database queries frequently return lists of records. Typing these arrays (`Product[]`, `Order[]`) allows working with the data safely and predictably.
*   **Storing Data from Dynamic Forms:** In forms where the user can add multiple entries of the same type (e.g., adding several phone numbers, email addresses), an array is ideal for storing these values.
*   **Batch Data Processing:** Performing operations on datasets, such as calculating the average of a list of scores (`number[]`), filtering a list of email addresses (`string[]`), or transforming a list of objects.
*   **Implementing Data Structures:** Arrays are the foundation for implementing other data structures like stacks, queues, or linked lists (although the latter are often implemented differently to optimize insertions/deletions).

[↑ Back to Top](#toc-container)


### Important Considerations

*   **Type Safety vs. Flexibility:** The main advantage is type safety. TypeScript verifies that only elements of the specified type are added. If you need flexibility to store different types, consider **union types** (`(string | number)[]`) which offer controlled flexibility, instead of resorting to `any[]` which disables type checking.
*   **Performance of Operations:** Arrays in JavaScript/TypeScript are dynamic. Operations like `push` (add to end) and `pop` (remove from end) are generally fast (O(1) amortized). However, inserting or deleting elements at the beginning or middle (`unshift`, `splice`) can be costly (O(n)) in large arrays, as it requires rearranging the indices of subsequent elements. For scenarios with many insertions/deletions in the middle, other structures like linked lists might be more efficient, although less common in day-to-day JavaScript/TypeScript development.
*   **Immutability and `readonly`:** By default, arrays are mutable (can be modified after creation). If you need to ensure an array is not modified, TypeScript offers the `readonly` modifier:
    ```typescript
    let fixedNumbers: readonly number[] = [10, 20, 30];
    // fixedNumbers.push(40); // Error: Property 'push' does not exist on type 'readonly number[]'.
    // fixedNumbers[0] = 5;   // Error: Index signature in type 'readonly number[]' only permits reading.

    // Also works with the generic syntax
    let fixedStrings: ReadonlyArray<string> = ["a", "b", "c"];
    ```
    Using `readonly` is an excellent practice in functional programming or when passing arrays to functions that should not alter them, preventing unexpected side effects. Note that `readonly` only prevents reassignment or direct modification of the array; if the array contains objects, the properties of those objects *can* still be modified (it's shallow immutability).
*   **Array Comparison:** Comparing arrays with `==` or `===` checks for reference equality (if they point to the same object in memory), not content equality. To compare content, you need to iterate and compare each element or use external libraries.
    ```typescript
    let arr1 = [1, 2];
    let arr2 = [1, 2];
    let arr3 = arr1;
    console.log(arr1 === arr2); // false (different objects in memory)
    console.log(arr1 === arr3); // true (same reference)
    ```
*   **Mutating vs. Non-Mutating Methods:** It is crucial to distinguish between methods that modify the original array and those that return a new one. Not being clear about this difference is a common source of errors.

[↑ Back to Top](#toc-container)


### Best Practices

*   **Specify the type explicitly:** Always define the element type (`number[]`, `User[]`) to maximize safety and clarity. Prevent TypeScript from inferring `any[]`.
*   **Prefer `T[]`:** Use the `T[]` syntax as it's more concise and common, unless team conventions dictate otherwise.
*   **Use `readonly` when immutability is desired:** Apply `readonly T[]` or `ReadonlyArray<T>` for arrays that shouldn't change after creation, improving code predictability.
*   **Initialize arrays upon declaration:** Avoid declaring arrays without initializing (`let myArray: number[];`) if possible, as they will have the value `undefined` until assigned an array, which can lead to errors if used prematurely. Prefer `let myArray: number[] = [];`.
*   **Use functional methods for transformations:** Prefer methods like `map`, `filter`, `reduce` to create new arrays based on the original instead of modifying it directly, especially if aiming for a more functional programming style or needing immutability.

[↑ Back to Top](#toc-container)


### Bad Practices

*   **Overusing `any[]`:** Using `any[]` indiscriminately defeats the main advantage of TypeScript. If you need mixed types, use union types (`(string | number | boolean)[]`) or define appropriate interfaces/types if the structure is more complex.
*   **Unexpected mutation of arrays passed as arguments:** If a function receives an array and modifies it without the caller expecting it, it can cause hard-to-debug side effects. Consider using `readonly` on function parameters or cloning the array (`[...myArray]`, `myArray.slice()`) before modifying it within the function.
*   **Sorting numbers without a compare function:** Relying on the default `sort()` for numbers (`[1, 10, 2].sort()`) produces incorrect order (`[1, 10, 2]`) because it sorts lexicographically (like strings). Always provide a compare function: `numbers.sort((a, b) => a - b);`.
*   **Ignoring that methods like `map`, `filter`, `slice` return *new* arrays:** Expecting `myArray.filter(...)` to modify `myArray` is a common mistake. These methods create and return a new array with the results. Assign the result to a new variable or the same one if you want to overwrite it: `filteredArray = myArray.filter(...)`.
*   **Creating sparse arrays unnecessarily:** Although JavaScript allows arrays with "holes" (`let a = []; a[0] = 1; a[100] = 100;`), they are often less efficient and more error-prone than dense arrays. In TypeScript, it's better to avoid them unless there's a very specific reason.

[↑ Back to Top](#toc-container)


### Common Errors and Pitfalls

*   **Accessing Out-of-Bounds Indices (`undefined`):** TypeScript **does not** prevent accessing an index that doesn't exist in the array at compile time. `myArray[invalidIndex]` will return `undefined` at runtime. This can cause errors if subsequent code expects a valid value. Always check bounds or handle the possibility of `undefined`, especially with dynamically calculated indices.
    ```typescript
    let names = ["Ana", "Luis"];
    console.log(names[2]); // undefined (no compile error)
    // console.log(names[2].toUpperCase()); // TypeError at runtime!
    ```
	 
{% hint style="warning" %}
Enabling the `noUncheckedIndexedAccess` option in `tsconfig.json` can help. With this option, accessing an array index or object property is considered potentially `undefined` (`string | undefined`), forcing explicit handling.
{% endhint %}

*   **Mutation by Reference:** When you assign an array to another variable (`let copy = original;`), both variables point to the *same* array in memory. Modifying `copy` will also affect `original`, and vice versa. To create an independent (shallow) copy, use the spread operator (`[...original]`) or `slice()` (`original.slice()`).
    ```typescript
    let original = [1, 2];
    let reference = original;
    let shallowCopy = [...original];

    reference.push(3);
    console.log(original); // [1, 2, 3] (modified!)
    console.log(reference); // [1, 2, 3]

    shallowCopy.push(4);
    console.log(original); // [1, 2, 3] (unaffected by the copy)
    console.log(shallowCopy); // [1, 2, 3, 4]
    ```
*   **Confusion between mutating vs. non-mutating methods:** Forgetting which methods modify the original array (`push`, `pop`, `splice`, `sort`, `reverse`, `fill`, `copyWithin`) and which return a new one (`map`, `filter`, `slice`, `concat`, `reduce`) is a common source of bugs. Consult the documentation if unsure.

[↑ Back to Top](#toc-container)


---

## Using Tuples (`[string, number]`)

Tuples are a TypeScript addition that doesn't exist directly in JavaScript (though they compile to JavaScript arrays). They represent an **array with a fixed number of elements, where the type of each element is strictly defined according to its position (index)**. They are useful for representing data structures where the order and type of each element are significant and known beforehand, such as a key-value pair, coordinates, or values returned by a function.

### Definition and Syntax

Tuples are defined using brackets `[]` and listing the types of each element in the exact order they must appear.

```typescript
// A tuple to represent a 2D point (x, y coordinates)
let point2D: [number, number];
point2D = [10, 20]; // Correct
// point2D = [10]; // Error: Type '[number]' is not assignable to type '[number, number]'. Source has 1 element(s) but target requires 2.
// point2D = [10, 20, 30]; // Error: Type '[number, number, number]' is not assignable to type '[number, number]'. Source has 3 element(s) but target allows only 2.
// point2D = ["10", 20]; // Error: Type 'string' is not assignable to type 'number'.

// A tuple to represent basic user info: ID (number) and Name (string)
let userInfo: [number, string];
userInfo = [123, "Alice"];

// Accessing elements by index (with specific type safety for that position)
let coordX: number = point2D[0]; // TypeScript knows the element at index 0 is 'number'
let userName: string = userInfo[1]; // TypeScript knows the element at index 1 is 'string'

// console.log(point2D[2]); // Error: Tuple type '[number, number]' of length '2' has no element at index '2'.
```

Unlike arrays, TypeScript **does** generally detect access to indices outside the defined tuple length at compile time.

[↑ Back to Top](#toc-container)


### Advanced Syntax Features

*   **Optional Elements (`?`):** You can mark elements as optional by adding `?` after their type. All optional elements must come *after* required elements.
    ```typescript
    // Optional third element (e.g., z-coordinate)
    let point3D: [number, number, number?]; 
    point3D = [10, 20];       // Correct (omitting optional z)
    point3D = [10, 20, 5];  // Correct (providing optional z)
    // point3D = [10]; // Error: Missing second element
    ```
*   **Rest Elements (`...T[]`):** Allows defining tuples that have a minimum number of specific initial elements followed by any number of elements of a certain type. The rest element must be an array or tuple type and must appear last.
    ```typescript
    // Tuple representing a function name followed by any number of arguments (strings)
    type FunctionCall = [string, ...string[]]; 

    let call1: FunctionCall = ["logMessage", "Error", "Network failed"]; // Correct
    let call2: FunctionCall = ["startProcess"];                     // Correct (no rest args)
    // let call3: FunctionCall = []; // Error: Source has 0 element(s) but target requires 1.
    // let call4: FunctionCall = ["setValue", 10]; // Error: Type 'number' is not assignable to type 'string'.
    
    // Tuple with specific start, end, and variable middle numbers
    type RangeData = [Date, ...number[], Date];
    let dataRange: RangeData = [new Date(), 10, 25, 15, new Date()]; // Correct
    ```

[↑ Back to Top](#toc-container)


### Comparison: Arrays vs. Tuples

| Feature           | Array (`T[]` or `Array<T>`)                  | Tuple (`[T1, T2, ..., Tn]`)                      |
| :---------------- | :------------------------------------------- | :----------------------------------------------- |
| **Purpose**       | Homogeneous list (same type elements)        | Heterogeneous fixed-size list (different types)  |
| **Size**          | Dynamic (can grow/shrink)                    | Fixed (defined length)                           |
| **Type Checking** | Checks if elements match the single type `T` | Checks if element at index `i` matches type `Ti` |
| **Order**         | Significant                                  | Significant (type depends on position)           |
| **Use Case**      | Lists of users, products, scores, etc.       | Key-value pairs, coordinates, function returns.  |
| **Flexibility**   | High (size, manipulation)                    | Low (fixed structure)                            |
| **Readability**   | Type indicates content (`User[]`)            | Structure indicates content (`[string, number]`) |
| **JS Equivalent** | Compiles directly to JS Array                | Compiles to JS Array (type info lost at runtime) |

[↑ Back to Top](#toc-container)


### Real and Recommended Use Cases

Tuples excel in situations where you need a fixed-size structure with elements of potentially different but known types:

*   **Representing Key-Value Pairs:** A common use case is representing entries in a map or dictionary, where the first element is the key (e.g., `string`) and the second is the value (e.g., `number`).
    ```typescript
    let entry: [string, number] = ["age", 30];
    ```
*   **Returning Multiple Values from a Function:** Instead of returning an object, a function can return a tuple if the number and types of return values are fixed and well-defined. This can be slightly more lightweight than an object, although objects are often more readable due to named properties.
    ```typescript
    function parseCoordinates(input: string): [number, number] | null {
        const parts = input.split(",");
        if (parts.length !== 2) return null;
        const lat = parseFloat(parts[0]);
        const lon = parseFloat(parts[1]);
        if (isNaN(lat) || isNaN(lon)) return null;
        return [lat, lon]; // Return tuple
    }
    
    const coords = parseCoordinates("40.71,-74.00");
    if (coords) {
        const latitude: number = coords[0];
        const longitude: number = coords[1];
        console.log(`Lat: ${latitude}, Lon: ${longitude}`);
    }
    ```
*   **Specific Data Records:** Representing data where each position has a distinct meaning, like RGB color values `[number, number, number]`, or status codes with messages `[number, string]`.
*   **Working with React Hooks:** Some React hooks like `useState` return tuples. `const [count, setCount] = useState(0);` destructures the tuple `[number, (newValue: number) => void]` returned by `useState`.

[↑ Back to Top](#toc-container)


### Important Considerations

*   **Fixed Length (Mostly):** While optional (`?`) and rest (`...`) elements add some flexibility, the core idea of a tuple is a fixed structure. Standard array methods like `push`, `pop` *can* technically be called on tuples (because they compile to arrays), but this **violates the tuple's type definition** and bypasses TypeScript's compile-time checks for length and type at specific indices beyond the defined length. It's a dangerous practice and generally discouraged.
    ```typescript
    let point: [number, number] = [10, 20];
    // point.push(30); // Technically works in JS, but TypeScript might complain or lead to unexpected types later.
                      // It violates the [number, number] contract.
    ```
    {% hint style="danger" %}
    Mutating tuple length with methods like `push` undermines their purpose. Stick to the defined length and types for safety.
    {% endhint %}
*   **Readability:** Accessing tuple elements by numeric index (`tuple[0]`, `tuple[1]`) can make code harder to read compared to accessing object properties by name (`config.host`, `user.name`). This is especially true for longer tuples.
*   **Destructuring for Readability:** Using array destructuring is highly recommended to assign meaningful names to tuple elements, improving readability significantly.
    ```typescript
    let userInfo: [number, string] = [42, "Bob"];
    
    // Less readable:
    // const userId = userInfo[0];
    // const userName = userInfo[1];
    
    // More readable (destructuring):
    const [userId, userName] = userInfo;
    console.log(`User ID: ${userId}, Name: ${userName}`); 
    ```
*   **Type Information Lost at Runtime:** Like all TypeScript type information, tuple types are erased during compilation to JavaScript. Runtime code only sees a standard JavaScript array.

[↑ Back to Top](#toc-container)


### Best Practices

*   **Use for Fixed, Heterogeneous Data:** Employ tuples when you have a small, fixed number of elements where the type of each element is known and matters based on its position.
*   **Keep Tuples Short:** Tuples are most effective when they have only a few elements (typically 2-3). Longer tuples become difficult to manage and read due to reliance on numeric indices.
*   **Prefer Destructuring:** Always use array destructuring to extract values from tuples into named variables for clarity.
*   **Use `readonly` Tuples:** If the tuple represents a fixed value that shouldn't change, declare it as `readonly` for added safety: `let point: readonly [number, number] = [10, 20];`.
*   **Consider Objects for Complex Structures:** If a tuple starts becoming long or if the meaning of each element isn't immediately obvious from its position, an object with named properties (defined via `interface` or `type`) is usually a more readable and maintainable alternative.

[↑ Back to Top](#toc-container)


### Bad Practices

*   **Using Tuples for Homogeneous Lists:** If all elements have the same type and the length is variable, use a regular array (`T[]`), not a tuple.
*   **Creating Very Long Tuples:** Avoid tuples with many elements, as accessing elements by index becomes confusing and error-prone. Use an object instead.
*   **Mutating Tuple Length:** Calling `push`, `pop`, `splice` on tuples, as it breaks the fixed-length contract enforced at compile time.
*   **Accessing Elements Solely by Index:** Relying only on numeric indices (`tuple[3]`) without destructuring makes the code hard to understand.

[↑ Back to Top](#toc-container)


### Common Errors and Pitfalls

*   **Assigning Incorrect Types or Length:** TypeScript catches these errors at compile time.
    ```typescript
    let record: [string, boolean] = ["active", 1]; // Error: Type 'number' is not assignable to type 'boolean'.
    let pair: [number, number] = [1];           // Error: Source has 1 element(s) but target requires 2.
    ```
*   **Modifying Readonly Tuples:** Attempting to change an element in a tuple declared with `readonly`.
    ```typescript
    const readOnlyPair: readonly [string, number] = ["key", 10];
    // readOnlyPair[0] = "newKey"; // Error: Cannot assign to '0' because it is a read-only property.
    ```
*   **Off-by-One Errors with Indices:** Forgetting that indices are 0-based.
*   **Losing Type Safety with `push`/`pop`:** As mentioned, mutating methods bypass tuple type checks beyond the initial length.

[↑ Back to Top](#toc-container)


---

## Tuples with Labels (`[id: number, name: string]`)

Introduced in TypeScript 4.0, labeled tuples allow assigning names to tuple elements. This feature significantly improves the readability and self-documentation of tuples, addressing one of their main drawbacks (reliance on numeric indices).

### Definition and Syntax

The syntax involves prefixing the type of each element with a label followed by a colon (`:`).

```typescript
// Standard tuple (less readable)
let userTuple: [number, string, boolean];

// Labeled tuple (more readable)
let labeledUserTuple: [id: number, name: string, isActive: boolean];

labeledUserTuple = [1, "Alice", true]; // Assignment is the same

// Destructuring still works and is recommended
const [userId, userName, userStatus] = labeledUserTuple;

console.log(`ID: ${userId}, Name: ${userName}, Active: ${userStatus}`);

// Access by index still works, but labels improve understanding
let extractedId: number = labeledUserTuple[0]; // Clearly corresponds to 'id'
let extractedName: string = labeledUserTuple[1]; // Clearly corresponds to 'name'
```

**Important:** The labels are **purely for documentation and developer experience**. They do **not** change the underlying structure (it's still an indexed array at runtime) and you **cannot** access elements using the labels like object properties (`labeledUserTuple.id` is **not** valid).

[↑ Back to Top](#toc-container)


### Advantages and Use Cases

*   **Improved Readability:** Labels make the purpose of each element in the tuple immediately clear without needing comments or relying solely on context.
*   **Enhanced Self-Documentation:** The tuple definition itself explains the meaning of each position.
*   **Better Editor Support:** Code editors can display the labels when hovering over tuple elements or during autocompletion, improving the development experience.

Labeled tuples are beneficial in the same scenarios as regular tuples, but they enhance the clarity, especially when tuples are used in function signatures or type aliases:

```typescript
// Using labeled tuples in function parameters and return types
type Point = [x: number, y: number];

function movePoint(point: Point, dx: number, dy: number): Point {
  const [currentX, currentY] = point; // Destructuring is still best
  return [currentX + dx, currentY + dy]; 
}

let startPoint: Point = [10, 5];
let endPoint = movePoint(startPoint, 3, -2); // endPoint is inferred as Point ([number, number])

console.log(endPoint); // [13, 3]
```

[↑ Back to Top](#toc-container)


### Important Considerations

*   **Labels are Compile-Time Only:** Remember that labels are erased during compilation and have no runtime effect.
*   **No Access via Label:** You must still use numeric indices or destructuring to access elements.
*   **Consistency:** If you label one element, it's good practice (though not required by the compiler) to label all elements for consistency.

[↑ Back to Top](#toc-container)


### Best Practices

*   **Use Labels for Clarity:** Add labels to your tuples whenever it improves understanding of what each element represents.
*   **Combine with Destructuring:** Use destructuring to extract values into variables with meaningful names, even if the tuple is labeled.
*   **Keep Tuples Concise:** Labeled tuples don't change the recommendation to keep tuples relatively short. Objects remain better for structures with many elements.

[↑ Back to Top](#toc-container)


### Bad Practices

*   **Assuming Runtime Access via Labels:** Trying to access elements using label names (`tuple.labelName`).
*   **Inconsistent Labeling:** Labeling only some elements in a longer tuple can sometimes be more confusing than no labels at all.

[↑ Back to Top](#toc-container)


### Common Errors and Pitfalls

*   The main pitfall is expecting labels to provide runtime property access, which they don't.
*   Syntax errors if the label format (`name: type`) is incorrect.

By understanding the specific characteristics and appropriate use cases for arrays, tuples, and labeled tuples, you can model your data more effectively and leverage TypeScript's type system for safer, more maintainable code.

[↑ Back to Top](#toc-container)

