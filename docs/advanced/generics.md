# Generics in TypeScript

Generics allow you to create reusable components that can work with a variety of types rather than a single one. This guide covers how to use generics in TypeScript.

## Basic Generic Functions

```typescript
function identity<T>(arg: T): T {
    return arg;
}

let output = identity<string>("myString");
let output2 = identity("myString"); // Type inference
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

## Generic Classes

```typescript
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```

## Generic Constraints

```typescript
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}
```

## Using Type Parameters in Generic Constraints

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };
getProperty(x, "a");
getProperty(x, "m"); // Error
```

## Generic Classes with Constraints

```typescript
class BeeKeeper {
    hasMask: boolean = true;
}

class ZooKeeper {
    nametag: string = "Mikle";
}

class Animal {
    numLegs: number = 4;
}

class Bee extends Animal {
    keeper: BeeKeeper = new BeeKeeper();
}

class Lion extends Animal {
    keeper: ZooKeeper = new ZooKeeper();
}

function createInstance<A extends Animal>(c: new () => A): A {
    return new c();
}

createInstance(Lion).keeper.nametag;
createInstance(Bee).keeper.hasMask;
```

## Generic Utility Types

### Partial

```typescript
interface Todo {
    title: string;
    description: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
    return { ...todo, ...fieldsToUpdate };
}
```

### Readonly

```typescript
interface Todo {
    title: string;
}

const todo: Readonly<Todo> = {
    title: "Delete inactive users",
};

todo.title = "Hello"; // Error
```

### Record

```typescript
interface PageInfo {
    title: string;
}

type Page = "home" | "about" | "contact";

const nav: Record<Page, PageInfo> = {
    about: { title: "about" },
    contact: { title: "contact" },
    home: { title: "home" },
};
```

### Pick

```typescript
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
    title: "Clean room",
    completed: false,
};
```

### Omit

```typescript
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}

type TodoPreview = Omit<Todo, "description">;

const todo: TodoPreview = {
    title: "Clean room",
    completed: false,
};
```

## Best Practices

1. **Use generics for reusable components**: When you need to work with multiple types
2. **Use constraints**: To limit the types that can be used with generics
3. **Use type inference**: When possible, let TypeScript infer generic types
4. **Use utility types**: For common type transformations
5. **Document generic types**: Add comments explaining the purpose of generic parameters

## Common Patterns

### Generic Repository Pattern

```typescript
interface Repository<T> {
    findById(id: string): Promise<T>;
    findAll(): Promise<T[]>;
    save(entity: T): Promise<void>;
    delete(id: string): Promise<void>;
}

class UserRepository implements Repository<User> {
    async findById(id: string): Promise<User> {
        // Implementation
    }
    // ... other methods
}
```

### Generic Factory Pattern

```typescript
interface Factory<T> {
    create(): T;
}

class CarFactory implements Factory<Car> {
    create(): Car {
        return new Car();
    }
}

class BikeFactory implements Factory<Bike> {
    create(): Bike {
        return new Bike();
    }
}
```

### Generic Builder Pattern

```typescript
class Builder<T> {
    private value: Partial<T> = {};

    set<K extends keyof T>(key: K, value: T[K]): this {
        this.value[key] = value;
        return this;
    }

    build(): T {
        return this.value as T;
    }
}

interface User {
    name: string;
    age: number;
    email: string;
}

const user = new Builder<User>()
    .set("name", "John")
    .set("age", 30)
    .set("email", "john@example.com")
    .build();
```

## Next Steps

Now that you understand generics, you can learn about:
- [Modules](./modules.md)
- [Advanced Types](./advanced-types.md)
- [Decorators](./decorators.md) 