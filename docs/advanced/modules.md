# Modules in TypeScript

TypeScript modules help you organize your code into reusable pieces. This guide covers how to use modules in TypeScript, including imports, exports, and module resolution.

## Basic Module Syntax

### Exporting

```typescript
// math.ts
export function add(x: number, y: number): number {
    return x + y;
}

export const PI = 3.14;
```

### Importing

```typescript
// app.ts
import { add, PI } from './math';

console.log(add(2, 3)); // 5
console.log(PI); // 3.14
```

## Default Exports

```typescript
// Calculator.ts
export default class Calculator {
    add(x: number, y: number): number {
        return x + y;
    }
}

// app.ts
import Calculator from './Calculator';
const calc = new Calculator();
```

## Namespace Imports

```typescript
// math.ts
export namespace Math {
    export function add(x: number, y: number): number {
        return x + y;
    }
    export function subtract(x: number, y: number): number {
        return x - y;
    }
}

// app.ts
import { Math } from './math';
console.log(Math.add(2, 3));
```

## Re-exporting

```typescript
// math.ts
export function add(x: number, y: number): number {
    return x + y;
}

// index.ts
export * from './math';
```

## Module Resolution

### Relative vs Non-relative Imports

```typescript
// Relative imports
import { X } from './moduleA';
import { Y } from '../moduleB';

// Non-relative imports
import { Z } from 'moduleC';
```

## Module Formats

### CommonJS

```typescript
// math.ts
export function add(x: number, y: number): number {
    return x + y;
}

// Generated JavaScript
exports.add = function(x, y) {
    return x + y;
};
```

### ES Modules

```typescript
// math.ts
export function add(x: number, y: number): number {
    return x + y;
}

// Generated JavaScript
export function add(x, y) {
    return x + y;
}
```

## Module Augmentation

```typescript
// original-module.ts
export interface Point {
    x: number;
    y: number;
}

// augmentation.ts
import { Point } from './original-module';

declare module './original-module' {
    interface Point {
        z: number;
    }
}

const point: Point = { x: 0, y: 0, z: 0 };
```

## Path Mapping

```json
// tsconfig.json
{
    "compilerOptions": {
        "baseUrl": ".",
        "paths": {
            "*": ["node_modules/*", "src/types/*"]
        }
    }
}
```

## Best Practices

1. **Use ES modules**: Prefer ES modules over CommonJS
2. **Use relative paths**: For imports within your project
3. **Use index files**: For cleaner imports
4. **Use named exports**: Instead of default exports when possible
5. **Use path aliases**: For cleaner imports
6. **Use module augmentation**: For extending third-party types

## Common Patterns

### Barrel Exports

```typescript
// math/index.ts
export * from './add';
export * from './subtract';
export * from './multiply';
export * from './divide';
```

### Dynamic Imports

```typescript
async function loadModule() {
    const module = await import('./math');
    console.log(module.add(2, 3));
}
```

### Module Aliasing

```typescript
import { ReallyLongModuleName as ShortName } from './really-long-module-name';
```

## Module Organization

### Feature-based Organization

```
src/
  feature1/
    components/
    services/
    types/
    index.ts
  feature2/
    components/
    services/
    types/
    index.ts
```

### Layer-based Organization

```
src/
  presentation/
    components/
    pages/
  domain/
    entities/
    repositories/
  infrastructure/
    services/
    api/
```

## Next Steps

Now that you understand modules, you can learn about:
- [Advanced Types](./advanced-types.md)
- [Decorators](./decorators.md)
- [Configuration](./configuration.md) 