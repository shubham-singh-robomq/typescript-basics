# Classes in TypeScript

TypeScript's class system is based on ES6 classes with additional type annotations and features. This guide covers how to define and use classes in TypeScript.

## Basic Class Syntax

```typescript
class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    greet(): string {
        return `Hello, my name is ${this.name} and I am ${this.age} years old.`;
    }
}
```

## Access Modifiers

### Public (default)

```typescript
class Person {
    public name: string;
    public age: number;
}
```

### Private

```typescript
class Person {
    private name: string;
    private age: number;
}
```

### Protected

```typescript
class Person {
    protected name: string;
    protected age: number;
}
```

## Readonly Properties

```typescript
class Person {
    readonly name: string;
    readonly age: number;
}
```

## Static Members

```typescript
class MathHelper {
    static PI: number = 3.14159;
    
    static calculateArea(radius: number): number {
        return this.PI * radius * radius;
    }
}
```

## Inheritance

```typescript
class Animal {
    constructor(public name: string) {}
    
    move(distance: number = 0): void {
        console.log(`${this.name} moved ${distance}m.`);
    }
}

class Dog extends Animal {
    constructor(name: string) {
        super(name);
    }
    
    bark(): void {
        console.log('Woof! Woof!');
    }
}
```

## Abstract Classes

```typescript
abstract class Animal {
    abstract makeSound(): void;
    
    move(): void {
        console.log('Moving...');
    }
}

class Dog extends Animal {
    makeSound(): void {
        console.log('Woof!');
    }
}
```

## Interfaces with Classes

```typescript
interface IPerson {
    name: string;
    age: number;
    greet(): string;
}

class Person implements IPerson {
    constructor(public name: string, public age: number) {}
    
    greet(): string {
        return `Hello, my name is ${this.name}`;
    }
}
```

## Getters and Setters

```typescript
class Person {
    private _age: number;
    
    get age(): number {
        return this._age;
    }
    
    set age(value: number) {
        if (value < 0) {
            throw new Error('Age cannot be negative');
        }
        this._age = value;
    }
}
```

## Class Decorators

```typescript
function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}

@sealed
class Person {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
}
```

## Method Decorators

```typescript
function log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    
    descriptor.value = function(...args: any[]) {
        console.log(`Calling ${propertyKey} with args:`, args);
        return originalMethod.apply(this, args);
    };
    
    return descriptor;
}

class Calculator {
    @log
    add(x: number, y: number): number {
        return x + y;
    }
}
```

## Property Decorators

```typescript
function format(formatString: string) {
    return function(target: any, propertyKey: string) {
        let value = target[propertyKey];
        
        const getter = function() {
            return value;
        };
        
        const setter = function(newVal: string) {
            value = formatString.replace('%s', newVal);
        };
        
        Object.defineProperty(target, propertyKey, {
            get: getter,
            set: setter,
            enumerable: true,
            configurable: true
        });
    };
}

class Person {
    @format('Hello, %s!')
    greeting: string;
}
```

## Best Practices

1. **Use access modifiers**: Explicitly declare public, private, or protected
2. **Use readonly**: For properties that shouldn't change after initialization
3. **Use interfaces**: To define contracts for classes
4. **Use abstract classes**: For base classes with common functionality
5. **Use decorators**: For cross-cutting concerns
6. **Use getters/setters**: For computed properties or validation
7. **Use static members**: For utility methods and constants

## Common Patterns

### Singleton Pattern

```typescript
class Singleton {
    private static instance: Singleton;
    private constructor() {}
    
    static getInstance(): Singleton {
        if (!Singleton.instance) {
            Singleton.instance = new Singleton();
        }
        return Singleton.instance;
    }
}
```

### Factory Pattern

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

class AnimalFactory {
    static createAnimal(type: 'dog' | 'cat'): Animal {
        switch (type) {
            case 'dog':
                return new Dog();
            case 'cat':
                return new Cat();
            default:
                throw new Error('Invalid animal type');
        }
    }
}
```

### Observer Pattern

```typescript
interface Observer {
    update(data: any): void;
}

class Subject {
    private observers: Observer[] = [];
    
    addObserver(observer: Observer): void {
        this.observers.push(observer);
    }
    
    notifyObservers(data: any): void {
        this.observers.forEach(observer => observer.update(data));
    }
}
```

## Next Steps

Now that you understand classes, you can learn about:
- [Interfaces](./interfaces.md)
- [Generics](./generics.md)
- [Modules](./modules.md) 