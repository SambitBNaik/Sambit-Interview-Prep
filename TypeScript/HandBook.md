# The Complete TypeScript Handbook

## Table of Contents
1. [Introduction](#introduction)
2. [Getting Started](#getting-started)
3. [Basic Types](#basic-types)
4. [Variables and Constants](#variables-and-constants)
5. [Functions](#functions)
6. [Interfaces](#interfaces)
7. [Classes](#classes)
8. [Generics](#generics)
9. [Type Manipulation](#type-manipulation)
10. [Enums](#enums)
11. [Advanced Types](#advanced-types)
12. [Modules](#modules)
13. [Decorators](#decorators)
14. [Asynchronous Programming](#asynchronous-programming)
15. [TypeScript with React](#typescript-with-react)
16. [Best Practices](#best-practices)

---

## Introduction

### What is TypeScript?

TypeScript is a strongly typed programming language that builds on JavaScript. It adds optional static type checking, which helps catch errors during development rather than at runtime.

**Key Benefits:**
- Catches errors early through static typing
- Enhanced IDE support with autocomplete and IntelliSense
- Better code documentation through types
- Easier refactoring
- Improved collaboration in large teams

### TypeScript vs JavaScript

```typescript
// JavaScript
function greet(name) {
    return "Hello, " + name;
}
greet(123); // No error, but might cause issues

// TypeScript
function greet(name: string): string {
    return "Hello, " + name;
}
greet(123); // Error: Argument of type 'number' is not assignable to parameter of type 'string'
```

---

## Getting Started

### Installation

```bash
# Install TypeScript globally
npm install -g typescript

# Check version
tsc --version

# Initialize a TypeScript project
tsc --init
```

### tsconfig.json

The `tsconfig.json` file configures TypeScript compiler options:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

### Compiling TypeScript

```bash
# Compile a single file
tsc filename.ts

# Compile all files in project
tsc

# Watch mode (auto-recompile on changes)
tsc --watch
```

---

## Basic Types

### Primitive Types

```typescript
// String
let username: string = "Alice";
let greeting: string = `Hello, ${username}`;

// Number
let age: number = 25;
let price: number = 99.99;
let hex: number = 0xf00d;
let binary: number = 0b1010;

// Boolean
let isActive: boolean = true;
let hasPermission: boolean = false;

// Null and Undefined
let nothing: null = null;
let notDefined: undefined = undefined;

// Symbol
let sym: symbol = Symbol("unique");

// BigInt
let bigNumber: bigint = 100n;
```

### Array Types

```typescript
// Array of numbers
let numbers: number[] = [1, 2, 3, 4, 5];
let scores: Array<number> = [90, 85, 88];

// Array of strings
let names: string[] = ["Alice", "Bob", "Charlie"];

// Mixed array (using union types)
let mixed: (string | number)[] = ["Alice", 25, "Bob", 30];

// Array of objects
let users: { name: string; age: number }[] = [
    { name: "Alice", age: 25 },
    { name: "Bob", age: 30 }
];
```

### Tuple Types

```typescript
// Fixed-length array with specific types
let person: [string, number] = ["Alice", 25];

// Accessing tuple elements
let name = person[0]; // string
let age = person[1];  // number

// Named tuples (TypeScript 4.0+)
type Point = [x: number, y: number];
let point: Point = [10, 20];

// Optional tuple elements
type Response = [success: boolean, data?: string];
let response1: Response = [true, "Success"];
let response2: Response = [false];

// Rest elements in tuples
type StringNumberBooleans = [string, number, ...boolean[]];
let data: StringNumberBooleans = ["hello", 42, true, false, true];
```

### Object Types

```typescript
// Object type annotation
let user: { name: string; age: number; email: string } = {
    name: "Alice",
    age: 25,
    email: "alice@example.com"
};

// Optional properties
let product: { name: string; price: number; description?: string } = {
    name: "Laptop",
    price: 999
};

// Readonly properties
let config: { readonly apiKey: string; timeout: number } = {
    apiKey: "abc123",
    timeout: 3000
};
// config.apiKey = "new"; // Error: Cannot assign to 'apiKey'
```

### Any Type

```typescript
// Avoid using 'any' when possible
let random: any = 42;
random = "now a string";
random = { anything: true };

// Better alternative: unknown
let uncertain: unknown = 42;
if (typeof uncertain === "string") {
    console.log(uncertain.toUpperCase()); // Type-safe
}
```

### Void, Never, and Unknown

```typescript
// Void: function returns nothing
function logMessage(message: string): void {
    console.log(message);
}

// Never: function never returns
function throwError(message: string): never {
    throw new Error(message);
}

function infiniteLoop(): never {
    while (true) {}
}

// Unknown: type-safe alternative to any
let value: unknown = "hello";

if (typeof value === "string") {
    console.log(value.toUpperCase());
}
```

---

## Variables and Constants

### Type Inference

```typescript
// TypeScript infers the type
let message = "Hello"; // inferred as string
let count = 42;        // inferred as number
let isValid = true;    // inferred as boolean

// Type widening
let x = 3; // number
const y = 3; // 3 (literal type)
```

### Type Assertions

```typescript
// Angle-bracket syntax
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

// 'as' syntax (preferred, especially in JSX)
let anotherValue: any = "another string";
let length: number = (anotherValue as string).length;

// Non-null assertion operator
function getValue(id?: string): string | undefined {
    return id;
}

let result = getValue("123")!; // Assert it's not null/undefined
```

### Literal Types

```typescript
// String literal types
let direction: "north" | "south" | "east" | "west";
direction = "north"; // OK
// direction = "up"; // Error

// Numeric literal types
let dice: 1 | 2 | 3 | 4 | 5 | 6;
dice = 3; // OK

// Boolean literal types
let yes: true = true;

// Combining literals with unions
type Status = "pending" | "approved" | "rejected";
type HttpCode = 200 | 404 | 500;
```

---

## Functions

### Function Declarations

```typescript
// Named function with typed parameters and return type
function add(a: number, b: number): number {
    return a + b;
}

// Function with optional parameters
function greet(name: string, greeting?: string): string {
    return greeting ? `${greeting}, ${name}` : `Hello, ${name}`;
}

// Function with default parameters
function createUser(name: string, role: string = "user"): object {
    return { name, role };
}

// Function with rest parameters
function sum(...numbers: number[]): number {
    return numbers.reduce((total, n) => total + n, 0);
}

console.log(sum(1, 2, 3, 4)); // 10
```

### Function Expressions

```typescript
// Function expression
const multiply = function(a: number, b: number): number {
    return a * b;
};

// Arrow function
const divide = (a: number, b: number): number => {
    if (b === 0) throw new Error("Division by zero");
    return a / b;
};

// Concise arrow function
const square = (n: number): number => n * n;
```

### Function Types

```typescript
// Declaring a function type
type MathOperation = (a: number, b: number) => number;

const add: MathOperation = (a, b) => a + b;
const subtract: MathOperation = (a, b) => a - b;

// Function type as parameter
function calculate(operation: MathOperation, a: number, b: number): number {
    return operation(a, b);
}

console.log(calculate(add, 5, 3));      // 8
console.log(calculate(subtract, 5, 3)); // 2
```

### Overloading

```typescript
// Function overload signatures
function format(value: string): string;
function format(value: number): string;
function format(value: boolean): string;

// Implementation signature
function format(value: string | number | boolean): string {
    if (typeof value === "string") {
        return value.toUpperCase();
    } else if (typeof value === "number") {
        return value.toFixed(2);
    } else {
        return value ? "YES" : "NO";
    }
}

console.log(format("hello"));  // "HELLO"
console.log(format(42.123));   // "42.12"
console.log(format(true));     // "YES"
```

### Callback Functions

```typescript
// Callback with typed parameters
function processArray(arr: number[], callback: (item: number) => void): void {
    arr.forEach(callback);
}

processArray([1, 2, 3], (num) => {
    console.log(num * 2);
});

// Callback with return value
function mapArray<T, U>(arr: T[], callback: (item: T) => U): U[] {
    return arr.map(callback);
}

const doubled = mapArray([1, 2, 3], (n) => n * 2);
console.log(doubled); // [2, 4, 6]
```

---

## Interfaces

### Basic Interfaces

```typescript
// Interface for object shape
interface User {
    id: number;
    name: string;
    email: string;
    age?: number; // Optional property
}

const user: User = {
    id: 1,
    name: "Alice",
    email: "alice@example.com"
};

// Function that uses the interface
function displayUser(user: User): void {
    console.log(`${user.name} (${user.email})`);
}
```

### Readonly Properties

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}

let point: Point = { x: 10, y: 20 };
// point.x = 15; // Error: Cannot assign to 'x'

// ReadonlyArray
let numbers: ReadonlyArray<number> = [1, 2, 3];
// numbers.push(4); // Error: Property 'push' does not exist
```

### Function Interfaces

```typescript
// Interface for function signature
interface SearchFunc {
    (source: string, substring: string): boolean;
}

const mySearch: SearchFunc = (src, sub) => {
    return src.indexOf(sub) > -1;
};

console.log(mySearch("hello world", "world")); // true
```

### Indexable Types

```typescript
// String index signature
interface StringDictionary {
    [key: string]: string;
}

const colors: StringDictionary = {
    red: "#FF0000",
    green: "#00FF00",
    blue: "#0000FF"
};

// Number index signature
interface NumberArray {
    [index: number]: string;
}

const animals: NumberArray = ["cat", "dog", "bird"];
```

### Interface Extension

```typescript
// Extending interfaces
interface Animal {
    name: string;
    age: number;
}

interface Dog extends Animal {
    breed: string;
    bark(): void;
}

const myDog: Dog = {
    name: "Buddy",
    age: 3,
    breed: "Golden Retriever",
    bark() {
        console.log("Woof!");
    }
};

// Multiple interface extension
interface Flyable {
    fly(): void;
}

interface Swimmable {
    swim(): void;
}

interface Duck extends Animal, Flyable, Swimmable {
    quack(): void;
}
```

### Hybrid Types

```typescript
// Interface describing both a function and an object
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    let counter = function(start: number) {
        return `Started at ${start}`;
    } as Counter;
    
    counter.interval = 1000;
    counter.reset = function() {
        console.log("Counter reset");
    };
    
    return counter;
}

const myCounter = getCounter();
console.log(myCounter(10));    // "Started at 10"
console.log(myCounter.interval); // 1000
myCounter.reset();              // "Counter reset"
```

---

## Classes

### Basic Class

```typescript
class Person {
    // Properties
    name: string;
    age: number;

    // Constructor
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    // Method
    greet(): string {
        return `Hello, I'm ${this.name} and I'm ${this.age} years old.`;
    }
}

const alice = new Person("Alice", 25);
console.log(alice.greet());
```

### Access Modifiers

```typescript
class BankAccount {
    public accountNumber: string;      // Accessible everywhere
    private balance: number;            // Only within this class
    protected owner: string;            // Within this class and subclasses

    constructor(accountNumber: string, owner: string, initialBalance: number) {
        this.accountNumber = accountNumber;
        this.owner = owner;
        this.balance = initialBalance;
    }

    public deposit(amount: number): void {
        if (amount > 0) {
            this.balance += amount;
        }
    }

    public getBalance(): number {
        return this.balance;
    }

    private calculateInterest(): number {
        return this.balance * 0.05;
    }
}

const account = new BankAccount("123456", "Alice", 1000);
account.deposit(500);
console.log(account.getBalance()); // 1500
// console.log(account.balance); // Error: Property 'balance' is private
```

### Readonly Properties

```typescript
class Product {
    readonly id: number;
    name: string;

    constructor(id: number, name: string) {
        this.id = id;
        this.name = name;
    }

    updateName(newName: string): void {
        this.name = newName;
        // this.id = 123; // Error: Cannot assign to 'id'
    }
}
```

### Parameter Properties

```typescript
// Shorthand for declaring and initializing properties
class User {
    constructor(
        public id: number,
        public name: string,
        private password: string
    ) {}

    authenticate(inputPassword: string): boolean {
        return this.password === inputPassword;
    }
}

const user = new User(1, "Alice", "secret123");
console.log(user.name); // "Alice"
// console.log(user.password); // Error: Property 'password' is private
```

### Getters and Setters

```typescript
class Employee {
    private _salary: number = 0;

    get salary(): number {
        return this._salary;
    }

    set salary(value: number) {
        if (value < 0) {
            throw new Error("Salary cannot be negative");
        }
        this._salary = value;
    }
}

const emp = new Employee();
emp.salary = 50000;
console.log(emp.salary); // 50000
// emp.salary = -1000; // Throws error
```

### Static Members

```typescript
class MathHelper {
    static PI: number = 3.14159;

    static calculateCircleArea(radius: number): number {
        return this.PI * radius * radius;
    }

    static calculateCircleCircumference(radius: number): number {
        return 2 * this.PI * radius;
    }
}

console.log(MathHelper.PI);                          // 3.14159
console.log(MathHelper.calculateCircleArea(5));      // 78.53975
```

### Inheritance

```typescript
// Base class
class Animal {
    constructor(public name: string) {}

    move(distance: number = 0): void {
        console.log(`${this.name} moved ${distance}m.`);
    }
}

// Derived class
class Dog extends Animal {
    constructor(name: string, public breed: string) {
        super(name); // Call parent constructor
    }

    bark(): void {
        console.log("Woof! Woof!");
    }

    // Override parent method
    move(distance: number = 5): void {
        console.log("Running...");
        super.move(distance);
    }
}

const dog = new Dog("Buddy", "Golden Retriever");
dog.bark();      // "Woof! Woof!"
dog.move(10);    // "Running..." then "Buddy moved 10m."
```

### Abstract Classes

```typescript
// Abstract classes cannot be instantiated directly
abstract class Shape {
    constructor(public color: string) {}

    abstract calculateArea(): number;

    // Concrete method
    describe(): string {
        return `A ${this.color} shape with area ${this.calculateArea()}`;
    }
}

class Circle extends Shape {
    constructor(color: string, public radius: number) {
        super(color);
    }

    calculateArea(): number {
        return Math.PI * this.radius * this.radius;
    }
}

class Rectangle extends Shape {
    constructor(color: string, public width: number, public height: number) {
        super(color);
    }

    calculateArea(): number {
        return this.width * this.height;
    }
}

const circle = new Circle("red", 5);
console.log(circle.describe());
// const shape = new Shape("blue"); // Error: Cannot create instance of abstract class
```

### Implementing Interfaces

```typescript
interface Drivable {
    speed: number;
    drive(): void;
    stop(): void;
}

class Car implements Drivable {
    speed: number = 0;

    constructor(public brand: string, public model: string) {}

    drive(): void {
        this.speed = 60;
        console.log(`${this.brand} ${this.model} is driving at ${this.speed} km/h`);
    }

    stop(): void {
        this.speed = 0;
        console.log(`${this.brand} ${this.model} has stopped`);
    }
}

const myCar = new Car("Toyota", "Camry");
myCar.drive();
myCar.stop();
```

---

## Generics

### Generic Functions

```typescript
// Generic function
function identity<T>(arg: T): T {
    return arg;
}

let output1 = identity<string>("hello");
let output2 = identity<number>(42);
let output3 = identity({ name: "Alice" }); // Type inference

// Generic array function
function getFirstElement<T>(arr: T[]): T | undefined {
    return arr[0];
}

console.log(getFirstElement([1, 2, 3]));        // 1
console.log(getFirstElement(["a", "b", "c"]));  // "a"
```

### Generic Interfaces

```typescript
interface Box<T> {
    value: T;
}

let stringBox: Box<string> = { value: "hello" };
let numberBox: Box<number> = { value: 42 };

// Generic interface for API response
interface ApiResponse<T> {
    data: T;
    status: number;
    message: string;
}

interface User {
    id: number;
    name: string;
}

const userResponse: ApiResponse<User> = {
    data: { id: 1, name: "Alice" },
    status: 200,
    message: "Success"
};

const usersResponse: ApiResponse<User[]> = {
    data: [
        { id: 1, name: "Alice" },
        { id: 2, name: "Bob" }
    ],
    status: 200,
    message: "Success"
};
```

### Generic Classes

```typescript
class DataStore<T> {
    private data: T[] = [];

    add(item: T): void {
        this.data.push(item);
    }

    get(index: number): T | undefined {
        return this.data[index];
    }

    getAll(): T[] {
        return [...this.data];
    }

    remove(index: number): void {
        this.data.splice(index, 1);
    }
}

const numberStore = new DataStore<number>();
numberStore.add(1);
numberStore.add(2);
console.log(numberStore.getAll()); // [1, 2]

const userStore = new DataStore<User>();
userStore.add({ id: 1, name: "Alice" });
userStore.add({ id: 2, name: "Bob" });
console.log(userStore.getAll());
```

### Generic Constraints

```typescript
// Constraint: T must have a length property
interface Lengthwise {
    length: number;
}

function logLength<T extends Lengthwise>(arg: T): void {
    console.log(arg.length);
}

logLength("hello");           // 5
logLength([1, 2, 3]);         // 3
logLength({ length: 10 });    // 10
// logLength(42);             // Error: number doesn't have length

// Using type parameters in generic constraints
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const person = { name: "Alice", age: 25 };
const name = getProperty(person, "name");  // "Alice"
const age = getProperty(person, "age");    // 25
// const invalid = getProperty(person, "email"); // Error
```

### Multiple Type Parameters

```typescript
function merge<T, U>(obj1: T, obj2: U): T & U {
    return { ...obj1, ...obj2 };
}

const merged = merge(
    { name: "Alice" },
    { age: 25, email: "alice@example.com" }
);

console.log(merged); // { name: "Alice", age: 25, email: "alice@example.com" }

// Generic with multiple constraints
function create<T extends { new(...args: any[]): any }, U>(
    constructor: T,
    ...args: U[]
): InstanceType<T> {
    return new constructor(...args);
}
```

---

## Type Manipulation

### Union Types

```typescript
// Variable can be one of multiple types
type StringOrNumber = string | number;

let value: StringOrNumber;
value = "hello";
value = 42;

// Function with union type parameter
function formatId(id: string | number): string {
    if (typeof id === "string") {
        return id.toUpperCase();
    } else {
        return `ID-${id}`;
    }
}

console.log(formatId("abc123"));  // "ABC123"
console.log(formatId(456));       // "ID-456"

// Union of literal types
type Status = "loading" | "success" | "error";

function handleStatus(status: Status): void {
    switch (status) {
        case "loading":
            console.log("Loading...");
            break;
        case "success":
            console.log("Success!");
            break;
        case "error":
            console.log("Error occurred");
            break;
    }
}
```

### Intersection Types

```typescript
// Combine multiple types
interface Nameable {
    name: string;
}

interface Ageable {
    age: number;
}

type Person = Nameable & Ageable;

const person: Person = {
    name: "Alice",
    age: 25
};

// Combining interfaces and types
interface ContactInfo {
    email: string;
    phone: string;
}

type Employee = Person & ContactInfo & {
    employeeId: number;
    department: string;
};

const employee: Employee = {
    name: "Bob",
    age: 30,
    email: "bob@company.com",
    phone: "123-456-7890",
    employeeId: 1001,
    department: "Engineering"
};
```

### Type Aliases

```typescript
// Simple type alias
type ID = string | number;
type Point = { x: number; y: number };

// Complex type alias
type User = {
    id: ID;
    name: string;
    email: string;
    role: "admin" | "user" | "guest";
    preferences?: {
        theme: "light" | "dark";
        notifications: boolean;
    };
};

const user: User = {
    id: "user-123",
    name: "Alice",
    email: "alice@example.com",
    role: "admin"
};

// Function type alias
type MathOperation = (a: number, b: number) => number;

const add: MathOperation = (a, b) => a + b;
const multiply: MathOperation = (a, b) => a * b;
```

### Type Guards

```typescript
// typeof type guard
function processValue(value: string | number): void {
    if (typeof value === "string") {
        console.log(value.toUpperCase());
    } else {
        console.log(value.toFixed(2));
    }
}

// instanceof type guard
class Dog {
    bark() { console.log("Woof!"); }
}

class Cat {
    meow() { console.log("Meow!"); }
}

function makeSound(animal: Dog | Cat): void {
    if (animal instanceof Dog) {
        animal.bark();
    } else {
        animal.meow();
    }
}

// Custom type guard
interface Fish {
    swim(): void;
}

interface Bird {
    fly(): void;
}

function isFish(pet: Fish | Bird): pet is Fish {
    return (pet as Fish).swim !== undefined;
}

function move(pet: Fish | Bird): void {
    if (isFish(pet)) {
        pet.swim();
    } else {
        pet.fly();
    }
}
```

### Mapped Types

```typescript
// Make all properties optional
type Partial<T> = {
    [P in keyof T]?: T[P];
};

interface User {
    id: number;
    name: string;
    email: string;
}

type PartialUser = Partial<User>;
// { id?: number; name?: string; email?: string; }

// Make all properties readonly
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

type ReadonlyUser = Readonly<User>;

// Custom mapped type
type Nullable<T> = {
    [P in keyof T]: T[P] | null;
};

type NullableUser = Nullable<User>;
// { id: number | null; name: string | null; email: string | null; }

// Pick specific properties
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};

type UserPreview = Pick<User, "id" | "name">;
// { id: number; name: string; }
```

### Conditional Types

```typescript
// Basic conditional type
type IsString<T> = T extends string ? true : false;

type A = IsString<string>;  // true
type B = IsString<number>;  // false

// Extract type from Promise
type Unwrap<T> = T extends Promise<infer U> ? U : T;

type C = Unwrap<Promise<string>>;  // string
type D = Unwrap<number>;           // number

// Practical example
type NonNullable<T> = T extends null | undefined ? never : T;

type E = NonNullable<string | null>;  // string
type F = NonNullable<number | undefined>;  // number
```

### Utility Types

```typescript
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}

// Partial - makes all properties optional
type PartialTodo = Partial<Todo>;

// Required - makes all properties required
type RequiredTodo = Required<PartialTodo>;

// Readonly - makes all properties readonly
type ReadonlyTodo = Readonly<Todo>;

// Record - constructs object type
type TodoRecord = Record<string, Todo>;

const todos: TodoRecord = {
    "todo1": { title: "Learn TS", description: "Study TypeScript", completed: false },
    "todo2": { title: "Build App", description: "Create application", completed: false }
};

// Pick - select specific properties
type TodoPreview = Pick<Todo, "title" | "completed">;

// Omit - exclude specific properties
type TodoInfo = Omit<Todo, "completed">;

// Exclude - exclude from union
type T1 = Exclude<"a" | "b" | "c", "a">;  // "b" | "c"

// Extract - extract from union
type T2 = Extract<"a" | "b" | "c", "a" | "f">;  // "a"

// ReturnType - get function return type
function createUser() {
    return { id: 1, name: "Alice" };
}

type UserReturnType = ReturnType<typeof createUser>;
// { id: number; name: string; }
```

---

## Enums

### Numeric Enums

```typescript
enum Direction {
    Up,      // 0
    Down,    // 1
    Left,    // 2
    Right    // 3
}

let dir: Direction = Direction.Up;
console.log(dir); // 0

// Custom starting value
enum Status {
    Pending = 1,
    Approved,      // 2
    Rejected       // 3
}

// Fully initialized
enum HttpStatus {
    OK = 200,
    BadRequest = 400,
    Unauthorized = 401,
    NotFound = 404,
    InternalServerError = 500
}

function handleResponse(status: HttpStatus): string {
    switch (status) {
        case HttpStatus.OK:
            return "Request successful";
        case HttpStatus.NotFound:
            return "Resource not found";
        default:
            return "Unknown status";
    }
}
```

### String Enums

```typescript
enum Color {
    Red = "RED",
    Green = "GREEN",
    Blue = "BLUE"
}

let favoriteColor: Color = Color.Red;
console.log(favoriteColor); // "RED"

// Practical example
enum LogLevel {
    Error = "ERROR",
    Warning = "WARNING",
    Info = "INFO",
    Debug = "DEBUG"
}

function log(level: LogLevel, message: string): void {
    console.log(`[${level}] ${message}`);
}

log(LogLevel.Error, "An error occurred");
log(LogLevel.Info, "Application started");
```

### Const Enums

```typescript
// Const enums are completely removed during compilation
const enum Size {
    Small,
    Medium,
    Large
}

let shirtSize: Size = Size.Medium;

// Compiles to: let shirtSize = 1;
// The enum is inlined and removed
```

### Heterogeneous Enums

```typescript
// Mix of string and numeric values (not recommended)
enum Mixed {
    No = 0,
    Yes = "YES"
}
```

---

## Advanced Types

### Index Types

```typescript
// Index type query (keyof)
interface Person {
    name: string;
    age: number;
    email: string;
}

type PersonKeys = keyof Person;  // "name" | "age" | "email"

// Index access types
type PersonName = Person["name"];  // string
type PersonAge = Person["age"];    // number

// Using with generics
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const person: Person = { name: "Alice", age: 25, email: "alice@example.com" };
const name = getProperty(person, "name");  // string
const age = getProperty(person, "age");    // number
```

### Template Literal Types

```typescript
// Template literal types (TypeScript 4.1+)
type EmailTemplate = `${string}@${string}.${string}`;

const validEmail: EmailTemplate = "user@example.com";
// const invalidEmail: EmailTemplate = "not-an-email"; // Error

// Combining with unions
type Direction = "top" | "bottom" | "left" | "right";
type Alignment = "start" | "center" | "end";
type Position = `${Direction}-${Alignment}`;

// Results in:
// "top-start" | "top-center" | "top-end" |
// "bottom-start" | "bottom-center" | "bottom-end" |
// "left-start" | "left-center" | "left-end" |
// "right-start" | "right-center" | "right-end"

const position: Position = "top-center";

// Practical example
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE";
type APIEndpoint = `/api/${string}`;
type APIRoute = `${HttpMethod} ${APIEndpoint}`;

const route: APIRoute = "GET /api/users";
```

### Discriminated Unions

```typescript
// Also known as tagged unions
interface Circle {
    kind: "circle";
    radius: number;
}

interface Square {
    kind: "square";
    sideLength: number;
}

interface Rectangle {
    kind: "rectangle";
    width: number;
    height: number;
}

type Shape = Circle | Square | Rectangle;

function calculateArea(shape: Shape): number {
    switch (shape.kind) {
        case "circle":
            return Math.PI * shape.radius ** 2;
        case "square":
            return shape.sideLength ** 2;
        case "rectangle":
            return shape.width * shape.height;
    }
}

const circle: Circle = { kind: "circle", radius: 5 };
const square: Square = { kind: "square", sideLength: 10 };

console.log(calculateArea(circle));  // 78.54
console.log(calculateArea(square));  // 100
```

### Recursive Types

```typescript
// Recursive type for nested structures
type NestedArray<T> = T | NestedArray<T>[];

const nested: NestedArray<number> = [1, [2, 3], [[4, 5], 6]];

// JSON type
type JSONValue =
    | string
    | number
    | boolean
    | null
    | JSONValue[]
    | { [key: string]: JSONValue };

const data: JSONValue = {
    name: "Alice",
    age: 25,
    hobbies: ["reading", "coding"],
    address: {
        city: "New York",
        zipCode: 10001
    }
};

// Tree structure
interface TreeNode<T> {
    value: T;
    children?: TreeNode<T>[];
}

const tree: TreeNode<number> = {
    value: 1,
    children: [
        { value: 2 },
        { value: 3, children: [{ value: 4 }, { value: 5 }] }
    ]
};
```

---

## Modules

### ES6 Modules

```typescript
// math.ts - Exporting
export function add(a: number, b: number): number {
    return a + b;
}

export function subtract(a: number, b: number): number {
    return a - b;
}

export const PI = 3.14159;

// Default export
export default function multiply(a: number, b: number): number {
    return a * b;
}

// app.ts - Importing
import multiply, { add, subtract, PI } from './math';

console.log(add(5, 3));        // 8
console.log(multiply(4, 2));   // 8
console.log(PI);               // 3.14159

// Import all
import * as MathUtils from './math';

console.log(MathUtils.add(10, 5));
console.log(MathUtils.default(3, 7));
```

### Named Exports

```typescript
// types.ts
export interface User {
    id: number;
    name: string;
    email: string;
}

export type UserRole = "admin" | "user" | "guest";

export class UserService {
    getUser(id: number): User | undefined {
        // Implementation
        return undefined;
    }
}

// main.ts
import { User, UserRole, UserService } from './types';

const user: User = { id: 1, name: "Alice", email: "alice@example.com" };
const role: UserRole = "admin";
const service = new UserService();
```

### Re-exporting

```typescript
// models/index.ts
export { User, UserRole } from './user';
export { Product, ProductCategory } from './product';
export { Order, OrderStatus } from './order';

// app.ts
import { User, Product, Order } from './models';
```

### Namespace

```typescript
// Legacy module system (prefer ES6 modules)
namespace Validation {
    export interface StringValidator {
        isValid(s: string): boolean;
    }

    export class EmailValidator implements StringValidator {
        isValid(s: string): boolean {
            return s.includes("@");
        }
    }

    export class URLValidator implements StringValidator {
        isValid(s: string): boolean {
            return s.startsWith("http");
        }
    }
}

const emailValidator = new Validation.EmailValidator();
console.log(emailValidator.isValid("test@example.com")); // true
```

---

## Decorators

**Note:** Decorators are an experimental feature. Enable with `"experimentalDecorators": true` in tsconfig.json.

### Class Decorators

```typescript
function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}

@sealed
class Person {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
}

// Class decorator factory
function Component(config: { selector: string }) {
    return function(constructor: Function) {
        console.log(`Component ${config.selector} created`);
    };
}

@Component({ selector: "app-root" })
class AppComponent {
    title = "My App";
}
```

### Method Decorators

```typescript
function log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;

    descriptor.value = function(...args: any[]) {
        console.log(`Calling ${propertyKey} with args: ${JSON.stringify(args)}`);
        const result = originalMethod.apply(this, args);
        console.log(`Result: ${result}`);
        return result;
    };

    return descriptor;
}

class Calculator {
    @log
    add(a: number, b: number): number {
        return a + b;
    }
}

const calc = new Calculator();
calc.add(2, 3);
// Logs: "Calling add with args: [2,3]"
// Logs: "Result: 5"
```

### Property Decorators

```typescript
function readonly(target: any, propertyKey: string) {
    Object.defineProperty(target, propertyKey, {
        writable: false
    });
}

class Book {
    @readonly
    isbn: string = "978-3-16-148410-0";
}

const book = new Book();
// book.isbn = "new-isbn"; // Error in runtime
```

### Parameter Decorators

```typescript
function required(target: Object, propertyKey: string, parameterIndex: number) {
    console.log(`Parameter ${parameterIndex} of ${propertyKey} is required`);
}

class UserService {
    createUser(@required name: string, @required email: string, age?: number) {
        // Implementation
    }
}
```

---

## Asynchronous Programming

### Promises

```typescript
// Creating a promise
function fetchUser(id: number): Promise<User> {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (id > 0) {
                resolve({ id, name: "Alice", email: "alice@example.com" });
            } else {
                reject(new Error("Invalid user ID"));
            }
        }, 1000);
    });
}

// Using promises
fetchUser(1)
    .then(user => {
        console.log(user.name);
    })
    .catch(error => {
        console.error(error.message);
    });

// Chaining promises
function fetchUserPosts(userId: number): Promise<string[]> {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve(["Post 1", "Post 2", "Post 3"]);
        }, 500);
    });
}

fetchUser(1)
    .then(user => {
        console.log(`Fetching posts for ${user.name}`);
        return fetchUserPosts(user.id);
    })
    .then(posts => {
        console.log(posts);
    })
    .catch(error => {
        console.error(error);
    });
```

### Async/Await

```typescript
// Async function
async function getUserData(id: number): Promise<void> {
    try {
        const user = await fetchUser(id);
        console.log(`User: ${user.name}`);
        
        const posts = await fetchUserPosts(user.id);
        console.log(`Posts: ${posts.length}`);
    } catch (error) {
        console.error("Error:", error);
    }
}

getUserData(1);

// Async function with return value
async function fetchAndProcessUser(id: number): Promise<string> {
    const user = await fetchUser(id);
    const posts = await fetchUserPosts(user.id);
    return `${user.name} has ${posts.length} posts`;
}

fetchAndProcessUser(1).then(result => console.log(result));
```

### Parallel Execution

```typescript
// Promise.all - wait for all promises
async function fetchMultipleUsers(ids: number[]): Promise<User[]> {
    const promises = ids.map(id => fetchUser(id));
    return Promise.all(promises);
}

fetchMultipleUsers([1, 2, 3]).then(users => {
    console.log(`Fetched ${users.length} users`);
});

// Promise.race - wait for first to complete
async function fetchWithTimeout<T>(
    promise: Promise<T>,
    timeout: number
): Promise<T> {
    const timeoutPromise = new Promise<T>((_, reject) => {
        setTimeout(() => reject(new Error("Timeout")), timeout);
    });
    
    return Promise.race([promise, timeoutPromise]);
}

// Promise.allSettled - wait for all, even if some fail
async function fetchUsersAllSettled(ids: number[]): Promise<void> {
    const promises = ids.map(id => fetchUser(id));
    const results = await Promise.allSettled(promises);
    
    results.forEach((result, index) => {
        if (result.status === "fulfilled") {
            console.log(`User ${ids[index]}: ${result.value.name}`);
        } else {
            console.error(`User ${ids[index]} failed: ${result.reason}`);
        }
    });
}
```

### Async Iterators

```typescript
// Async generator
async function* numberGenerator(max: number): AsyncGenerator<number> {
    for (let i = 0; i < max; i++) {
        await new Promise(resolve => setTimeout(resolve, 100));
        yield i;
    }
}

// Using async iterator
async function processNumbers(): Promise<void> {
    for await (const num of numberGenerator(5)) {
        console.log(num);
    }
}

processNumbers();
```

---

## TypeScript with React

### Component Props

```typescript
// Functional component with typed props
interface ButtonProps {
    label: string;
    onClick: () => void;
    variant?: "primary" | "secondary";
    disabled?: boolean;
}

const Button: React.FC<ButtonProps> = ({ 
    label, 
    onClick, 
    variant = "primary", 
    disabled = false 
}) => {
    return (
        <button 
            onClick={onClick} 
            disabled={disabled}
            className={`btn btn-${variant}`}
        >
            {label}
        </button>
    );
};

// Using the component
<Button label="Click Me" onClick={() => console.log("Clicked")} />
```

### Component with Children

```typescript
interface CardProps {
    title: string;
    children: React.ReactNode;
}

const Card: React.FC<CardProps> = ({ title, children }) => {
    return (
        <div className="card">
            <h2>{title}</h2>
            <div className="card-content">{children}</div>
        </div>
    );
};

// Usage
<Card title="My Card">
    <p>Card content goes here</p>
</Card>
```

### State and Hooks

```typescript
import { useState, useEffect } from 'react';

interface User {
    id: number;
    name: string;
    email: string;
}

const UserProfile: React.FC = () => {
    // useState with type inference
    const [count, setCount] = useState(0);
    
    // useState with explicit type
    const [user, setUser] = useState<User | null>(null);
    
    // useState with array
    const [users, setUsers] = useState<User[]>([]);

    useEffect(() => {
        // Fetch user data
        fetchUser(1).then(userData => {
            setUser(userData);
        });
    }, []);

    return (
        <div>
            <p>Count: {count}</p>
            {user && <p>User: {user.name}</p>}
        </div>
    );
};
```

### Event Handlers

```typescript
const FormComponent: React.FC = () => {
    const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
        console.log("Button clicked", event.currentTarget);
    };

    const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
        console.log("Input value:", event.target.value);
    };

    const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
        event.preventDefault();
        console.log("Form submitted");
    };

    return (
        <form onSubmit={handleSubmit}>
            <input type="text" onChange={handleChange} />
            <button onClick={handleClick}>Submit</button>
        </form>
    );
};
```

### Custom Hooks

```typescript
// Custom hook with TypeScript
function useLocalStorage<T>(key: string, initialValue: T) {
    const [storedValue, setStoredValue] = useState<T>(() => {
        try {
            const item = window.localStorage.getItem(key);
            return item ? JSON.parse(item) : initialValue;
        } catch (error) {
            console.error(error);
            return initialValue;
        }
    });

    const setValue = (value: T | ((val: T) => T)) => {
        try {
            const valueToStore = value instanceof Function ? value(storedValue) : value;
            setStoredValue(valueToStore);
            window.localStorage.setItem(key, JSON.stringify(valueToStore));
        } catch (error) {
            console.error(error);
        }
    };

    return [storedValue, setValue] as const;
}

