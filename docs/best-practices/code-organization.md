# TypeScript Code Organization Best Practices

## 1. Project Structure

```
src/
├── components/     # Reusable UI components
├── features/       # Feature-specific modules
├── services/       # API and external services
├── utils/          # Utility functions
├── types/          # Type definitions
├── constants/      # Constants and enums
├── hooks/          # Custom React hooks
├── styles/         # Global styles and themes
└── index.ts        # Main entry point
```

## 2. File Naming Conventions

```typescript
// Good
UserProfile.tsx
user-profile.tsx
userProfile.tsx

// Bad
userprofile.tsx
user_profile.tsx
```

## 3. Module Organization

```typescript
// Good - Single responsibility
// user.service.ts
export class UserService {
  async getUser(id: string): Promise<User> {
    // ...
  }
  
  async updateUser(user: User): Promise<void> {
    // ...
  }
}

// Bad - Multiple responsibilities
// user.ts
export class User {
  // User model
}

export class UserService {
  // User service
}

export class UserValidator {
  // User validation
}
```

## 4. Barrel Files

```typescript
// Good - index.ts in a directory
export * from './user.service';
export * from './user.model';
export * from './user.validator';

// Usage
import { UserService, User, UserValidator } from './user';
```

## 5. Dependency Management

```typescript
// Good - Dependency injection
interface UserRepository {
  getUser(id: string): Promise<User>;
  saveUser(user: User): Promise<void>;
}

class UserService {
  constructor(private repository: UserRepository) {}
  
  async getUser(id: string): Promise<User> {
    return this.repository.getUser(id);
  }
}

// Bad - Direct dependency
class UserService {
  async getUser(id: string): Promise<User> {
    return Database.getUser(id); // Direct dependency
  }
}
```

## 6. Type Organization

```typescript
// Good - Separate type file
// types/user.ts
export interface User {
  id: string;
  name: string;
  email: string;
}

export type UserRole = 'admin' | 'user' | 'guest';

// Good - Group related types
// types/api.ts
export interface ApiResponse<T> {
  data: T;
  status: number;
}

export interface ApiError {
  code: string;
  message: string;
}
```

## 7. Constants Organization

```typescript
// Good - Group related constants
// constants/api.ts
export const API_ENDPOINTS = {
  USERS: '/api/users',
  POSTS: '/api/posts',
} as const;

// constants/validation.ts
export const VALIDATION_RULES = {
  MIN_PASSWORD_LENGTH: 8,
  MAX_USERNAME_LENGTH: 20,
} as const;
```

## 8. Utility Functions Organization

```typescript
// Good - Group by purpose
// utils/string.ts
export function capitalize(str: string): string {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

// utils/date.ts
export function formatDate(date: Date): string {
  return date.toISOString();
}
```

## 9. Component Organization

```typescript
// Good - Feature-based organization
// features/user/components/UserProfile.tsx
export function UserProfile({ user }: { user: User }) {
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}

// features/user/hooks/useUser.ts
export function useUser(userId: string) {
  // User-related hooks
}
```

## 10. Service Layer Organization

```typescript
// Good - Service interfaces
// services/types.ts
export interface IUserService {
  getUser(id: string): Promise<User>;
  updateUser(user: User): Promise<void>;
}

// services/implementations/user.service.ts
export class UserService implements IUserService {
  // Implementation
}
```

## 11. Configuration Organization

```typescript
// Good - Environment-based config
// config/index.ts
export const config = {
  api: {
    baseUrl: process.env.API_BASE_URL,
    timeout: 5000,
  },
  auth: {
    tokenKey: 'auth_token',
  },
} as const;
```

## 12. Test Organization

```typescript
// Good - Mirror source structure
src/
├── components/
│   └── Button.tsx
└── __tests__/
    └── components/
        └── Button.test.tsx

// Good - Test file naming
Button.test.tsx
Button.spec.tsx
Button.e2e.ts
```

## 13. Documentation Organization

```typescript
// Good - JSDoc comments
/**
 * Represents a user in the system
 * @interface User
 * @property {string} id - Unique identifier
 * @property {string} name - User's full name
 */
interface User {
  id: string;
  name: string;
}

// Good - README organization
README.md
├── Installation
├── Usage
├── API Reference
├── Contributing
└── License
```

## 14. Import Organization

```typescript
// Good - Grouped imports
import React from 'react';
import { useState, useEffect } from 'react';

import { UserService } from '@/services';
import { User } from '@/types';

import { Button } from '@/components';
import { useUser } from '@/hooks';

import { API_ENDPOINTS } from '@/constants';
import { formatDate } from '@/utils';
``` 