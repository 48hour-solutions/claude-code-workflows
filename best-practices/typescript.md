# TypeScript Comprehensive Guide & Best Practices

## Table of Contents
1.  [Introduction to TypeScript](#1-introduction-to-typescript)
    * [What is TypeScript?](#11-what-is-typescript)
    * [Why Use TypeScript? Core Benefits](#12-why-use-typescript-core-benefits)
    * [How TypeScript Works](#13-how-typescript-works)
2.  [Setting Up a TypeScript Project](#2-setting-up-a-typescript-project)
    * [Installation](#21-installation)
    * [The TypeScript Compiler (TSC)](#22-the-typescript-compiler-tsc)
    * [Understanding `tsconfig.json`](#23-understanding-tsconfigjson)
    * [Recommended `tsconfig.json` Base](#24-recommended-tsconfigjson-base)
3.  [Core Concepts: Types & Syntax](#3-core-concepts-types--syntax)
    * [Basic Types](#31-basic-types)
    * [Type Inference](#32-type-inference)
    * [The `any`, `unknown`, and `never` Types](#33-the-any-unknown-and-never-types)
    * [Arrays and Tuples](#34-arrays-and-tuples)
    * [Objects, Type Aliases, and Interfaces](#35-objects-type-aliases-and-interfaces)
    * [Enums](#36-enums)
    * [Union and Intersection Types](#37-union-and-intersection-types)
    * [Literal Types](#38-literal-types)
4.  [Functions in TypeScript](#4-functions-in-typescript)
    * [Typing Function Parameters and Return Values](#41-typing-function-parameters-and-return-values)
    * [Optional and Default Parameters](#42-optional-and-default-parameters)
    * [Rest Parameters](#43-rest-parameters)
    * [Function Overloads](#44-function-overloads)
    * [Typing `this`](#45-typing-this)
5.  [Classes and Object-Oriented Programming](#5-classes-and-object-oriented-programming)
    * [Basic Class Definition](#51-basic-class-definition)
    * [Inheritance](#52-inheritance)
    * [Access Modifiers: `public`, `private`, `protected`](#53-access-modifiers-public-private-protected)
    * [Readonly Modifier](#54-readonly-modifier)
    * [Getters and Setters](#55-getters-and-setters)
    * [Static Members](#56-static-members)
    * [Abstract Classes](#57-abstract-classes)
    * [Classes vs. Interfaces](#58-classes-vs-interfaces)
6.  [Advanced Types and Patterns](#6-advanced-types-and-patterns)
    * [Generics](#61-generics)
    * [Utility Types](#62-utility-types)
    * [Mapped Types](#63-mapped-types)
    * [Conditional Types](#64-conditional-types)
    * [Type Guards and Narrowing](#65-type-guards-and-narrowing)
    * [Branding and Nominal Typing](#66-branding-and-nominal-typing)
7.  [Modules and Namespaces](#7-modules-and-namespaces)
    * [ES Modules (`import`/`export`)](#71-es-modules-importexport)
    * [CommonJS Interoperability](#72-commonjs-interoperability)
    * [Namespaces](#73-namespaces)
    * [Ambient Declarations (`d.ts` files)](#74-ambient-declarations-dts-files)
8.  [Best Practices and Style Guide](#8-best-practices-and-style-guide)
    * [Typing Philosophy](#81-typing-philosophy)
    * [Code Formatting and Naming Conventions](#82-code-formatting-and-naming-conventions)
    * [Error Handling](#83-error-handling)
    * [Asynchronous Code (`async`/`await`)](#84-asynchronous-code-asyncawait)
    * [Working with Third-Party Libraries](#85-working-with-third-party-libraries)
9.  [Tooling and Ecosystem](#9-tooling-and-ecosystem)
    * [Linters and Formatters (ESLint, Prettier)](#91-linters-and-formatters-eslint-prettier)
    * [Testing with TypeScript (Jest)](#92-testing-with-typescript-jest)
    * [Build Tools (Webpack, Vite)](#93-build-tools-webpack-vite)
10. [Common Pitfalls and How to Avoid Them](#10-common-pitfalls-and-how-to-avoid-them)
    * [Overusing `any`](#101-overusing-any)
    * [Misunderstanding Structural Typing](#102-misunderstanding-structural-typing)
    * [Incorrectly Typing `this`](#103-incorrectly-typing-this)
    * [Mutation of Readonly Objects](#104-mutation-of-readonly-objects)
11. [Conclusion and Further Resources](#11-conclusion-and-further-resources)

---

## 1. Introduction to TypeScript

### 1.1 What is TypeScript?

TypeScript is an open-source programming language developed and maintained by Microsoft. It is a **strict syntactical superset of JavaScript** and adds optional **static typing** to the language. This means that any valid JavaScript code is also valid TypeScript code, but TypeScript adds its own features on top, primarily a powerful type system.

### 1.2 Why Use TypeScript? Core Benefits

Static typing provides several key advantages over plain JavaScript, especially in large-scale applications:

* **Catch Errors Early**: The TypeScript compiler (`tsc`) analyzes your code and catches type-related errors during development (compile-time), long before your code runs in a browser or on a server (run-time). This prevents entire classes of bugs.
* **Improved Readability and Maintainability**: Type annotations serve as a form of documentation. They make code easier to understand, reason about, and refactor. When another developer (or your future self) reads the code, the shapes of objects, function signatures, and data flows are explicit.
* **Enhanced IDE/Editor Experience**: TypeScript's language server provides powerful tooling for code editors like VS Code, WebStorm, and others. This includes intelligent autocompletion, go-to-definition, refactoring tools, and inline error messages.
* **Scalability**: For large codebases with multiple teams, a strong type system enforces contracts between different parts of the application, making integration safer and more manageable.
* **Access to Modern JavaScript Features**: TypeScript allows you to use the latest ECMAScript features (like optional chaining, nullish coalescing, etc.) and compile them down to older, more widely supported versions of JavaScript for production environments.

### 1.3 How TypeScript Works

TypeScript code is not executed directly. It must first be **transpiled** into plain JavaScript. The workflow is as follows:

1.  **Write**: You write your code in `.ts` or `.tsx` (for React) files.
2.  **Compile**: You run the TypeScript compiler (`tsc`) on your code.
3.  **Type Checking**: `tsc` checks your code for type errors based on your type annotations and its own type inference.
4.  **Transpile**: If there are no errors (or if configured to emit on error), `tsc` converts your TypeScript code into JavaScript code (`.js` files).
5.  **Run**: The resulting JavaScript files are then executed in the target environment (e.g., a browser, Node.js).

---

## 2. Setting Up a TypeScript Project

### 2.1 Installation

TypeScript is installed as an npm package.

```bash
# Install globally (useful for running `tsc` anywhere)
npm install -g typescript

# Install as a project dependency (recommended)
npm install --save-dev typescript
````

### 2.2 The TypeScript Compiler (TSC)

The `tsc` command is the heart of TypeScript.

  * **`tsc <filename>.ts`**: Compiles a single file.
  * **`tsc`**: Compiles the entire project based on the `tsconfig.json` file in the current directory.
  * **`tsc --init`**: Creates a new `tsconfig.json` file with default options.

### 2.3 Understanding `tsconfig.json`

This file is the configuration hub for a TypeScript project. It specifies the root files and the compiler options required to compile the project.

**Key Properties:**

  * `compilerOptions`: An object containing the bulk of the configuration.
  * `include`: An array of file paths or patterns to include in the compilation.
  * `exclude`: An array of file paths or patterns to exclude.
  * `extends`: A path to another configuration file to inherit from.

### 2.4 Recommended `tsconfig.json` Base

For a modern, strict, and robust project, a strong `tsconfig.json` is essential.

```json
{
  "compilerOptions": {
    /* Type Checking */
    "strict": true,                         // Enable all strict type-checking options.
    "noImplicitAny": true,                  // Raise error on expressions and declarations with an implied 'any' type.
    "strictNullChecks": true,               // Enable strict null checks.
    "strictFunctionTypes": true,            // Enable strict checking of function types.
    "strictBindCallApply": true,            // Enable strict 'bind', 'call', and 'apply' methods on functions.
    "strictPropertyInitialization": true,   // Ensure non-undefined class properties are initialized in the constructor.
    "noImplicitThis": true,                 // Raise error on 'this' expressions with an implied 'any' type.
    "alwaysStrict": true,                   // Parse in strict mode and emit "use strict" for each source file.
    "useUnknownInCatchVariables": true,     // Type catch clause variables as 'unknown' instead of 'any'.
    "noUnusedLocals": true,                 // Report errors on unused local variables.
    "noUnusedParameters": true,             // Report errors on unused parameters.
    "noFallthroughCasesInSwitch": true,     // Report errors for fallthrough cases in switch statement.
    "noImplicitReturns": true,              // Report error when not all code paths in function return a value.

    /* Modules */
    "module": "ESNext",                     // Specify module code generation: 'ESNext' for modern bundlers.
    "moduleResolution": "node",             // Resolve modules using Node.js style.
    "resolveJsonModule": true,              // Include modules imported with '.json' extension.
    "isolatedModules": true,                // Transpile each file as a separate module.
    "esModuleInterop": true,                // Enables compatibility with CommonJS modules.
    "allowSyntheticDefaultImports": true,   // Allow default imports from modules with no default export.

    /* JavaScript Support */
    "allowJs": false,                       // Do not allow JavaScript files to be compiled.
    "checkJs": false,                       // Do not type-check JS files.

    /* Emit */
    "declaration": true,                    // Generates corresponding '.d.ts' file.
    "declarationMap": true,                 // Generates a source map for each corresponding '.d.ts' file.
    "sourceMap": true,                      // Generates corresponding '.map' file.
    "outDir": "./dist",                     // Redirect output structure to the directory.
    "removeComments": true,                 // Do not emit comments to output.
    "noEmit": false,                        // Set to true if a bundler like Vite/Webpack is handling transpilation.

    /* Language and Environment */
    "target": "ES2022",                     // Specify ECMAScript target version.
    "lib": ["ES2022", "DOM", "DOM.Iterable"], // List of library files to be included in the compilation.
    "jsx": "react-jsx",                     // Specify JSX code generation for frameworks like React.
    "skipLibCheck": true,                   // Skip type checking of declaration files.
    "forceConsistentCasingInFileNames": true, // Disallow inconsistently-cased references to the same file.

    /* Project Structure */
    "rootDir": "./src",                     // Specify the root directory of input files.
    "baseUrl": ".",                         // Base directory to resolve non-relative module names.
    "paths": {                              // Module path mapping.
      "@/*": ["src/*"]
    }
  },
  "include": ["src/**/*"],                  // Files to include
  "exclude": ["node_modules", "dist"]       // Files to exclude
}
```

-----

## 3\. Core Concepts: Types & Syntax

### 3.1 Basic Types

TypeScript extends JavaScript's primitives with explicit type annotations.

```typescript
let isDone: boolean = false;
let decimal: number = 6;
let hex: number = 0xf00d;
let big: bigint = 100n;
let color: string = "blue";
let sentence: string = `The color is ${color}.`;

// null and undefined
let u: undefined = undefined;
let n: null = null;
```

**Best Practice:** Use lowercase type names (`string`, `number`, `boolean`) instead of their uppercase counterparts (`String`, `Number`, `Boolean`). The lowercase versions refer to JavaScript primitives, while the uppercase versions refer to object wrappers, which are rarely what you need.

### 3.2 Type Inference

TypeScript is smart. If you initialize a variable, it will often infer the type for you, saving you from redundant annotations.

```typescript
// `age` is inferred to be of type `number`
let age = 30;

// `name` is inferred to be of type `string`
let name = "Alice";

// This will cause a type error
// age = "thirty"; // Error: Type 'string' is not assignable to type 'number'.
```

**Best Practice:** Rely on type inference for simple variable declarations. Only add explicit types when the type is not obvious or when you want to be more deliberate (e.g., function return types, complex object types).

### 3.3 The `any`, `unknown`, and `never` Types

These are special types for handling specific scenarios.

  * **`any`**: The "escape hatch." A variable of type `any` opts out of type checking. You can assign anything to it and perform any operation on it without the compiler complaining.

      * **Best Practice:** **Avoid `any` whenever possible.** It defeats the purpose of TypeScript. It is sometimes necessary for legacy JavaScript code or dynamic content, but it should be a last resort.

    <!-- end list -->

    ```typescript
    let looselyTyped: any = 4;
    looselyTyped.ifItExists(); // No error at compile time, but will fail at runtime
    looselyTyped = "now a string";
    ```

  * **`unknown`**: The type-safe counterpart to `any`. You can assign any value to a variable of type `unknown`, but you cannot perform any operations on it until you have performed a type check (type narrowing) to prove what it is.

      * **Best Practice:** **Prefer `unknown` over `any`.** It forces you to handle the value safely.

    <!-- end list -->

    ```typescript
    let safelyTyped: unknown = "hello";

    // safelyTyped.toUpperCase(); // Error: Object is of type 'unknown'.

    if (typeof safelyTyped === "string") {
      // Now TypeScript knows it's a string inside this block
      console.log(safelyTyped.toUpperCase());
    }
    ```

  * **`never`**: Represents the type of values that never occur. It's used in two main cases:

    1.  Functions that never return (e.g., they always throw an error or have an infinite loop).
    2.  For exhaustive checking in control flow analysis (e.g., in a `switch` statement).

    <!-- end list -->

    ```typescript
    function throwError(message: string): never {
      throw new Error(message);
    }

    function infiniteLoop(): never {
      while (true) {}
    }
    ```

### 3.4 Arrays and Tuples

  * **Arrays**: A collection of values of the same type.

    ```typescript
    // Syntax 1: Type[]
    let list: number[] = [1, 2, 3];

    // Syntax 2: Array<Type> (generic)
    let names: Array<string> = ["Alice", "Bob"];
    ```

  * **Tuples**: An array with a fixed number of elements whose types are known, but need not be the same.

    ```typescript
    // A tuple of [string, number]
    let user: [string, number];
    user = ["Alice", 30];

    // user = [30, "Alice"]; // Error: Type 'number' is not assignable to type 'string'.
    ```

### 3.5 Objects, Type Aliases, and Interfaces

  * **Objects**: You can define the shape of an object inline.

    ```typescript
    let person: { name: string; age: number } = {
      name: "Alice",
      age: 30,
    };
    ```

To make object shapes reusable, use `type` aliases or `interface`.

  * **Type Alias**: Gives a new name to a type. Can represent primitives, unions, tuples, or any other type.

    ```typescript
    type Point = {
      x: number;
      y: number;
    };

    type ID = string | number;

    function printCoord(pt: Point) {
      console.log("The coordinate's x value is " + pt.x);
    }

    printCoord({ x: 100, y: 200 });
    ```

  * **Interface**: Another way to name an object type. Interfaces can be extended by other interfaces and implemented by classes.

    ```typescript
    interface User {
      readonly id: number; // property cannot be changed after creation
      name: string;
      email?: string; // `?` marks the property as optional
    }

    interface Admin extends User {
      role: 'admin' | 'super-admin';
    }

    const adminUser: Admin = {
      id: 1,
      name: "Super User",
      role: "admin",
    };
    ```

  * **`type` vs. `interface` Best Practice**:

      * **Use `interface` when defining the "shape" of an object or a class contract.** Interfaces support declaration merging, which can be useful when extending third-party types. They are generally preferred for public-facing APIs.
      * **Use `type` for defining aliases for primitive types, union types, intersection types, tuples, or more complex types that `interface` cannot represent.** They are often used for internal or utility types.

### 3.6 Enums

Enums allow you to define a set of named constants.

  * **Numeric Enums**: Default to number values, auto-incrementing from 0.

    ```typescript
    enum Direction {
      Up,    // 0
      Down,  // 1
      Left,  // 2
      Right, // 3
    }
    let go: Direction = Direction.Up;
    ```

  * **String Enums**: Each member must be explicitly initialized with a string value. They are generally safer and provide more meaningful debug messages.

    ```typescript
    enum LogLevel {
      Error = "ERROR",
      Warn = "WARN",
      Info = "INFO",
      Debug = "DEBUG",
    }
    ```

**Best Practice:** **Prefer string enums or union types of string literals over numeric enums.** Numeric enums can have unintuitive behavior (like reverse mapping) and are less safe (any number can be assigned to a numeric enum type).

```typescript
// Alternative using a union type (often preferred)
type LogLevelUnion = "ERROR" | "WARN" | "INFO" | "DEBUG";

function log(message: string, level: LogLevelUnion) {
  // ...
}

log("Something went wrong", "ERROR");
```

### 3.7 Union and Intersection Types

  * **Union Types (`|`)**: A value that can be one of several types.

    ```typescript
    function printId(id: number | string) {
      if (typeof id === "string") {
        console.log(id.toUpperCase()); // Safe
      } else {
        console.log(id); // Safe
      }
    }
    ```

  * **Intersection Types (`&`)**: Combines multiple types into one. The new type has all the members of all the types.

    ```typescript
    interface Draggable {
      drag: () => void;
    }

    interface Resizable {
      resize: () => void;
    }

    type UIWidget = Draggable & Resizable;

    let widget: UIWidget = {
      drag: () => console.log("dragging"),
      resize: () => console.log("resizing"),
    };
    ```

### 3.8 Literal Types

You can specify the exact value a string, number, or boolean must have.

```typescript
type Alignment = "left" | "right" | "center";
type Status = "success" | "pending" | "failure";
type DiceRoll = 1 | 2 | 3 | 4 | 5 | 6;

let textAlignment: Alignment = "center";
// textAlignment = "middle"; // Error: Type '"middle"' is not assignable to type 'Alignment'.
```

This is extremely powerful when combined with union types for creating finite sets of allowed values.

-----

## 4\. Functions in TypeScript

### 4.1 Typing Function Parameters and Return Values

Explicitly type all function parameters and, as a best practice, the return value.

```typescript
// Best practice: Explicitly type parameters and the return value.
function add(x: number, y: number): number {
  return x + y;
}

// Arrow function syntax
const subtract = (x: number, y: number): number => {
  return x - y;
};

// Void return type for functions that don't return a value
function logMessage(message: string): void {
  console.log(message);
}
```

While TypeScript can often infer the return type, being explicit makes the function's contract clear at a glance.

### 4.2 Optional and Default Parameters

  * **Optional (`?`)**: A parameter that may or may not be provided.

    ```typescript
    function greet(name: string, title?: string): string {
      if (title) {
        return `Hello, ${title} ${name}`;
      }
      return `Hello, ${name}`;
    }

    greet("Alice"); // "Hello, Alice"
    greet("Alice", "Dr."); // "Hello, Dr. Alice"
    ```

  * **Default**: A parameter that has a default value if not provided.

    ```typescript
    function power(base: number, exponent: number = 2): number {
      return base ** exponent;
    }

    power(3); // 9
    power(3, 3); // 27
    ```

### 4.3 Rest Parameters

Collect multiple arguments into a single array.

```typescript
function sum(...numbers: number[]): number {
  return numbers.reduce((total, num) => total + num, 0);
}

sum(1, 2, 3); // 6
sum(10, 20); // 30
```

### 4.4 Function Overloads

Define multiple function signatures for a single function body. This is useful for functions that behave differently based on the number or type of arguments.

```typescript
// Overload signatures
function makeDate(timestamp: number): Date;
function makeDate(m: number, d: number, y: number): Date;

// Implementation signature (must be compatible with all overloads)
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
  if (d !== undefined && y !== undefined) {
    return new Date(y, mOrTimestamp, d);
  } else {
    return new Date(mOrTimestamp);
  }
}

const d1 = makeDate(12345678);
const d2 = makeDate(5, 5, 2025);
// const d3 = makeDate(5, 5); // Error: No overload expects 2 arguments.
```

The implementation signature is not publicly visible; only the overload signatures are.

### 4.5 Typing `this`

You can provide an explicit `this` parameter to a function. This parameter is fake and is erased during compilation, but it allows TypeScript to statically check `this` usage.

```typescript
interface Card {
  suit: string;
  card: number;
}

interface Deck {
  suits: string[];
  cards: number[];
  createCardPicker(this: Deck): () => Card;
}

let deck: Deck = {
  suits: ["hearts", "spades", "clubs", "diamonds"],
  cards: Array(52),
  createCardPicker: function (this: Deck) {
    return () => {
      let pickedCard = Math.floor(Math.random() * 52);
      let pickedSuit = Math.floor(pickedCard / 13);

      return { suit: this.suits[pickedSuit], card: pickedCard % 13 };
    };
  },
};

let cardPicker = deck.createCardPicker();
let pickedCard = cardPicker();

console.log("card: " + pickedCard.card + " of " + pickedCard.suit);
```

-----

## 5\. Classes and Object-Oriented Programming

### 5.1 Basic Class Definition

TypeScript fully supports ES2015 class syntax and adds type annotations.

```typescript
class Greeter {
  // Property
  greeting: string;

  // Constructor
  constructor(message: string) {
    this.greeting = message;
  }

  // Method
  greet(): string {
    return "Hello, " + this.greeting;
  }
}

let greeter = new Greeter("world");
```

### 5.2 Inheritance

Classes can inherit from a base class using the `extends` keyword.

```typescript
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  move(distanceInMeters: number = 0) {
    console.log(`${this.name} moved ${distanceInMeters}m.`);
  }
}

class Dog extends Animal {
  constructor(name: string) {
    super(name); // Call the constructor of the base class
  }

  bark() {
    console.log("Woof! Woof!");
  }

  // Method overriding
  move(distanceInMeters = 5) {
    console.log("Running...");
    super.move(distanceInMeters);
  }
}

const dog = new Dog("Fido");
dog.bark();
dog.move(10);
```

### 5.3 Access Modifiers: `public`, `private`, `protected`

  * **`public` (default)**: The member can be accessed from anywhere.
  * **`private`**: The member can only be accessed from within the same class.
  * **`protected`**: The member can be accessed from within the same class and any subclasses.

<!-- end list -->

```typescript
class Person {
  public name: string;
  private age: number;
  protected ssn: string;

  constructor(name: string, age: number, ssn: string) {
    this.name = name;
    this.age = age;
    this.ssn = ssn;
  }

  public getAge(): number {
    return this.age; // Can access private member from within the class
  }
}

class Employee extends Person {
  constructor(name: string, age: number, ssn: string) {
    super(name, age, ssn);
    console.log(this.ssn); // Can access protected member
    // console.log(this.age); // Error: 'age' is private and only accessible within class 'Person'.
  }
}

const person = new Person("Alice", 30, "123-45-678");
console.log(person.name); // OK
// console.log(person.age); // Error: 'age' is private.
```

A common shorthand is to use **parameter properties** to declare and initialize members directly in the constructor.

```typescript
class Car {
  constructor(
    public readonly make: string,
    public model: string,
    private year: number
  ) {}
}
```

### 5.4 Readonly Modifier

Properties marked `readonly` can only be set during initialization (in the constructor or via a property initializer).

```typescript
class Octopus {
  readonly name: string;
  readonly numberOfLegs: number = 8;

  constructor(theName: string) {
    this.name = theName;
  }
}

let dad = new Octopus("Man with the 8 strong legs");
// dad.name = "Man with the 3-piece suit"; // Error! name is readonly.
```

### 5.5 Getters and Setters

Control access to properties by defining `get` and `set` accessors.

```typescript
class UserProfile {
  private _email: string = "";

  get email(): string {
    return this._email;
  }

  set email(newEmail: string) {
    if (newEmail.includes("@")) {
      this._email = newEmail;
    } else {
      console.error("Invalid email format");
    }
  }
}

const profile = new UserProfile();
profile.email = "test@example.com"; // Calls the setter
console.log(profile.email); // Calls the getter
```

### 5.6 Static Members

Members that belong to the class itself, not to instances of the class.

```typescript
class MathConstants {
  static PI = 3.14159;
  static add(x: number, y: number): number {
    return x + y;
  }
}

console.log(MathConstants.PI);
console.log(MathConstants.add(2, 3));
```

### 5.7 Abstract Classes

Base classes from which other classes may be derived. They cannot be instantiated directly. They may contain abstract methods, which must be implemented by derived classes.

```typescript
abstract class Logger {
  abstract log(message: string): void;

  printDate(date: Date): void {
    this.log(date.toString());
  }
}

class ConsoleLogger extends Logger {
  log(message: string): void {
    console.log(message);
  }
}

// const myLogger = new Logger(); // Error: Cannot create an instance of an abstract class.
const myLogger = new ConsoleLogger();
myLogger.log("Hello, world!");
```

### 5.8 Classes vs. Interfaces

  * A **class** is a blueprint for creating objects with properties and methods. It contains implementation details.
  * An **interface** is a contract that defines the shape of an object. It contains no implementation.

A class can `implement` an interface to ensure it adheres to the interface's contract.

```typescript
interface ClockInterface {
  currentTime: Date;
  setTime(d: Date): void;
}

class DigitalClock implements ClockInterface {
  currentTime: Date = new Date();
  setTime(d: Date) {
    this.currentTime = d;
  }
}
```

-----

## 6\. Advanced Types and Patterns

### 6.1 Generics

Generics provide a way to create reusable components that can work over a variety of types rather than a single one.

  * **Generic Functions**:

    ```typescript
    function identity<T>(arg: T): T {
      return arg;
    }

    let output1 = identity<string>("myString"); // Explicitly set T to string
    let output2 = identity(123); // Type is inferred as number
    ```

  * **Generic Interfaces**:

    ```typescript
    interface GenericResponse<T> {
      data: T;
      status: 'success' | 'error';
      message?: string;
    }

    interface UserData {
      id: number;
      name: string;
    }

    const userResponse: GenericResponse<UserData> = {
      data: { id: 1, name: "Alice" },
      status: 'success'
    };
    ```

  * **Generic Classes**:

    ```typescript
    class Box<T> {
      private content: T;

      constructor(initialContent: T) {
        this.content = initialContent;
      }

      getContent(): T {
        return this.content;
      }
    }

    const stringBox = new Box<string>("hello");
    const numberBox = new Box(42); // Inferred
    ```

  * **Generic Constraints**: You can constrain the types that can be used with a generic.

    ```typescript
    interface Lengthwise {
      length: number;
    }

    function loggingIdentity<T extends Lengthwise>(arg: T): T {
      console.log(arg.length); // We know T has a .length property
      return arg;
    }

    loggingIdentity({ length: 10, value: 3 });
    loggingIdentity("hello");
    // loggingIdentity(3); // Error: number does not have a 'length' property.
    ```

### 6.2 Utility Types

TypeScript provides several built-in utility types to facilitate common type transformations.

  * **`Partial<T>`**: Constructs a type with all properties of `T` set to optional.

    ```typescript
    interface Todo {
      title: string;
      description: string;
    }

    function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
      return { ...todo, ...fieldsToUpdate };
    }
    ```

  * **`Readonly<T>`**: Constructs a type with all properties of `T` set to `readonly`.

    ```typescript
    const todo: Readonly<Todo> = {
      title: "Delete inactive users",
      description: "...",
    };
    // todo.title = "Hello"; // Error: Cannot assign to 'title' because it is a read-only property.
    ```

  * **`Pick<T, K>`**: Constructs a type by picking the set of properties `K` from `T`.

    ```typescript
    interface UserProfile {
      id: number;
      name: string;
      email: string;
      isActive: boolean;
    }

    type UserPreview = Pick<UserProfile, "name" | "email">;
    // const preview: UserPreview = { name: "Alice", email: "a@b.com" };
    ```

  * **`Omit<T, K>`**: Constructs a type by picking all properties from `T` and then removing `K`.

    ```typescript
    type UserForCreation = Omit<UserProfile, "id" | "isActive">;
    // const newUser: UserForCreation = { name: "Bob", email: "b@c.com" };
    ```

  * **`Record<K, T>`**: Constructs an object type with a set of properties `K` of type `T`.

    ```typescript
    type PageInfo = { title: string };
    type Page = "home" | "about" | "contact";

    const nav: Record<Page, PageInfo> = {
      home: { title: "Home" },
      about: { title: "About" },
      contact: { title: "Contact" },
    };
    ```

  * **Others**: `Exclude<T, U>`, `Extract<T, U>`, `NonNullable<T>`, `ReturnType<T>`, `Parameters<T>`, etc.

### 6.3 Mapped Types

A mapped type creates a new type by iterating over the properties of an existing type.

```typescript
type ReadonlyAndOptional<T> = {
  +readonly [P in keyof T]+?: T[P];
};

interface User {
  name: string;
  age: number;
}

// Result: { readonly name?: string; readonly age?: number; }
type ReadonlyOptionalUser = ReadonlyAndOptional<User>;
```

The `keyof` operator takes an object type and produces a string or numeric literal union of its keys. `T[P]` is a indexed access or lookup type.

### 6.4 Conditional Types

A conditional type selects one of two possible types based on a condition expressed as a type relationship test.
`T extends U ? X : Y`

```typescript
type IsString<T> = T extends string ? true : false;

type A = IsString<"hello">; // type A = true
type B = IsString<123>;     // type B = false
```

The `infer` keyword can be used within the `extends` clause of a conditional type to infer a type variable.

```typescript
// Infers the type of the elements in an array
type Flatten<T> = T extends Array<infer Item> ? Item : T;

type Str = Flatten<string[]>; // type Str = string
type Num = Flatten<number>;   // type Num = number
```

### 6.5 Type Guards and Narrowing

Type narrowing is the process of moving a variable from a less precise type to a more precise type.

  * **`typeof` guards**:

    ```typescript
    function padLeft(value: string, padding: string | number) {
      if (typeof padding === "number") {
        return Array(padding + 1).join(" ") + value;
      }
      if (typeof padding === "string") {
        return padding + value;
      }
      throw new Error("Expected string or number.");
    }
    ```

  * **Truthiness narrowing**: Checking for `null`, `undefined`, `false`, `0`, `""`, etc.

  * **`instanceof` guards**: For checking class instances.

  * **`in` operator guards**: For checking if an object has a property.

    ```typescript
    interface Fish { swim: () => void; }
    interface Bird { fly: () => void; }

    function move(pet: Fish | Bird) {
      if ("swim" in pet) {
        return pet.swim(); // pet is Fish here
      }
      return pet.fly(); // pet is Bird here
    }
    ```

  * **Custom type guards (user-defined)**: A function whose return type is a **type predicate**.

    ```typescript
    function isFish(pet: Fish | Bird): pet is Fish {
      return (pet as Fish).swim !== undefined;
    }

    function moveSafely(pet: Fish | Bird) {
      if (isFish(pet)) {
        pet.swim();
      } else {
        pet.fly();
      }
    }
    ```

### 6.6 Branding and Nominal Typing

TypeScript uses a structural type system, which means if two types have the same structure, they are considered compatible. Sometimes you need to distinguish between types with the same structure (e.g., `UserID` and `PostID` which are both `string`). This is called nominal typing. You can simulate it using "branding".

```typescript
type Brand<K, T> = K & { __brand: T };

type UserID = Brand<string, "UserID">;
type PostID = Brand<string, "PostID">;

function getUser(id: UserID) { /* ... */ }
function getPost(id: PostID) { /* ... */ }

const userId = "user-123" as UserID;
const postId = "post-456" as PostID;

getUser(userId);
getPost(postId);

// getUser(postId); // Error: Type 'PostID' is not assignable to type 'UserID'.
```

-----

## 7\. Modules and Namespaces

### 7.1 ES Modules (`import`/`export`)

TypeScript uses ES Modules syntax. This is the standard and recommended way to structure your code.

  * **`export`**:

    ```typescript
    // math.ts
    export const PI = 3.14;
    export function add(x: number, y: number): number {
      return x + y;
    }
    export default class Calculator {
      // ...
    }
    ```

  * **`import`**:

    ```typescript
    // main.ts
    import Calculator, { PI, add } from './math';

    console.log(PI);
    const result = add(2, 3);

    import * as math from './math'; // Import everything into a namespace object
    console.log(math.PI);
    ```

### 7.2 CommonJS Interoperability

When working with Node.js or older libraries, you may encounter CommonJS modules (`require`/`module.exports`). TypeScript can handle these if `esModuleInterop` is set to `true` in `tsconfig.json`.

### 7.3 Namespaces

Before ES Modules were standard, TypeScript had its own module format called `namespaces`. They are now largely obsolete.

**Best Practice**: **Use ES Modules instead of namespaces.** Namespaces can be useful in some specific legacy scenarios or for organizing types in declaration files, but modern application code should always prefer `import`/`export`.

### 7.4 Ambient Declarations (`d.ts` files)

When you use a JavaScript library that was not written in TypeScript, you need to tell TypeScript about the shapes and types of that library. This is done with a **declaration file**, which has a `.d.ts` extension.

These files contain only type information, no implementations.

```typescript
// my-library.d.ts
declare module 'my-untyped-library' {
  export function doSomething(input: string): boolean;
  export const version: string;
}
```

Many popular libraries have their types available on the **DefinitelyTyped** project, which you can install from npm under the `@types/` scope.

```bash
npm install --save-dev @types/node
npm install --save-dev @types/react
npm install --save-dev @types/lodash
```

-----

## 8\. Best Practices and Style Guide

### 8.1 Typing Philosophy

1.  **Be as specific as possible**: Avoid `any` like the plague. Use `unknown` if you must accept any value, then narrow it down.

2.  **Let inference do its job**: Don't over-annotate. If the compiler can figure out the type, let it.

3.  **Type boundaries**: Be explicit with types at the "boundaries" of your system—function arguments, return values, and public class members. This makes your API contracts clear.

4.  **Embrace `readonly` and immutability**: Use `readonly` for properties and `Readonly<T>` or `as const` for objects/arrays to prevent accidental mutations.

    ```typescript
    const config = {
      apiUrl: "[https://api.example.com](https://api.example.com)",
      timeout: 5000,
    } as const; // `as const` makes all properties readonly and literals

    // config.timeout = 3000; // Error
    ```

### 8.2 Code Formatting and Naming Conventions

  * **Formatting**: Use **Prettier**. It's the de-facto standard for code formatting in the JavaScript/TypeScript ecosystem and eliminates debates over style.
  * **Naming**:
      * **Variables and Functions**: `camelCase` (e.g., `userName`, `calculateTotal`).
      * **Classes, Interfaces, Type Aliases, Enums**: `PascalCase` (e.g., `UserService`, `IUser`, `UserType`).
      * **Private Members**: Some teams prefix private members with an underscore `_` (e.g., `_internalValue`), but it's not required since TypeScript enforces privacy.
      * **Interfaces**: Do not prefix interfaces with `I` (e.g., use `User` not `IUser`). This is a convention from other languages (like C\#) that is not idiomatic in TypeScript.

### 8.3 Error Handling

Type errors for expected failures. Use union types with a result object or tuple.

```typescript
type Result<T, E> = { success: true; value: T } | { success: false; error: E };

function parseJson(jsonString: string): Result<any, Error> {
  try {
    return { success: true, value: JSON.parse(jsonString) };
  } catch (error) {
    if (error instanceof Error) {
      return { success: false, error };
    }
    return { success: false, error: new Error("An unknown error occurred") };
  }
}

const result = parseJson('{ "name": "Alice" }');
if (result.success) {
  console.log(result.value.name);
} else {
  console.error(result.error.message);
}
```

### 8.4 Asynchronous Code (`async`/`await`)

Properly type your Promises. The `Promise<T>` generic type represents the value the promise will eventually resolve with.

```typescript
interface User {
  id: number;
  name: string;
}

async function fetchUser(id: number): Promise<User> {
  const response = await fetch(`https://api.example.com/users/${id}`);
  if (!response.ok) {
    throw new Error("Failed to fetch user");
  }
  const user: User = await response.json();
  return user;
}
```

**Best Practice**: A function marked `async` must always have a return type annotation of `Promise<T>`.

### 8.5 Working with Third-Party Libraries

1.  **Always install `@types` packages** if they exist.
2.  If no types exist, consider creating a basic `d.ts` file for the parts of the library you use.
3.  Use module augmentation to add properties to existing interfaces from third-party libraries if needed.

-----

## 9\. Tooling and Ecosystem

### 9.1 Linters and Formatters (ESLint, Prettier)

  * **ESLint** analyzes your code to find problems. With `@typescript-eslint/parser` and `@typescript-eslint/eslint-plugin`, it can enforce TypeScript-specific rules.
  * **Prettier** is an opinionated code formatter that ensures a consistent style across the entire codebase.

**Setup**:

1.  Install packages: `eslint`, `prettier`, `@typescript-eslint/parser`, `@typescript-eslint/eslint-plugin`, `eslint-config-prettier` (to disable ESLint rules that conflict with Prettier).
2.  Configure `.eslintrc.js` and `.prettierrc`.

### 9.2 Testing with TypeScript (Jest)

Jest is a popular testing framework. Use `ts-jest` to allow Jest to work directly with TypeScript files.

**Setup**:

1.  `npm install --save-dev jest ts-jest @types/jest`
2.  Create `jest.config.js`:
    ```javascript
    module.exports = {
      preset: 'ts-jest',
      testEnvironment: 'node',
    };
    ```
3.  Write tests in `.test.ts` files.

### 9.3 Build Tools (Webpack, Vite)

While `tsc` can transpile your code, for complex front-end applications, you'll use a build tool or bundler.

  * **Vite**: A modern, fast build tool that uses native ES modules during development. It has first-class TypeScript support out of the box. **Highly recommended for new projects.**
  * **Webpack**: A powerful and highly configurable module bundler. Use `ts-loader` or `@babel/preset-typescript` to integrate TypeScript into a Webpack build pipeline.

-----

## 10\. Common Pitfalls and How to Avoid Them

### 10.1 Overusing `any`

  * **Problem**: Using `any` silences the type checker, removing the benefits of TypeScript.
  * **Solution**: Use `unknown` for values of unknown type. Use specific types whenever possible. If you are migrating a JS codebase, use a gradual approach but aim to eliminate `any`.

### 10.2 Misunderstanding Structural Typing

  * **Problem**: TypeScript checks if the *shape* of an object matches, not its explicit name. This can lead to unexpected compatibility.
    ```typescript
    class Point2D { x = 0; y = 0; }
    class Point3D { x = 0; y = 0; z = 0; }

    function logPoint(p: Point2D) { console.log(p.x, p.y); }

    logPoint(new Point3D()); // This is OK! Point3D has x and y.
    ```
  * **Solution**: Understand that this is a feature, not a bug. If you need nominal typing, use the branding pattern described earlier.

### 10.3 Incorrectly Typing `this`

  * **Problem**: `this` can be rebound in JavaScript, leading to runtime errors.
    ```typescript
    class MyClass {
      message = "Hello";
      logMessage() {
        console.log(this.message);
      }
    }
    const instance = new MyClass();
    const logger = instance.logMessage;
    // logger(); // Fails at runtime, `this` is undefined
    ```
  * **Solution**: Use arrow functions for class methods to automatically bind `this`, or use the explicit `this` parameter typing in functions.
    ```typescript
    class MyClass {
      message = "Hello";
      logMessage = () => { // Arrow function binds 'this'
        console.log(this.message);
      }
    }
    ```

### 10.4 Mutation of Readonly Objects

  * **Problem**: `Readonly<T>` provides compile-time safety, but it's erased at runtime. JavaScript code can still mutate the object.
  * **Solution**: Be aware of this limitation. For true runtime immutability, use libraries like `immer` or `immutable.js`, or techniques like deep freezing objects.

-----

## 11\. Conclusion and Further Resources

TypeScript is a powerful tool that brings safety, predictability, and improved developer experience to JavaScript development. By understanding its core concepts, adhering to best practices, and leveraging its advanced features, you can build more robust, maintainable, and scalable applications.

  * **Official TypeScript Handbook**: [typescriptlang.org/docs/handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
  * **TypeScript Playground**: [typescriptlang.org/play](https://www.typescriptlang.org/play)
  * **DefinitelyTyped (for type definitions)**: [github.com/DefinitelyTyped/DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)
  * **Total TypeScript (Tutorials)**: [totaltypescript.com](https://www.totaltypescript.com/)
