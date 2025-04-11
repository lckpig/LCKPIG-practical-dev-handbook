<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/typescript/types)
<!-- MULTILANGUAJE MENU END -->

# Tipos en TypeScript

## Introducción

TypeScript es un lenguaje de programación que extiende JavaScript añadiendo tipos estáticos. Los tipos son una característica fundamental que nos permite definir la forma de los datos y mejorar la calidad del código.

## Tipos Básicos

### Tipos Primitivos

- `boolean`: Representa valores verdaderos o falsos
- `number`: Representa números, incluyendo enteros y decimales
- `string`: Representa texto
- `null` y `undefined`: Representan la ausencia de valor

### Ejemplos de Tipos Primitivos

```typescript
let isActive: boolean = true;
let age: number = 25;
let name: string = "Juan";
let nothing: null = null;
let notDefined: undefined = undefined;
```

## Tipos Compuestos

### Arrays

```typescript
let numbers: number[] = [1, 2, 3];
let names: string[] = ["Ana", "Juan", "María"];
```

### Tuplas

```typescript
let person: [string, number] = ["Juan", 25];
```

### Enums

```typescript
enum Color {
  Red,
  Green,
  Blue
}
```

## Tipos Personalizados

### Interfaces

```typescript
interface Person {
  name: string;
  age: number;
  isActive?: boolean; // Propiedad opcional
}
```

### Type Aliases

```typescript
type Point = {
  x: number;
  y: number;
};
```

## Tipos Avanzados

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

## Buenas Prácticas

1. Usar tipos específicos en lugar de `any`
2. Definir interfaces para estructuras de datos complejas
3. Utilizar tipos genéricos para código reutilizable
4. Implementar type guards para validaciones de tipo
5. Documentar tipos complejos con comentarios 