<h1 align="center">
  NFCtron Class Transformer
</h1>

<h4 align="center">Decorator-based transformation, serialization, and deserialization of plain objects to class instances</h4>

<p align="center">
  <img src="https://img.shields.io/badge/maintained_with-semantic--release-cc00ff" />
  <img src="https://img.shields.io/badge/contributions-welcome-blue?logo=github" />
  <img src="https://img.shields.io/badge/slack-dev__api-moccasin?logo=slack" />
</p>

<p align="center">
  <a href="https://www.typescriptlang.org">
    <img src="https://img.shields.io/badge/typescript-4.9.5-%23007acc.svg?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript">
  </a>
  <a href="https://nodejs.org">
    <img src="https://img.shields.io/badge/node.js-18+-339933.svg?style=for-the-badge&logo=node.js&logoColor=white" alt="Node.js">
  </a>
  <a href="https://www.npmjs.com">
    <img src="https://img.shields.io/badge/npm-package-CB3837.svg?style=for-the-badge&logo=npm&logoColor=white" alt="npm">
  </a>
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#installation">Installation</a> •
  <a href="#usage">Usage</a> •
  <a href="#features">Features</a> •
  <a href="#development">Development</a> •
  <a href="#tech-stack">Tech Stack</a>
</p>

<!-- docs:index:start -->

## 📋 Overview

