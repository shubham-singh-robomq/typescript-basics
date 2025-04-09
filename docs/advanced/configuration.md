# TypeScript Configuration

This guide covers the essential configuration options in TypeScript and how to use them effectively.

## tsconfig.json

The `tsconfig.json` file is the main configuration file for TypeScript projects. It specifies the root files and compiler options.

### Basic Configuration

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

### Key Compiler Options

#### Target and Module
- `target`: Specifies the ECMAScript target version (e.g., "ES5", "ES2015", "ES2020")
- `module`: Specifies the module system (e.g., "commonjs", "es2015", "esnext")

#### Strict Type Checking
- `strict`: Enables all strict type checking options
- `noImplicitAny`: Raises error on expressions and declarations with an implied 'any' type
- `strictNullChecks`: Makes handling of null and undefined more explicit
- `strictFunctionTypes`: Enables stricter checking of function types

#### Module Resolution
- `baseUrl`: Base directory to resolve non-relative module names
- `paths`: A series of entries which re-map imports to lookup locations
- `moduleResolution`: Specifies module resolution strategy ("node" or "classic")

#### Output Configuration
- `outDir`: Redirect output structure to the directory
- `rootDir`: Specifies the root directory of input files
- `declaration`: Generates corresponding '.d.ts' file

### Advanced Configuration Options

#### Type Checking
```json
{
  "compilerOptions": {
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true
  }
}
```

#### Source Maps
```json
{
  "compilerOptions": {
    "sourceMap": true,
    "inlineSourceMap": true,
    "inlineSources": true
  }
}
```

#### Experimental Features
```json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "useDefineForClassFields": true
  }
}
```

### Project References

TypeScript supports project references for better organization of large codebases:

```json
{
  "references": [
    { "path": "./shared" },
    { "path": "./webapp" }
  ]
}
```

### Watch Mode

Enable watch mode for development:

```json
{
  "watchOptions": {
    "watchFile": "useFsEvents",
    "watchDirectory": "useFsEvents",
    "fallbackPolling": "dynamicPriority"
  }
}
```

### Best Practices

1. Always enable `strict` mode for better type safety
2. Use `baseUrl` and `paths` for cleaner imports
3. Configure `outDir` to separate compiled code from source
4. Use project references for large codebases
5. Enable source maps for debugging
6. Configure appropriate `target` and `module` settings for your environment
