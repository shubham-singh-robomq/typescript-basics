# Interfaces in TypeScript

Interfaces in TypeScript are powerful tools for defining contracts within your code and with external code. This guide covers how to define and use interfaces in TypeScript.

## Basic Interface Syntax

```typescript
interface Person {
    name: string;
    age: number;
    greet(): string;
}
```

## Optional Properties

```typescript
interface Person {
    name: string;
    age?: number;  // Optional property
}
```

## Readonly Properties

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
```

## Function Types

```typescript
interface SearchFunc {
    (source: string, subString: string): boolean;
}

const mySearch: SearchFunc = function(source: string, subString: string) {
    return source.search(subString) > -1;
};
```

## Indexable Types

```typescript
interface StringArray {
    [index: number]: string;
}

const myArray: StringArray = ["Bob", "Fred"];
```

## Class Types

```typescript
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date): void;
}

class Clock implements ClockInterface {
    currentTime: Date = new Date();
    setTime(d: Date) {
        this.currentTime = d;
    }
}
```

## Extending Interfaces

```typescript
interface Shape {
    color: string;
}

interface Square extends Shape {
    sideLength: number;
}
```

## Hybrid Types

```typescript
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    let counter = function (start: number) { } as Counter;
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}
```

## Generic Interfaces

```typescript
interface GenericIdentityFn<T> {
    (arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
```

## Interface vs Type Alias

```typescript
// Interface
interface Point {
    x: number;
    y: number;
}

// Type Alias
type Point = {
    x: number;
    y: number;
};
```

## Declaration Merging

```typescript
interface Box {
    height: number;
    width: number;
}

interface Box {
    scale: number;
}

let box: Box = { height: 5, width: 6, scale: 10 };
```

## Best Practices

1. **Use interfaces for object shapes**: When defining the shape of objects
2. **Use type aliases for unions**: When creating union types
3. **Use interfaces for class contracts**: When defining class contracts
4. **Use interfaces for declaration merging**: When you need to extend existing types
5. **Use interfaces for library definitions**: When creating type definitions for libraries

## Common Patterns

### Factory Pattern with Interfaces

```typescript
interface Animal {
    makeSound(): void;
}

class Dog implements Animal {
    makeSound(): void {
        console.log('Woof!');
    }
}

class Cat implements Animal {
    makeSound(): void {
        console.log('Meow!');
    }
}

function createAnimal(type: 'dog' | 'cat'): Animal {
    switch (type) {
        case 'dog':
            return new Dog();
        case 'cat':
            return new Cat();
        default:
            throw new Error('Invalid animal type');
    }
}
```

### Strategy Pattern with Interfaces

```typescript
interface PaymentStrategy {
    pay(amount: number): void;
}

class CreditCardPayment implements PaymentStrategy {
    pay(amount: number): void {
        console.log(`Paying ${amount} with credit card`);
    }
}

class PayPalPayment implements PaymentStrategy {
    pay(amount: number): void {
        console.log(`Paying ${amount} with PayPal`);
    }
}

class PaymentProcessor {
    private strategy: PaymentStrategy;

    constructor(strategy: PaymentStrategy) {
        this.strategy = strategy;
    }

    processPayment(amount: number): void {
        this.strategy.pay(amount);
    }
}
```

### Observer Pattern with Interfaces

```typescript
interface Observer {
    update(data: any): void;
}

interface Subject {
    registerObserver(observer: Observer): void;
    removeObserver(observer: Observer): void;
    notifyObservers(): void;
}

class WeatherStation implements Subject {
    private observers: Observer[] = [];
    private temperature: number = 0;

    registerObserver(observer: Observer): void {
        this.observers.push(observer);
    }

    removeObserver(observer: Observer): void {
        const index = this.observers.indexOf(observer);
        if (index > -1) {
            this.observers.splice(index, 1);
        }
    }

    notifyObservers(): void {
        this.observers.forEach(observer => observer.update(this.temperature));
    }

    setTemperature(temp: number): void {
        this.temperature = temp;
        this.notifyObservers();
    }
}
```

## Next Steps

Now that you understand interfaces, you can learn about:
- [Generics](./generics.md)
- [Modules](./modules.md)
- [Advanced Types](../advanced/advanced-types.md) 