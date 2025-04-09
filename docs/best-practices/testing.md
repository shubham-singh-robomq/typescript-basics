# TypeScript Testing Best Practices

## 1. Test Organization

```typescript
// Good - Test file structure
describe('UserService', () => {
  describe('getUser', () => {
    it('should return user when found', async () => {
      // Test implementation
    });

    it('should throw error when user not found', async () => {
      // Test implementation
    });
  });
});
```

## 2. Use Proper Test Naming

```typescript
// Good - Descriptive test names
it('should return 404 when resource is not found', () => {
  // Test implementation
});

// Bad - Vague test names
it('should work', () => {
  // Test implementation
});
```

## 3. Use Test Fixtures

```typescript
// Good - Test fixtures
const mockUser: User = {
  id: '1',
  name: 'John Doe',
  email: 'john@example.com'
};

describe('UserService', () => {
  it('should process user data correctly', () => {
    const result = processUser(mockUser);
    expect(result).toBeDefined();
  });
});
```

## 4. Use Proper Mocks

```typescript
// Good - Mock implementation
jest.mock('./userService', () => ({
  UserService: jest.fn().mockImplementation(() => ({
    getUser: jest.fn().mockResolvedValue(mockUser)
  }))
}));

// Good - Mock functions
const mockFetch = jest.fn();
global.fetch = mockFetch;
```

## 5. Use Test Utilities

```typescript
// Good - Test utilities
function createTestUser(overrides: Partial<User> = {}): User {
  return {
    id: '1',
    name: 'Test User',
    email: 'test@example.com',
    ...overrides
  };
}

// Usage
const user = createTestUser({ name: 'Custom Name' });
```

## 6. Use Proper Assertions

```typescript
// Good - Specific assertions
expect(result).toBeDefined();
expect(result).toHaveProperty('id');
expect(result.id).toBe('1');

// Bad - Generic assertions
expect(result).toBeTruthy();
```

## 7. Test Error Cases

```typescript
// Good - Error testing
it('should throw error when input is invalid', () => {
  expect(() => processUser(null)).toThrow(ValidationError);
  expect(() => processUser({})).toThrow('Invalid user data');
});
```

## 8. Use Async Testing

```typescript
// Good - Async testing
it('should fetch user data', async () => {
  const user = await userService.getUser('1');
  expect(user).toEqual(mockUser);
});

// Good - Promise rejection testing
it('should reject when API fails', async () => {
  await expect(userService.getUser('1')).rejects.toThrow(NetworkError);
});
```

## 9. Use Test Coverage

```typescript
// Good - Test coverage comments
/* istanbul ignore next */
function helperFunction() {
  // Implementation
}

// Good - Coverage thresholds in jest.config.js
module.exports = {
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80
    }
  }
};
```

## 10. Use Test Hooks

```typescript
// Good - Test hooks
describe('UserService', () => {
  let userService: UserService;
  let mockRepository: jest.Mocked<UserRepository>;

  beforeEach(() => {
    mockRepository = {
      getUser: jest.fn(),
      saveUser: jest.fn()
    };
    userService = new UserService(mockRepository);
  });

  afterEach(() => {
    jest.clearAllMocks();
  });
});
```

## 11. Use Integration Tests

```typescript
// Good - Integration test
describe('User API Integration', () => {
  let server: Server;

  beforeAll(async () => {
    server = await startTestServer();
  });

  afterAll(async () => {
    await server.close();
  });

  it('should create and retrieve user', async () => {
    const user = await createUser(testUser);
    const retrieved = await getUser(user.id);
    expect(retrieved).toEqual(user);
  });
});
```

## 12. Use E2E Tests

```typescript
// Good - E2E test
describe('User Flow', () => {
  it('should complete user registration', async () => {
    await page.goto('/register');
    await page.fill('#email', 'test@example.com');
    await page.fill('#password', 'password123');
    await page.click('#submit');
    await expect(page).toHaveURL('/dashboard');
  });
});
```

## 13. Use Test Data Factories

```typescript
// Good - Test data factory
class TestDataFactory {
  static createUser(overrides: Partial<User> = {}): User {
    return {
      id: faker.datatype.uuid(),
      name: faker.name.fullName(),
      email: faker.internet.email(),
      ...overrides
    };
  }
}

// Usage
const user = TestDataFactory.createUser({ role: 'admin' });
```

## 14. Use Snapshot Testing

```typescript
// Good - Snapshot testing
it('should render user profile correctly', () => {
  const { container } = render(<UserProfile user={mockUser} />);
  expect(container).toMatchSnapshot();
});

// Good - Inline snapshot
it('should format user data correctly', () => {
  const result = formatUserData(mockUser);
  expect(result).toMatchInlineSnapshot(`
    {
      "email": "john@example.com",
      "id": "1",
      "name": "John Doe"
    }
  `);
});
```

## 15. Use Test Utilities for Common Operations

```typescript
// Good - Test utilities
const TestUtils = {
  async renderWithProviders(component: React.ReactElement) {
    return render(
      <Provider store={store}>
        <ThemeProvider theme={theme}>
          {component}
        </ThemeProvider>
      </Provider>
    );
  },

  async waitForElement(selector: string) {
    return waitFor(() => {
      const element = document.querySelector(selector);
      if (!element) throw new Error('Element not found');
      return element;
    });
  }
};
```

## 16. Use Performance Testing

```typescript
// Good - Performance testing
describe('Performance', () => {
  it('should process 1000 users under 1 second', async () => {
    const start = performance.now();
    await processUsers(largeUserList);
    const end = performance.now();
    expect(end - start).toBeLessThan(1000);
  });
});
```

## 17. Use Mutation Testing

```typescript
// Good - Mutation testing setup
module.exports = {
  mutate: ['src/**/*.ts'],
  testRunner: 'jest',
  mutator: 'typescript',
  reporters: ['html', 'clear-text', 'progress'],
  coverageAnalysis: 'perTest'
};
``` 