// Usage
const [name, setName] = useLocalStorage<string>("name", "");
```

### Context API

```typescript
import { createContext, useContext, useState } from 'react';

interface AuthContextType {
    user: User | null;
    login: (user: User) => void;
    logout: () => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export const AuthProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
    const [user, setUser] = useState<User | null>(null);

    const login = (user: User) => {
        setUser(user);
    };

    const logout = () => {
        setUser(null);
    };

    return (
        <AuthContext.Provider value={{ user, login, logout }}>
            {children}
        </AuthContext.Provider>
    );
};

export const useAuth = () => {
    const context = useContext(AuthContext);
    if (context === undefined) {
        throw new Error("useAuth must be used within AuthProvider");
    }
    return context;
};

// Usage
const LoginButton: React.FC = () => {
    const { login } = useAuth();
    
    return (
        <button onClick={() => login({ id: 1, name: "Alice", email: "alice@example.com" })}>
            Login
        </button>
    );
};
```

---

## Best Practices

### 1. Use Strict Mode

```typescript
// tsconfig.json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictBindCallApply": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "alwaysStrict": true
  }
}
```

### 2. Prefer Interfaces for Object Shapes

```typescript
// Good: Use interface for object shapes
interface User {
    id: number;
    name: string;
    email: string;
}

