# Variables in TypeScript

Variables are fundamental building blocks in TypeScript. This guide covers how to declare and use variables with TypeScript's type system.

## Variable Declarations

TypeScript supports three ways to declare variables:

### `var`

```typescript
var x = 10;
```

- Function-scoped
- Hoisted
- Can be redeclared
- Avoid using in modern TypeScript

### `let`

```typescript
let x = 10;
```

- Block-scoped
- Not hoisted
- Cannot be redeclared in the same scope
- Preferred for variables that change

### `const`

```typescript
const x = 10;
```

- Block-scoped
- Not hoisted
- Cannot be reassigned
- Must be initialized
- Preferred for values that don't change

## Type Annotations

You can explicitly specify types for variables:

```typescript
let name: string = "John";
let age: number = 30;
let isActive: boolean = true;
```

## Type Inference

TypeScript can infer types from initial values:

```typescript
let name = "John"; // TypeScript infers string
let age = 30;      // TypeScript infers number
let isActive = true; // TypeScript infers boolean
```

## Variable Scope

### Global Scope

```typescript
var globalVar = "I'm global";
let globalLet = "I'm also global";
const globalConst = "I'm global too";
```

### Function Scope

```typescript
function example() {
    var functionVar = "I'm function scoped";
    let functionLet = "I'm block scoped";
    const functionConst = "I'm block scoped";
}
```

### Block Scope

```typescript
if (true) {
    var blockVar = "I'm still accessible outside";
    let blockLet = "I'm only accessible here";
    const blockConst = "I'm only accessible here";
}
console.log(blockVar); // Works
console.log(blockLet); // Error
console.log(blockConst); // Error
```

## Variable Hoisting

### `var` Hoisting

```typescript
console.log(x); // undefined
var x = 5;
console.log(x); // 5
```

### `let` and `const` Hoisting

```typescript
console.log(x); // ReferenceError
let x = 5;
```

## Destructuring

### Array Destructuring

```typescript
let [first, second] = [1, 2];
console.log(first); // 1
console.log(second); // 2

// With type annotations
let [a, b]: [number, number] = [1, 2];
```

### Object Destructuring

```typescript
let { name, age } = { name: "John", age: 30 };
console.log(name); // John
console.log(age); // 30

// With type annotations
let { name, age }: { name: string; age: number } = { name: "John", age: 30 };
```

## Default Values

```typescript
function greet(name: string = "Guest") {
    console.log(`Hello, ${name}!`);
}

greet(); // Hello, Guest!
greet("John"); // Hello, John!
```

## Best Practices

1. **Use `const` by default**: Only use `let` when you need to reassign
2. **Avoid `var`**: Use `let` or `const` instead
3. **Be explicit with types**: When type inference isn't clear
4. **Use meaningful names**: Make variable names descriptive
5. **Initialize variables**: Always initialize variables when declaring them
6. **Use destructuring**: For cleaner code when working with objects and arrays

## Common Patterns

### Swapping Variables

```typescript
let a = 1;
let b = 2;
[a, b] = [b, a];
console.log(a); // 2
console.log(b); // 1
```

### Rest Parameters

```typescript
function sum(...numbers: number[]): number {
    return numbers.reduce((a, b) => a + b, 0);
}
console.log(sum(1, 2, 3)); // 6
```

### Spread Operator

```typescript
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]
```

## Next Steps

Now that you understand variables, you can learn about:
- [Functions](./functions.md)
- [Types](./types.md)
- [Advanced Types](../advanced/interfaces.md) 