# Advanced Types in TypeScript

This guide covers advanced type features in TypeScript that help you create more precise and flexible type definitions.

## Union Types

Union types allow a value to be one of several types:

```typescript
type Status = 'active' | 'inactive' | 'pending';
type ID = string | number;

function processId(id: ID) {
    if (typeof id === 'string') {
        return id.toUpperCase();
    }
    return id.toString();
}
```

## Intersection Types

Intersection types combine multiple types into one:

```typescript
interface Person {
    name: string;
    age: number;
}

interface Employee {
    employeeId: string;
    department: string;
}

type EmployeePerson = Person & Employee;

const john: EmployeePerson = {
    name: 'John',
    age: 30,
    employeeId: 'E123',
    department: 'Engineering'
};
```

## Type Guards

Type guards help narrow down types within conditional blocks:

```typescript
function isString(value: unknown): value is string {
    return typeof value === 'string';
}

function processValue(value: string | number) {
    if (isString(value)) {
        return value.toUpperCase();
    }
    return value.toFixed(2);
}
```

## Mapped Types

Mapped types create new types by transforming properties of existing types:

```typescript
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

type Optional<T> = {
    [P in keyof T]?: T[P];
};

interface User {
    name: string;
    age: number;
}

type ReadonlyUser = Readonly<User>;
type OptionalUser = Optional<User>;
```

## Conditional Types

Conditional types select one of two possible types based on a condition:

```typescript
type NonNullable<T> = T extends null | undefined ? never : T;

type StringOrNumber<T> = T extends string ? string : number;

type Result1 = StringOrNumber<string>;  // string
type Result2 = StringOrNumber<boolean>; // number
```

## Utility Types

TypeScript provides several built-in utility types:

```typescript
// Partial<T> - Makes all properties optional
type PartialUser = Partial<User>;

// Required<T> - Makes all properties required
type RequiredUser = Required<PartialUser>;

// Pick<T, K> - Selects specific properties
type UserName = Pick<User, 'name'>;

// Omit<T, K> - Removes specific properties
type UserWithoutAge = Omit<User, 'age'>;

// Record<K, T> - Creates an object type with specified keys and value type
type UserMap = Record<string, User>;
```

## Template Literal Types

Template literal types allow you to create types based on string patterns:

```typescript
type CSSUnit = 'px' | 'em' | 'rem' | '%';
type CSSValue = `${number}${CSSUnit}`;

const width: CSSValue = '100px';  // Valid
const height: CSSValue = '2em';   // Valid
const margin: CSSValue = '50%';   // Valid
```

## Index Signatures

Index signatures allow you to define the types of properties that aren't known at compile time:

```typescript
interface StringArray {
    [index: number]: string;
}

const arr: StringArray = ['a', 'b', 'c'];

interface Dictionary {
    [key: string]: number;
}

const dict: Dictionary = {
    'one': 1,
    'two': 2
};
```