// Use type for unions, intersections, and primitives
type ID = string | number;
type UserRole = "admin" | "user";
```

### 3. Avoid 'any' Type

```typescript
// Bad
function processData(data: any) {
    return data.value;
}

// Good: Use unknown and type guards
function processData(data: unknown) {
    if (typeof data === "object" && data !== null && "value" in data) {
        return (data as { value: any }).value;
    }
    throw new Error("Invalid data");
}

// Better: Use proper types
interface DataWithValue {
    value: string;
}

function processData(data: DataWithValue) {
    return data.value;
}
```

### 4. Use const Assertions

```typescript
// Inferred as string
let color1 = "red";

// Inferred as "red" (literal type)
const color2 = "red";

// const assertion
let color3 = "red" as const;

// Object with const assertion
const config = {
    apiUrl: "https://api.example.com",
    timeout: 3000
} as const;

// config.apiUrl = "new"; // Error: readonly
```

### 5. Leverage Utility Types

```typescript
interface User {
    id: number;
    name: string;
    email: string;
    password: string;
}

// Only pick needed properties
type UserDTO = Pick<User, "id" | "name" | "email">;

// Omit sensitive data
type PublicUser = Omit<User, "password">;

// Make all properties optional for updates
type UserUpdate = Partial<User>;

// Make all properties required
type CompleteUser = Required<User>;
```

### 6. Use Readonly for Immutability

```typescript
interface Config {
    readonly apiKey: string;
    readonly endpoint: string;
}

