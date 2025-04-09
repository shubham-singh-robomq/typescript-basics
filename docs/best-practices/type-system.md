# TypeScript Type System Best Practices

## 1. Use Strict Type Checking

Always enable strict mode in your `tsconfig.json`:

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

## 2. Type Inference

Let TypeScript infer types when possible:

```typescript
// Good
const name = "John";  // TypeScript infers string

// Bad
const name: string = "John";  // Unnecessary type annotation
```

## 3. Use Type Aliases and Interfaces Appropriately

Use interfaces for object shapes and type aliases for unions, primitives, and tuples:

```typescript
// Good - Interface for object shape
interface User {
  id: number;
  name: string;
}

// Good - Type alias for union
type Status = 'active' | 'inactive' | 'pending';

// Good - Type alias for tuple
type Point = [number, number];
```

## 4. Avoid Using `any`

Use more specific types instead of `any`:

```typescript
// Bad
function processData(data: any) {
  // ...
}

// Good
function processData(data: unknown) {
  if (typeof data === 'string') {
    // ...
  }
}
```

## 5. Use Generics for Reusable Components

```typescript
// Good
function identity<T>(value: T): T {
  return value;
}

// Good
interface Response<T> {
  data: T;
  status: number;
}
```

## 6. Use Union Types for Enums

Prefer union types over enums for better type safety:

```typescript
// Good
type Direction = 'north' | 'south' | 'east' | 'west';

// Bad
enum Direction {
  North,
  South,
  East,
  West
}
```

## 7. Use Type Guards

```typescript
// Good
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

function process(value: unknown) {
  if (isString(value)) {
    // value is now typed as string
    console.log(value.toUpperCase());
  }
}
```

## 8. Use Mapped Types

```typescript
// Good
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

type Partial<T> = {
  [P in keyof T]?: T[P];
};
```

## 9. Use Utility Types

```typescript
// Good
interface User {
  id: number;
  name: string;
  email: string;
}

type UserPreview = Pick<User, 'id' | 'name'>;
type UserWithoutEmail = Omit<User, 'email'>;
```

## 10. Use Const Assertions

```typescript
// Good
const colors = ['red', 'green', 'blue'] as const;
type Color = typeof colors[number];  // 'red' | 'green' | 'blue'
```

## 11. Use Non-null Assertion Operator Sparingly

```typescript
// Bad
function process(value: string | null) {
  return value!.toUpperCase();
}

// Good
function process(value: string | null) {
  if (value === null) {
    throw new Error('Value cannot be null');
  }
  return value.toUpperCase();
}
```

## 12. Use Type Predicates

```typescript
// Good
function isError(value: unknown): value is Error {
  return value instanceof Error;
}

function handleError(error: unknown) {
  if (isError(error)) {
    console.error(error.message);
  }
}
```

## 13. Use Index Signatures Carefully

```typescript
// Good
interface StringMap {
  [key: string]: string;
}

// Better - more specific
interface Config {
  [key: string]: string | number | boolean;
}
```

## 14. Use Discriminated Unions

```typescript
// Good
type Shape =
  | { kind: 'circle'; radius: number }
  | { kind: 'square'; size: number }
  | { kind: 'rectangle'; width: number; height: number };

function area(shape: Shape): number {
  switch (shape.kind) {
    case 'circle':
      return Math.PI * shape.radius ** 2;
    case 'square':
      return shape.size ** 2;
    case 'rectangle':
      return shape.width * shape.height;
  }
}
``` 