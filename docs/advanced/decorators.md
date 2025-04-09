# TypeScript Decorators

Decorators are a special kind of declaration that can be attached to classes, methods, accessors, properties, or parameters. They use the form `@expression`, where `expression` must evaluate to a function that will be called at runtime.

## Enabling Decorators

To use decorators, you need to enable them in your `tsconfig.json`:

```json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

## Class Decorators

Class decorators are applied to the constructor of a class:

```typescript
function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}

@sealed
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}
```

## Method Decorators

Method decorators are applied to method definitions:

```typescript
function log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    
    descriptor.value = function(...args: any[]) {
        console.log(`Calling ${propertyKey} with args:`, args);
        const result = originalMethod.apply(this, args);
        console.log(`Result:`, result);
        return result;
    };
    
    return descriptor;
}

class Calculator {
    @log
    add(a: number, b: number): number {
        return a + b;
    }
}
```

## Property Decorators

Property decorators are applied to property declarations:

```typescript
function format(formatString: string) {
    return function (target: any, propertyKey: string) {
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

class Message {
    @format('Hello, %s!')
    greeting: string;
}
```

## Parameter Decorators

Parameter decorators are applied to parameter declarations:

```typescript
function validate(target: any, propertyKey: string, parameterIndex: number) {
    const existingRequiredParameters: number[] = Reflect.getOwnMetadata('required', target, propertyKey) || [];
    existingRequiredParameters.push(parameterIndex);
    Reflect.defineMetadata('required', existingRequiredParameters, target, propertyKey);
}

class User {
    save(@validate name: string, @validate age: number) {
        // Implementation
    }
}
```

## Accessor Decorators

Accessor decorators are applied to property accessors:

```typescript
function configurable(value: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.configurable = value;
    };
}

class Point {
    private _x: number;
    private _y: number;
    
    constructor(x: number, y: number) {
        this._x = x;
        this._y = y;
    }
    
    @configurable(false)
    get x() { return this._x; }
    
    @configurable(false)
    get y() { return this._y; }
}
```

## Decorator Factories

Decorator factories are functions that return decorator functions:

```typescript
function color(value: string) {
    return function (target: any) {
        // do something with 'target' and 'value'...
    };
}

@color('red')
class Car {
    // ...
}
```

## Metadata Reflection

TypeScript supports emitting type metadata for decorators:

```typescript
import 'reflect-metadata';

const formatMetadataKey = Symbol('format');

function format(formatString: string) {
    return Reflect.metadata(formatMetadataKey, formatString);
}

function getFormat(target: any, propertyKey: string) {
    return Reflect.getMetadata(formatMetadataKey, target, propertyKey);
}

class Greeter {
    @format('Hello, %s')
    greeting: string;
    
    constructor(message: string) {
        this.greeting = message;
    }
    
    greet() {
        const formatString = getFormat(this, 'greeting');
        return formatString.replace('%s', this.greeting);
    }
}
```

## Best Practices

1. Use decorators sparingly and only when they provide clear benefits
2. Keep decorator logic simple and focused
3. Document decorator behavior clearly
4. Consider using decorator factories for more flexibility
5. Be aware of the performance implications of decorators
6. Use metadata reflection when you need to access type information at runtime