const config: Config = {
    apiKey: "secret",
    endpoint: "https://api.example.com"
};

// config.apiKey = "new"; // Error

// Readonly arrays
const numbers: ReadonlyArray<number> = [1, 2, 3];
// numbers.push(4); // Error
```

### 7. Proper Error Handling

```typescript
class ValidationError extends Error {
    constructor(public field: string, message: string) {
        super(message);
        this.name = "ValidationError";
    }
}

function validateEmail(email: string): void {
    if (!email.includes("@")) {
        throw new ValidationError("email", "Invalid email format");
    }
}

try {
    validateEmail("invalid");
} catch (error) {
    if (error instanceof ValidationError) {
        console.error(`${error.field}: ${error.message}`);
    } else {
        console.error("Unknown error:", error);
    }
}
```

### 8. Use Enums for Fixed Sets

```typescript
// Good: Use enum for fixed set of values
enum UserRole {
    Admin = "ADMIN",
    User = "USER",
    Guest = "GUEST"
}

function checkPermission(role: UserRole): boolean {
    return role === UserRole.Admin;
}

// Alternative: Use const object with as const
const UserRole = {
    Admin: "ADMIN",
    User: "USER",
    Guest: "GUEST"
} as const;

type UserRole = typeof UserRole[keyof typeof UserRole];
```

### 9. Type Your API Responses

```typescript
interface ApiResponse<T> {
    data: T;
    status: number;
    message: string;
}

