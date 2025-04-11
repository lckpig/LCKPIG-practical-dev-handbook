<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/typescript/types)
<!-- MULTILANGUAJE MENU END -->

# Types in TypeScript

LET'S TRY THIS OTHER ONE - we changed it VERY FAST CAR

## Introduction

HERE WE WILL ADD THIS PARAGRAPH TO TEST

TypeScript is a programming language that extends JavaScript by adding static types. Types are a fundamental feature that allows us to define the shape of data and improve code quality.

## Basic Types

### Primitive Types

- `boolean`: Represents true or false values
- `number`: Represents numbers, including integers and decimals
- `string`: Represents text
- `null` and `undefined`: Represent the absence of value

### Primitive Types Examples

```typescript
let isActive: boolean = true;
let age: number = 25;
let name: string = "John";
let nothing: null = null;
let notDefined: undefined = undefined;
```

## Compound Types

### Arrays

```typescript
let numbers: number[] = [1, 2, 3];
let names: string[] = ["Ana", "John", "Mary"];
```

### Tuples

```typescript
let person: [string, number] = ["John", 25];
```

### Enums

```typescript
enum Color {
  Red,
  Green,
  Blue
}
```

## Custom Types

### Interfaces

```typescript
interface Person {
  name: string;
  age: number;
  isActive?: boolean; // Optional property
}
```

### Type Aliases

```typescript
type Point = {
  x: number;
  y: number;
};
```

## Advanced Types

### Union Types

```typescript
let id: string | number;
```

### Intersection Types

```typescript
type Employee = Person & {
  employeeId: number;
};
```

### Generic Types

```typescript
function identity<T>(arg: T): T {
  return arg;
}
```

## Type Guards

```typescript
function isString(value: any): value is string {
  return typeof value === "string";
}
```

## Best Practices

1. Use specific types instead of `any`
2. Define interfaces for complex data structures
3. Use generic types for reusable code
4. Implement type guards for type validation
5. Document complex types with comments 