NFCtron Class Transformer is a TypeScript library that provides decorator-based transformation, serialization, and deserialization of plain JavaScript objects to class instances and vice versa. This is an NFCtron-maintained fork of the popular [class-transformer](https://github.com/typestack/class-transformer) library, published as `@nfctron/class-transformer` on GitHub Packages.

The library is essential for working with class-based models in TypeScript/JavaScript, especially when dealing with JSON data from APIs, databases, or configuration files. It allows you to transform plain objects into class instances with methods and getters, enabling proper object-oriented programming patterns.

### Key Responsibilities

-   **Object Transformation**: Convert plain objects to class instances and vice versa
-   **Serialization/Deserialization**: Transform objects for JSON serialization and deserialization
-   **Type Safety**: Maintain TypeScript type safety during transformations
-   **Decorator-Based API**: Use decorators to control transformation behavior
-   **Flexible Configuration**: Control what properties are exposed, excluded, or transformed

## 🚀 Installation

### npm

```bash
npm install @nfctron/class-transformer reflect-metadata
```

### pnpm

```bash
pnpm add @nfctron/class-transformer reflect-metadata
```

### yarn

```bash
yarn add @nfctron/class-transformer reflect-metadata
```

### Important: reflect-metadata

The `reflect-metadata` shim is required for this library to work. Make sure to import it in a global place in your application:

```typescript
import 'reflect-metadata';
```

**For Node.js applications**, add this import at the top of your main entry file (e.g., `app.ts`, `index.ts`).

**For browser applications**, add a script tag in your HTML:

```html
<html>
  <head>
    <script src="node_modules/reflect-metadata/Reflect.js"></script>
  </head>
</html>
```

## 📖 Usage

### Basic Example

Transform a plain object to a class instance:

```typescript
import { plainToInstance } from '@nfctron/class-transformer';
import 'reflect-metadata';

class User {
  id: number;
  firstName: string;
  lastName: string;
  age: number;

  getName() {
    return this.firstName + ' ' + this.lastName;
  }

  isAdult() {
    return this.age >= 18;
  }
}

// Plain object from API/JSON
const userJson = {
  id: 1,
  firstName: 'John',
  lastName: 'Doe',
  age: 25,
};

// Transform to class instance
const user = plainToInstance(User, userJson);

// Now you can use class methods
console.log(user.getName()); // "John Doe"
console.log(user.isAdult()); // true
```

### Transform Class to Plain Object

```typescript
import { instanceToPlain } from '@nfctron/class-transformer';

const user = new User();
user.id = 1;
user.firstName = 'John';
user.lastName = 'Doe';

// Transform to plain object for JSON serialization
const plainUser = instanceToPlain(user);
console.log(JSON.stringify(plainUser));
```

### Working with Arrays

```typescript
const usersJson = [
  { id: 1, firstName: 'John', lastName: 'Doe' },
  { id: 2, firstName: 'Jane', lastName: 'Smith' },
];

const users = plainToInstance(User, usersJson);
// users is now User[] with all methods available
```

## ✨ Features

### Decorator-Based API

#### @Expose() - Control Property Exposure

```typescript
import { Expose, instanceToPlain } from '@nfctron/class-transformer';

class User {
  @Expose()
  id: number;

  @Expose()
  email: string;

  password: string; // Will be excluded
}

const user = new User();
user.id = 1;
user.email = 'user@example.com';
user.password = 'secret';

const plain = instanceToPlain(user);
// { id: 1, email: 'user@example.com' }
```

#### @Exclude() - Skip Properties

```typescript
import { Exclude } from '@nfctron/class-transformer';

class User {
  id: number;
  email: string;

  @Exclude()
  password: string;
}
```

#### @Type() - Handle Nested Objects

```typescript
import { Type, plainToInstance } from '@nfctron/class-transformer';

class Photo {
  id: number;
  filename: string;
}

class Album {
  id: number;
  name: string;

  @Type(() => Photo)
  photos: Photo[];
}

const albumJson = {
  id: 1,
  name: 'Vacation',
  photos: [
    { id: 1, filename: 'photo1.jpg' },
    { id: 2, filename: 'photo2.jpg' },
  ],
};

const album = plainToInstance(Album, albumJson);
// album.photos is now Photo[] with proper types
```

#### @Transform() - Custom Transformations

```typescript
import { Transform, Type } from '@nfctron/class-transformer';

class User {
  id: number;

  @Type(() => Date)
  @Transform(({ value }) => value.toISOString(), { toPlainOnly: true })
  createdAt: Date;
}
```

### Advanced Features

-   **Groups**: Control property exposure based on user roles or contexts
-   **Versioning**: Expose different properties for different API versions
-   **Custom Transformers**: Apply custom transformation logic
-   **Circular References**: Handle circular object references
-   **Type Discriminators**: Support for polymorphic types
-   **Implicit Type Conversion**: Automatic type conversion (optional)

## 🛠️ Development

### Prerequisites

-   **[Node.js](https://nodejs.org/)** 18+ (for development)
-   **[npm](https://www.npmjs.com/)** or **[pnpm](https://pnpm.io/)** (package manager)
-   **TypeScript** 4.9.5+

### Setting Up Development Environment

1.   **Clone the repository**:

```bash
git clone git@github.com:NFCtron/class-transformer.git
cd class-transformer
```

2.   **Install dependencies**:

```bash
npm install
# or
pnpm install
```

3.   **Build the project**:

```bash
npm run build
```

This builds multiple output formats:
-   CommonJS (`cjs/`)
-   ES5 modules (`esm5/`)
-   ES2015 modules (`esm2015/`)
-   TypeScript types (`types/`)

### Available Scripts

```bash
# Build all formats
npm run build

# Build specific formats
npm run build:cjs        # CommonJS
npm run build:esm5       # ES5 modules
npm run build:esm2015    # ES2015 modules
npm run build:types      # TypeScript definitions
npm run build:umd        # UMD bundle

# Code quality
npm run lint:check       # Check linting
npm run lint:fix         # Fix linting issues
npm run prettier:check   # Check formatting
npm run prettier:fix     # Fix formatting

# Testing
npm test                 # Run tests with coverage
npm run test:watch       # Watch mode
npm run test:ci          # CI mode (no cache, run in band)
```

### Project Structure

```
.
├── src/                    # Source code
│   ├── ClassTransformer.ts # Main transformer class
│   ├── decorators/         # Decorator implementations
│   ├── interfaces/         # TypeScript interfaces
│   ├── enums/              # Enumerations
│   ├── utils/              # Utility functions
│   └── index.ts            # Main entry point
├── test/                   # Test files
│   └── functional/          # Functional tests
├── sample/                 # Example code samples
├── docs/                   # Documentation
├── tsconfig.json           # TypeScript configuration
└── package.json            # Package configuration
```

### Running Tests

```bash
# Run all tests
npm test

# Watch mode for development
npm run test:watch

# CI mode (for continuous integration)
npm run test:ci
```

### Code Style

-   Follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) for commit messages
-   Use TypeScript for all code
-   Run linting and formatting before committing
-   Ensure all tests pass before submitting PRs

## 📦 Publishing

This package is published to GitHub Packages:

```bash
npm publish
```

The package is configured to publish to:
-   Registry: `https://npm.pkg.github.com/`
-   Package name: `@nfctron/class-transformer`

### Version Management

Versions follow [Semantic Versioning](https://semver.org/):
-   **MAJOR**: Breaking changes
-   **MINOR**: New features (backward compatible)
-   **PATCH**: Bug fixes (backward compatible)

## 🧰 Tech Stack

### Core Technologies

-   **TypeScript**: Programming language (v4.9.5)
-   **reflect-metadata**: Runtime metadata reflection (required dependency)

### Build Tools

-   **TypeScript Compiler**: Multi-format compilation (CJS, ESM5, ESM2015)
-   **Rollup**: UMD bundle generation
-   **Jest**: Testing framework
-   **ESLint**: Code linting
-   **Prettier**: Code formatting

### Development Tools

-   **Husky**: Git hooks
-   **lint-staged**: Pre-commit linting
-   **rimraf**: Clean build directories

## 🔗 Related Repositories

-   [class-validator](https://github.com/typestack/class-validator) - Often used together with class-transformer for validation
-   [nfctron-api](https://github.com/NFCtron/nfctron-api) - Uses this library for DTO transformations
-   [nfctron-cobra](https://github.com/NFCtron/nfctron-cobra) - Uses this library for data transformations

## 📚 Documentation

### API Reference

Main transformation methods:

-   `plainToInstance<T, V>(cls: ClassConstructor<T>, plain: V, options?): T` - Transform plain object to class instance
-   `instanceToPlain<T>(object: T, options?): Record<string, any>` - Transform class instance to plain object
-   `instanceToInstance<T>(object: T, options?): T` - Deep clone class instance

### Decorators

-   `@Expose(options?)` - Expose property during transformation
-   `@Exclude(options?)` - Exclude property from transformation
-   `@Type(typeFn)` - Specify nested object type
-   `@Transform(transformFn, options?)` - Apply custom transformation

### Examples

See the `sample/` directory for comprehensive examples:

-   `sample1-simple-usage/` - Basic usage patterns
-   `sample2-inheritance/` - Working with class inheritance
-   `sample3-custom-arrays/` - Custom array types
-   `sample4-generics/` - Generic type handling
-   `sample5-custom-transformer/` - Custom transformation logic

## 📝 Notes

-   **reflect-metadata Required**: This library requires `reflect-metadata` to be imported globally
-   **ES6 Features**: Uses ES6 classes and decorators
-   **TypeScript Decorators**: Requires `experimentalDecorators` and `emitDecoratorMetadata` in `tsconfig.json`
-   **Circular References**: Handled automatically (ignored except in `instanceToInstance`)
-   **Performance**: Optimized for production use with minimal overhead

## 🚨 Important Considerations

-   **Decorator Metadata**: Ensure `emitDecoratorMetadata: true` in your TypeScript configuration
-   **Runtime Performance**: Transformation has minimal overhead but consider caching for high-frequency operations
-   **Type Safety**: While TypeScript provides compile-time type checking, runtime validation may be needed
-   **Compatibility**: Works with TypeScript 4.9+ and modern JavaScript runtimes

## 🔄 Upstream Relationship

This is an NFCtron-maintained fork of [typestack/class-transformer](https://github.com/typestack/class-transformer). Changes and improvements are maintained by the NFCtron team for internal use and published as `@nfctron/class-transformer`.

<!-- docs:index:end -->