interface User {
    id: number;
    name: string;
    email: string;
}

async function fetchUser(id: number): Promise<ApiResponse<User>> {
    const response = await fetch(`/api/users/${id}`);
    return response.json();
}

async function fetchUsers(): Promise<ApiResponse<User[]>> {
    const response = await fetch("/api/users");
    return response.json();
}
```

### 10. Document Complex Types

```typescript
/**
 * Represents a user in the system
 * @property id - Unique identifier
 * @property name - Full name of the user
 * @property email - Email address (must be unique)
 * @property role - User's role in the system
 * @property createdAt - Timestamp of user creation
 */
interface User {
    id: number;
    name: string;
    email: string;
    role: UserRole;
    createdAt: Date;
}

/**
 * Fetches a user by ID
 * @param id - The user's unique identifier
 * @returns Promise resolving to the user object
 * @throws {Error} If user not found
 */
async function getUser(id: number): Promise<User> {
    // Implementation
    throw new Error("Not implemented");
}
```

---

## Conclusion

This handbook covers the essential concepts of TypeScript, from basic types to advanced features. TypeScript's type system helps catch errors early, improves code quality, and enhances developer productivity.

### Next Steps

1. Practice writing TypeScript code regularly
2. Convert existing JavaScript projects to TypeScript
3. Explore advanced type system features
4. Learn TypeScript with your favorite framework (React, Angular, Vue, etc.)
5. Contribute to open-source TypeScript projects

### Additional Resources

- Official TypeScript Documentation: https://www.typescriptlang.org/docs/
- TypeScript Playground: https://www.typescriptlang.org/play
- DefinitelyTyped: https://github.com/DefinitelyTyped/DefinitelyTyped
- TypeScript Deep Dive: https://basarat.gitbook.io/typescript/

---

**Happy Coding with TypeScript! 🚀**