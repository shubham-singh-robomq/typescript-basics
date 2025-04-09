# Functions in TypeScript

Functions are a fundamental building block in TypeScript. This guide covers how to define and use functions with TypeScript's type system.

## Basic Function Syntax

```typescript
function greet(name: string): string {
    return `Hello, ${name}!`;
}
```

## Function Types

### Named Functions

```typescript
function add(x: number, y: number): number {
    return x + y;
}
```

### Function Expressions

```typescript
const multiply = function(x: number, y: number): number {
    return x * y;
};
```

### Arrow Functions

```typescript
const divide = (x: number, y: number): number => x / y;
```

## Optional Parameters

```typescript
function buildName(firstName: string, lastName?: string): string {
    return lastName ? `${firstName} ${lastName}` : firstName;
}
```

## Default Parameters

```typescript
function buildName(firstName: string, lastName: string = "Smith"): string {
    return `${firstName} ${lastName}`;
}
```

## Rest Parameters

```typescript
function sum(...numbers: number[]): number {
    return numbers.reduce((a, b) => a + b, 0);
}
```

## Function Overloads

```typescript
function makeDate(timestamp: number): Date;
function makeDate(m: number, d: number, y: number): Date;
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
    if (d !== undefined && y !== undefined) {
        return new Date(y, mOrTimestamp, d);
    } else {
        return new Date(mOrTimestamp);
    }
}
```

## Callback Functions

```typescript
function fetchData(callback: (data: string) => void): void {
    // Simulate fetching data
    setTimeout(() => {
        callback("Data received");
    }, 1000);
}

fetchData((data) => {
    console.log(data);
});
```

## Higher-Order Functions

```typescript
function multiplyBy(factor: number): (x: number) => number {
    return (x: number) => x * factor;
}

const double = multiplyBy(2);
console.log(double(5)); // 10
```

## Async Functions

```typescript
async function fetchUserData(userId: string): Promise<User> {
    const response = await fetch(`/api/users/${userId}`);
    return response.json();
}
```

## Generator Functions

```typescript
function* generateSequence(): Generator<number> {
    yield 1;
    yield 2;
    yield 3;
}

const generator = generateSequence();
console.log(generator.next().value); // 1
console.log(generator.next().value); // 2
console.log(generator.next().value); // 3
```

## Function Type Aliases

```typescript
type BinaryOperation = (x: number, y: number) => number;

const add: BinaryOperation = (x, y) => x + y;
const subtract: BinaryOperation = (x, y) => x - y;
```

## Best Practices

1. **Use explicit return types**: For complex functions
2. **Use arrow functions**: For callbacks and methods
3. **Use async/await**: For asynchronous operations
4. **Use function overloads**: When a function can be called in multiple ways
5. **Use type aliases**: For complex function types
6. **Use default parameters**: Instead of optional parameters when possible
7. **Use rest parameters**: For functions with variable arguments

## Common Patterns

### Memoization

```typescript
function memoize<T>(fn: (arg: T) => T): (arg: T) => T {
    const cache = new Map<T, T>();
    return (arg: T): T => {
        if (cache.has(arg)) {
            return cache.get(arg)!;
        }
        const result = fn(arg);
        cache.set(arg, result);
        return result;
    };
}
```

### Currying

```typescript
function curry<T, U, V>(fn: (x: T, y: U) => V): (x: T) => (y: U) => V {
    return (x: T) => (y: U) => fn(x, y);
}

const add = (x: number, y: number) => x + y;
const curriedAdd = curry(add);
const addFive = curriedAdd(5);
console.log(addFive(3)); // 8
```

### Composition

```typescript
function compose<T, U, V>(f: (x: U) => V, g: (x: T) => U): (x: T) => V {
    return (x: T) => f(g(x));
}

const double = (x: number) => x * 2;
const square = (x: number) => x * x;
const doubleThenSquare = compose(square, double);
console.log(doubleThenSquare(3)); // 36
```

## Next Steps

Now that you understand functions, you can learn about:
- [Classes](../advanced/classes.md)
- [Interfaces](../advanced/interfaces.md)
- [Generics](../advanced/generics.md) 