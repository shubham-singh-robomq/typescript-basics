# Installing TypeScript

This guide will walk you through setting up TypeScript in your development environment.

## Prerequisites

Before installing TypeScript, you need to have:

- Node.js (version 12 or higher)
- npm (Node Package Manager) or yarn

You can check if you have these installed by running:

```bash
node --version
npm --version
```

## Installation Methods

### Global Installation

To install TypeScript globally on your system:

```bash
npm install -g typescript
```

This will make the `tsc` command available globally.

### Project-specific Installation

For a project-specific installation (recommended):

```bash
npm init -y
npm install typescript --save-dev
```

This will:
1. Initialize a new npm project
2. Install TypeScript as a development dependency

## Setting Up a TypeScript Project

### Initialize TypeScript Configuration

Create a `tsconfig.json` file:

```bash
npx tsc --init
```

This will create a `tsconfig.json` file with default settings. You can customize it based on your project needs.

### Basic tsconfig.json

Here's a basic configuration to get started:

```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

## Project Structure

A typical TypeScript project structure looks like this:

```
project/
├── src/
│   ├── index.ts
│   └── ...
├── dist/
├── node_modules/
├── package.json
└── tsconfig.json
```

## Running TypeScript

### Compiling TypeScript

To compile your TypeScript files:

```bash
npx tsc
```

This will compile all `.ts` files in your `src` directory to JavaScript in the `dist` directory.

### Watching for Changes

To automatically compile when files change:

```bash
npx tsc --watch
```

## IDE Support

### Visual Studio Code

VS Code has excellent TypeScript support out of the box. Install the following extensions for better experience:

- TypeScript and JavaScript Language Features
- ESLint
- Prettier

### Other IDEs

Most modern IDEs support TypeScript:
- WebStorm
- Atom
- Sublime Text
- Vim/Neovim

## Next Steps

Now that you have TypeScript installed, you're ready to write your [First TypeScript Program](./first-program.md)! 