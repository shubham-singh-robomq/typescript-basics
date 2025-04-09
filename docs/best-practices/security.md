# TypeScript Security Best Practices

## 1. Input Validation

```typescript
// Good - Input validation
function validateUserInput(input: unknown): UserInput {
  if (typeof input !== 'object' || input === null) {
    throw new ValidationError('Invalid input format');
  }
  
  const { name, email } = input as Record<string, unknown>;
  
  if (typeof name !== 'string' || name.length < 2) {
    throw new ValidationError('Invalid name');
  }
  
  if (typeof email !== 'string' || !email.includes('@')) {
    throw new ValidationError('Invalid email');
  }
  
  return { name, email };
}
```

## 2. Sanitize User Input

```typescript
// Good - Input sanitization
function sanitizeHtml(input: string): string {
  return input
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#039;');
}

// Good - Use template literals safely
const safeHtml = `<div>${sanitizeHtml(userInput)}</div>`;
```

## 3. Secure Password Handling

```typescript
// Good - Password hashing
import * as bcrypt from 'bcrypt';

async function hashPassword(password: string): Promise<string> {
  const saltRounds = 10;
  return bcrypt.hash(password, saltRounds);
}

async function verifyPassword(password: string, hash: string): Promise<boolean> {
  return bcrypt.compare(password, hash);
}
```

## 4. Secure API Authentication

```typescript
// Good - JWT handling
import * as jwt from 'jsonwebtoken';

const JWT_SECRET = process.env.JWT_SECRET;

function generateToken(user: User): string {
  return jwt.sign(
    { userId: user.id, role: user.role },
    JWT_SECRET,
    { expiresIn: '1h' }
  );
}

function verifyToken(token: string): UserPayload {
  return jwt.verify(token, JWT_SECRET) as UserPayload;
}
```

## 5. Secure File Uploads

```typescript
// Good - File upload validation
function validateFileUpload(file: File): void {
  const MAX_SIZE = 5 * 1024 * 1024; // 5MB
  const ALLOWED_TYPES = ['image/jpeg', 'image/png'];
  
  if (file.size > MAX_SIZE) {
    throw new Error('File too large');
  }
  
  if (!ALLOWED_TYPES.includes(file.type)) {
    throw new Error('Invalid file type');
  }
}
```

## 6. Secure Database Operations

```typescript
// Good - Parameterized queries
async function getUserById(id: string): Promise<User> {
  const query = 'SELECT * FROM users WHERE id = $1';
  const result = await db.query(query, [id]);
  return result.rows[0];
}

// Bad - String concatenation
async function getUserById(id: string): Promise<User> {
  const query = `SELECT * FROM users WHERE id = ${id}`; // SQL injection risk
  return db.query(query);
}
```

## 7. Secure Environment Variables

```typescript
// Good - Environment validation
function validateEnv(): void {
  const required = ['DB_URL', 'JWT_SECRET', 'API_KEY'];
  
  for (const key of required) {
    if (!process.env[key]) {
      throw new Error(`Missing required environment variable: ${key}`);
    }
  }
}

// Good - Type-safe environment
interface Env {
  DB_URL: string;
  JWT_SECRET: string;
  API_KEY: string;
}

const env: Env = {
  DB_URL: process.env.DB_URL!,
  JWT_SECRET: process.env.JWT_SECRET!,
  API_KEY: process.env.API_KEY!
};
```

## 8. Secure CORS Configuration

```typescript
// Good - CORS configuration
const corsOptions = {
  origin: (origin: string | undefined, callback: (err: Error | null, allow?: boolean) => void) => {
    const allowedOrigins = ['https://example.com', 'https://api.example.com'];
    if (!origin || allowedOrigins.includes(origin)) {
      callback(null, true);
    } else {
      callback(new Error('Not allowed by CORS'));
    }
  },
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization'],
  credentials: true
};
```

## 9. Secure Rate Limiting

```typescript
// Good - Rate limiting
import rateLimit from 'express-rate-limit';

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP, please try again later'
});

app.use('/api/', limiter);
```

## 10. Secure Headers

```typescript
// Good - Security headers
import helmet from 'helmet';

app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'", "'unsafe-inline'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      imgSrc: ["'self'", 'data:', 'https:'],
      connectSrc: ["'self'", 'https://api.example.com']
    }
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true
  }
}));
```

## 11. Secure Session Management

```typescript
// Good - Session configuration
import session from 'express-session';

app.use(session({
  secret: process.env.SESSION_SECRET!,
  name: 'sessionId',
  cookie: {
    secure: true,
    httpOnly: true,
    sameSite: 'strict',
    maxAge: 24 * 60 * 60 * 1000 // 24 hours
  },
  resave: false,
  saveUninitialized: false
}));
```

## 12. Secure Error Handling

```typescript
// Good - Secure error handling
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
  console.error(err.stack);
  
  // Don't expose internal errors
  const errorResponse = {
    message: 'An error occurred',
    ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
  };
  
  res.status(500).json(errorResponse);
});
```

## 13. Secure Logging

```typescript
// Good - Secure logging
function secureLog(data: unknown): void {
  const sanitizedData = {
    ...data,
    password: '[REDACTED]',
    token: '[REDACTED]',
    creditCard: '[REDACTED]'
  };
  
  console.log(JSON.stringify(sanitizedData));
}
```

## 14. Secure File System Operations

```typescript
// Good - Secure file operations
import * as path from 'path';

function getSafePath(userInput: string): string {
  const basePath = '/uploads';
  const safePath = path.normalize(userInput).replace(/^(\.\.(\/|\\|$))+/, '');
  return path.join(basePath, safePath);
}
```

## 15. Secure API Keys

```typescript
// Good - API key management
class ApiKeyManager {
  private static instance: ApiKeyManager;
  private keys: Map<string, string> = new Map();
  
  private constructor() {}
  
  static getInstance(): ApiKeyManager {
    if (!ApiKeyManager.instance) {
      ApiKeyManager.instance = new ApiKeyManager();
    }
    return ApiKeyManager.instance;
  }
  
  validateKey(key: string): boolean {
    return this.keys.has(key);
  }
}
```

## 16. Secure WebSocket Connections

```typescript
// Good - WebSocket security
import * as WebSocket from 'ws';

const wss = new WebSocket.Server({
  verifyClient: (info, callback) => {
    const token = info.req.headers['sec-websocket-protocol'];
    try {
      verifyToken(token as string);
      callback(true);
    } catch (error) {
      callback(false, 401, 'Unauthorized');
    }
  }
});
```

## 17. Secure Dependency Management

```typescript
// Good - Dependency security
{
  "scripts": {
    "preinstall": "npx npm-audit",
    "postinstall": "npx snyk test"
  },
  "devDependencies": {
    "npm-audit": "^1.0.0",
    "snyk": "^1.0.0"
  }
}
``` 