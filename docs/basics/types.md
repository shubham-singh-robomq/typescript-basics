# TypeScript Types

TypeScript's type system is one of its most powerful features. It allows you to catch errors during development and provides better tooling support. This guide covers the fundamental types in TypeScript.

## Basic Types

### Number

```typescript
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let big: number = 1_000_000;
```

### String

```typescript
let color: string = "blue";
color = 'red';
let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${fullName}. I'll be ${age + 1} years old next month.`;
```

### Boolean

```typescript
let isDone: boolean = false;
```

### Array

```typescript
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3]; // Generic array type
```

### Tuple

```typescript
let x: [string, number];
x = ["hello", 10]; // OK
x = [10, "hello"]; // Error
```

### Enum

```typescript
enum Color {
    Red,
    Green,
    Blue
}
let c: Color = Color.Green;

// String enums
enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT"
}
```

### Any

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false;
```

### Void

```typescript
function warnUser(): void {
    console.log("This is a warning message");
}
```

### Null and Undefined

```typescript
let u: undefined = undefined;
let n: null = null;
```

## Type Assertions

Type assertions are a way to tell the compiler "trust me, I know what I'm doing."

```typescript
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
// or
let strLength: number = (<string>someValue).length;
```

## Type Aliases

```typescript
type Point = {
    x: number;
    y: number;
};

function printCoord(pt: Point) {
    console.log("The coordinate's x value is " + pt.x);
    console.log("The coordinate's y value is " + pt.y);
}

printCoord({ x: 100, y: 100 });
```

## Union Types

```typescript
function printId(id: number | string) {
    console.log("Your ID is: " + id);
}

printId(101); // OK
printId("202"); // OK
printId({ myID: 22342 }); // Error
```

## Literal Types

```typescript
let x: "hello" = "hello";
// OK
x = "hello";
// Error
x = "howdy";

// Combine with unions
function printText(s: string, alignment: "left" | "right" | "center") {
    // ...
}
printText("Hello, world", "left");
printText("G'day, mate", "centre"); // Error
```

## Type Guards

```typescript
function isNumber(x: any): x is number {
    return typeof x === "number";
}

function isString(x: any): x is string {
    return typeof x === "string";
}

function padLeft(value: string, padding: string | number) {
    if (isNumber(padding)) {
        return Array(padding + 1).join(" ") + value;
    }
    if (isString(padding)) {
        return padding + value;
    }
    throw new Error(`Expected string or number, got '${padding}'.`);
}
```

## Type Inference

TypeScript can infer types in many cases:

```typescript
let x = 3; // TypeScript infers x is a number
let y = [0, 1, null]; // TypeScript infers y is (number | null)[]
```

## Best Practices

1. **Avoid using `any`**: Use more specific types whenever possible
2. **Use type inference**: Let TypeScript infer types when it's obvious
3. **Be explicit with complex types**: Use type annotations for complex types
4. **Use type guards**: Implement type guards for type checking
5. **Leverage union types**: Use union types for variables that can be multiple types

## Next Steps

Now that you understand basic types, you can learn about:
- [Variables](./variables.md)
- [Functions](./functions.md)
- [Advanced Types](../advanced/interfaces.md) 