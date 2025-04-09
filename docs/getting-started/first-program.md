# Your First TypeScript Program

Now that you have TypeScript installed, let's write your first TypeScript program! We'll create a simple calculator application to demonstrate basic TypeScript concepts.

## Setting Up the Project

First, create a new directory for your project and initialize it:

```bash
mkdir typescript-calculator
cd typescript-calculator
npm init -y
npm install typescript --save-dev
npx tsc --init
```

## Creating the Source Files

Create a `src` directory and add a new file called `calculator.ts`:

```typescript
// src/calculator.ts

// Define a Calculator class
class Calculator {
    // Add method with type annotations
    add(a: number, b: number): number {
        return a + b;
    }

    // Subtract method with type annotations
    subtract(a: number, b: number): number {
        return a - b;
    }

    // Multiply method with type annotations
    multiply(a: number, b: number): number {
        return a * b;
    }

    // Divide method with type annotations
    divide(a: number, b: number): number {
        if (b === 0) {
            throw new Error("Cannot divide by zero");
        }
        return a / b;
    }
}

// Create an instance of the Calculator class
const calculator = new Calculator();

// Test the calculator
console.log("Addition: ", calculator.add(5, 3));      // Output: 8
console.log("Subtraction: ", calculator.subtract(5, 3)); // Output: 2
console.log("Multiplication: ", calculator.multiply(5, 3)); // Output: 15
console.log("Division: ", calculator.divide(6, 2));    // Output: 3
```

## Compiling and Running

1. Compile the TypeScript code:

```bash
npx tsc
```

2. Run the compiled JavaScript:

```bash
node dist/calculator.js
```

## Understanding the Code

Let's break down what we've written:

1. **Type Annotations**: Notice how we specify types for parameters and return values:
   ```typescript
   add(a: number, b: number): number
   ```

2. **Class Definition**: We use the `class` keyword to define a class:
   ```typescript
   class Calculator { ... }
   ```

3. **Error Handling**: We include basic error handling for division by zero:
   ```typescript
   if (b === 0) {
       throw new Error("Cannot divide by zero");
   }
   ```

## Adding More Features

Let's enhance our calculator with some additional features:

```typescript
// src/calculator.ts

class Calculator {
    // ... existing methods ...

    // Calculate power
    power(base: number, exponent: number): number {
        return Math.pow(base, exponent);
    }

    // Calculate square root
    squareRoot(x: number): number {
        if (x < 0) {
            throw new Error("Cannot calculate square root of negative number");
        }
        return Math.sqrt(x);
    }

    // Calculate factorial
    factorial(n: number): number {
        if (n < 0) {
            throw new Error("Cannot calculate factorial of negative number");
        }
        if (n === 0 || n === 1) {
            return 1;
        }
        return n * this.factorial(n - 1);
    }
}

// Test the new features
console.log("Power: ", calculator.power(2, 3));        // Output: 8
console.log("Square Root: ", calculator.squareRoot(16)); // Output: 4
console.log("Factorial: ", calculator.factorial(5));    // Output: 120
```

## Best Practices

1. **Type Safety**: Always specify types for parameters and return values
2. **Error Handling**: Include appropriate error handling
3. **Code Organization**: Use classes to organize related functionality
4. **Documentation**: Add comments to explain complex logic

## Next Steps

Now that you've written your first TypeScript program, you can:
1. Add more mathematical operations
2. Create a user interface for the calculator
3. Add unit tests
4. Explore more TypeScript features in the [Basics](../basics/types.md) section 