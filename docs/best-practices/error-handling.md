# TypeScript Error Handling Best Practices

## 1. Use Custom Error Classes

```typescript
// Good
class ValidationError extends Error {
  constructor(message: string) {
    super(message);
    this.name = 'ValidationError';
  }
}

class NetworkError extends Error {
  constructor(message: string) {
    super(message);
    this.name = 'NetworkError';
  }
}
```

## 2. Use Type Guards for Error Handling

```typescript
// Good
function isValidationError(error: unknown): error is ValidationError {
  return error instanceof ValidationError;
}

try {
  // Some operation that might throw
} catch (error) {
  if (isValidationError(error)) {
    // Handle validation error
  } else if (error instanceof NetworkError) {
    // Handle network error
  } else {
    // Handle unexpected error
    console.error('Unexpected error:', error);
  }
}
```

## 3. Use Result Type Pattern

```typescript
// Good
type Result<T, E = Error> = 
  | { success: true; value: T }
  | { success: false; error: E };

function divide(a: number, b: number): Result<number, string> {
  if (b === 0) {
    return { success: false, error: 'Division by zero' };
  }
  return { success: true, value: a / b };
}

const result = divide(10, 2);
if (result.success) {
  console.log(result.value);
} else {
  console.error(result.error);
}
```

## 4. Use Async Error Handling

```typescript
// Good
async function fetchData(): Promise<Result<Data>> {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) {
      return { success: false, error: new NetworkError('Failed to fetch data') };
    }
    const data = await response.json();
    return { success: true, value: data };
  } catch (error) {
    return { success: false, error: new NetworkError('Network request failed') };
  }
}
```

## 5. Use Error Boundaries

```typescript
// Good
class ErrorBoundary extends React.Component<
  { children: React.ReactNode },
  { hasError: boolean; error: Error | null }
> {
  constructor(props: { children: React.ReactNode }) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error: Error) {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <ErrorFallback error={this.state.error} />;
    }
    return this.props.children;
  }
}
```

## 6. Use Proper Error Logging

```typescript
// Good
function logError(error: unknown, context: Record<string, unknown> = {}) {
  const errorInfo = {
    timestamp: new Date().toISOString(),
    error: error instanceof Error ? {
      name: error.name,
      message: error.message,
      stack: error.stack
    } : error,
    context
  };
  
  // Log to appropriate service
  console.error('Error occurred:', errorInfo);
}
```

## 7. Use Error Recovery Strategies

```typescript
// Good
async function withRetry<T>(
  operation: () => Promise<T>,
  maxRetries: number = 3
): Promise<T> {
  let lastError: Error | null = null;
  
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await operation();
    } catch (error) {
      lastError = error as Error;
      if (i < maxRetries - 1) {
        await new Promise(resolve => setTimeout(resolve, 1000 * Math.pow(2, i)));
      }
    }
  }
  
  throw lastError;
}
```

## 8. Use Error Aggregation

```typescript
// Good
class AggregateError extends Error {
  constructor(
    public errors: Error[],
    message: string = 'Multiple errors occurred'
  ) {
    super(message);
    this.name = 'AggregateError';
  }
}

async function validateAll(inputs: string[]): Promise<void> {
  const errors: Error[] = [];
  
  for (const input of inputs) {
    try {
      await validateInput(input);
    } catch (error) {
      errors.push(error as Error);
    }
  }
  
  if (errors.length > 0) {
    throw new AggregateError(errors);
  }
}
```

## 9. Use Error Codes

```typescript
// Good
enum ErrorCode {
  INVALID_INPUT = 'INVALID_INPUT',
  NETWORK_ERROR = 'NETWORK_ERROR',
  DATABASE_ERROR = 'DATABASE_ERROR',
}

class CodedError extends Error {
  constructor(
    public code: ErrorCode,
    message: string
  ) {
    super(message);
    this.name = 'CodedError';
  }
}

function handleError(error: unknown) {
  if (error instanceof CodedError) {
    switch (error.code) {
      case ErrorCode.INVALID_INPUT:
        // Handle invalid input
        break;
      case ErrorCode.NETWORK_ERROR:
        // Handle network error
        break;
      case ErrorCode.DATABASE_ERROR:
        // Handle database error
        break;
    }
  }
}
```

## 10. Use Error Context

```typescript
// Good
class ContextualError extends Error {
  constructor(
    message: string,
    public context: Record<string, unknown>
  ) {
    super(message);
    this.name = 'ContextualError';
  }
}

function processUserData(userId: string, data: unknown) {
  try {
    // Process data
  } catch (error) {
    throw new ContextualError('Failed to process user data', {
      userId,
      data,
      timestamp: new Date().toISOString()
    });
  }
}
``` 