# TypeScript Practice Questions - 20 Comprehensive Coding Challenges

This document contains 20 TypeScript coding questions covering all major topics from basics to advanced concepts. Each question includes a detailed solution with explanations.

---

## Question 1: Type Annotations and Basic Types

**Topic:** Basic Types

**Question:** Create a function that takes a person's name (string), age (number), and whether they are a student (boolean), and returns a formatted string describing them.

**Solution:**
```typescript
function describePerson(name: string, age: number, isStudent: boolean): string {
  const studentStatus = isStudent ? "is" : "is not";
  return `${name} is ${age} years old and ${studentStatus} a student.`;
}

// Usage
console.log(describePerson("Alice", 25, true));
// Output: "Alice is 25 years old and is a student."
```

**Explanation:** This demonstrates basic type annotations for primitive types (string, number, boolean) and a return type annotation.

---

## Question 2: Interfaces

**Topic:** Interfaces

**Question:** Define an interface for a `Book` with properties: title, author, year, and an optional ISBN. Create a function that accepts a Book and returns whether it's a classic (published before 1950).

**Solution:**
```typescript
interface Book {
  title: string;
  author: string;
  year: number;
  isbn?: string; // Optional property
}

function isClassic(book: Book): boolean {
  return book.year < 1950;
}

// Usage
const book1: Book = {
  title: "1984",
  author: "George Orwell",
  year: 1949,
  isbn: "978-0451524935"
};

console.log(isClassic(book1)); // Output: true
```

**Explanation:** Interfaces define the shape of objects. The `?` symbol marks optional properties.

---

## Question 3: Type Aliases and Union Types

**Topic:** Type Aliases, Union Types

**Question:** Create a type alias for different payment methods (cash, credit card, or PayPal). Write a function that processes payments and returns a confirmation message based on the payment method.

**Solution:**
```typescript
type PaymentMethod = "cash" | "credit_card" | "paypal";

function processPayment(amount: number, method: PaymentMethod): string {
  switch (method) {
    case "cash":
      return `Received $${amount} in cash`;
    case "credit_card":
      return `Charged $${amount} to credit card`;
    case "paypal":
      return `Processed $${amount} via PayPal`;
  }
}

// Usage
console.log(processPayment(100, "paypal"));
// Output: "Processed $100 via PayPal"
```

**Explanation:** Union types allow a value to be one of several types. String literal types create a set of allowed string values.

---

## Question 4: Arrays and Tuples

**Topic:** Arrays, Tuples

**Question:** Create a function that takes an array of numbers and returns a tuple containing the minimum and maximum values.

**Solution:**
```typescript
function findMinMax(numbers: number[]): [number, number] {
  if (numbers.length === 0) {
    throw new Error("Array cannot be empty");
  }
  
  const min = Math.min(...numbers);
  const max = Math.max(...numbers);
  
  return [min, max];
}

// Usage
const result: [number, number] = findMinMax([5, 2, 9, 1, 7]);
console.log(result); // Output: [1, 9]
console.log(`Min: ${result[0]}, Max: ${result[1]}`);
```

**Explanation:** Arrays hold multiple values of the same type. Tuples are fixed-length arrays where each element can have a different type at a specific position.

---

## Question 5: Enums

**Topic:** Enums

**Question:** Create an enum for days of the week and a function that determines if a day is a weekend.

**Solution:**
```typescript
enum DayOfWeek {
  Monday,
  Tuesday,
  Wednesday,
  Thursday,
  Friday,
  Saturday,
  Sunday
}

function isWeekend(day: DayOfWeek): boolean {
  return day === DayOfWeek.Saturday || day === DayOfWeek.Sunday;
}

// Usage
console.log(isWeekend(DayOfWeek.Saturday)); // Output: true
console.log(isWeekend(DayOfWeek.Monday));   // Output: false

// String enums example
enum Status {
  Active = "ACTIVE",
  Inactive = "INACTIVE",
  Pending = "PENDING"
}
```

**Explanation:** Enums allow you to define a set of named constants. By default, numeric enums start at 0.

---

## Question 6: Generics - Basic

**Topic:** Generics

**Question:** Create a generic function that returns the first element of an array, regardless of type.

**Solution:**
```typescript
function getFirstElement<T>(arr: T[]): T | undefined {
  return arr[0];
}

// Usage
const firstNumber = getFirstElement<number>([1, 2, 3]); // Type: number | undefined
const firstString = getFirstElement(["hello", "world"]); // Type inference: string | undefined
const firstBool = getFirstElement<boolean>([]); // Type: boolean | undefined

console.log(firstNumber); // Output: 1
console.log(firstString); // Output: "hello"
console.log(firstBool);   // Output: undefined
```

**Explanation:** Generics allow you to write reusable code that works with multiple types while maintaining type safety.

---

## Question 7: Generics - Advanced

**Topic:** Generics with Constraints

**Question:** Create a generic function that merges two objects, ensuring both parameters have an `id` property.

**Solution:**
```typescript
interface HasId {
  id: number;
}

function mergeObjects<T extends HasId, U extends HasId>(obj1: T, obj2: U): T & U {
  return { ...obj1, ...obj2 };
}

// Usage
const person = { id: 1, name: "John" };
const employee = { id: 1, department: "IT", salary: 50000 };

const merged = mergeObjects(person, employee);
console.log(merged);
// Output: { id: 1, name: "John", department: "IT", salary: 50000 }

// This would cause a TypeScript error:
// const invalid = mergeObjects({ name: "Jane" }, { age: 30 });
// Error: Argument of type '{ name: string; }' is not assignable to parameter of type 'HasId'
```

**Explanation:** Generic constraints (`extends`) ensure that generic types meet certain requirements. The `&` operator creates intersection types.

---

## Question 8: Classes and Access Modifiers

**Topic:** Classes, Access Modifiers

**Question:** Create a `BankAccount` class with private balance, and public methods to deposit, withdraw, and check balance.

**Solution:**
```typescript
class BankAccount {
  private balance: number;
  public readonly accountNumber: string;

  constructor(initialBalance: number, accountNumber: string) {
    this.balance = initialBalance;
    this.accountNumber = accountNumber;
  }

  public deposit(amount: number): void {
    if (amount > 0) {
      this.balance += amount;
      console.log(`Deposited $${amount}. New balance: $${this.balance}`);
    }
  }

  public withdraw(amount: number): boolean {
    if (amount > 0 && amount <= this.balance) {
      this.balance -= amount;
      console.log(`Withdrew $${amount}. New balance: $${this.balance}`);
      return true;
    }
    console.log("Insufficient funds or invalid amount");
    return false;
  }

  public getBalance(): number {
    return this.balance;
  }
}

// Usage
const account = new BankAccount(1000, "ACC123456");
account.deposit(500);
account.withdraw(200);
console.log(account.getBalance()); // Output: 1300
// account.balance = 5000; // Error: Property 'balance' is private
```

**Explanation:** Access modifiers control visibility: `private` (class only), `public` (everywhere), `protected` (class and subclasses). `readonly` prevents modification after initialization.

---

## Question 9: Inheritance and Abstract Classes

**Topic:** Inheritance, Abstract Classes

**Question:** Create an abstract `Shape` class with an abstract method `calculateArea()`. Implement `Circle` and `Rectangle` subclasses.

**Solution:**
```typescript
abstract class Shape {
  abstract calculateArea(): number;

  describe(): string {
    return `This shape has an area of ${this.calculateArea()} square units.`;
  }
}

class Circle extends Shape {
  constructor(private radius: number) {
    super();
  }

  calculateArea(): number {
    return Math.PI * this.radius ** 2;
  }
}

class Rectangle extends Shape {
  constructor(private width: number, private height: number) {
    super();
  }

  calculateArea(): number {
    return this.width * this.height;
  }
}

// Usage
const circle = new Circle(5);
const rectangle = new Rectangle(4, 6);

console.log(circle.describe());
// Output: "This shape has an area of 78.53981633974483 square units."
console.log(rectangle.describe());
// Output: "This shape has an area of 24 square units."

// const shape = new Shape(); // Error: Cannot create an instance of an abstract class
```

**Explanation:** Abstract classes cannot be instantiated and serve as base classes. Abstract methods must be implemented by subclasses.

---

## Question 10: Type Guards and Type Narrowing

**Topic:** Type Guards, typeof, instanceof

**Question:** Create a function that handles different input types (string, number, array) and processes them accordingly.

**Solution:**
```typescript
function processInput(input: string | number | string[]): string {
  // typeof type guard
  if (typeof input === "string") {
    return `String: ${input.toUpperCase()}`;
  }
  
  // typeof type guard
  if (typeof input === "number") {
    return `Number: ${input * 2}`;
  }
  
  // Array.isArray type guard
  if (Array.isArray(input)) {
    return `Array: ${input.join(", ")}`;
  }
  
  // This line is unreachable due to exhaustive checking
  return "Unknown type";
}

// Custom type guard
interface Cat {
  meow(): void;
}

interface Dog {
  bark(): void;
}

function isCat(pet: Cat | Dog): pet is Cat {
  return (pet as Cat).meow !== undefined;
}

function makeSound(pet: Cat | Dog): void {
  if (isCat(pet)) {
    pet.meow();
  } else {
    pet.bark();
  }
}

// Usage
console.log(processInput("hello"));        // Output: "String: HELLO"
console.log(processInput(10));             // Output: "Number: 20"
console.log(processInput(["a", "b", "c"])); // Output: "Array: a, b, c"
```

**Explanation:** Type guards narrow types within conditional blocks. `typeof`, `instanceof`, and custom type predicates (`is`) are common type guards.

---

## Question 11: Utility Types

**Topic:** Utility Types (Partial, Required, Pick, Omit, Record)

**Question:** Demonstrate the use of utility types to transform and manipulate type definitions.

**Solution:**
```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
  isActive: boolean;
}

// Partial - makes all properties optional
function updateUser(id: number, updates: Partial<User>): void {
  console.log(`Updating user ${id} with:`, updates);
}

updateUser(1, { name: "John Doe" }); // Only updating name is fine

// Pick - select specific properties
type UserPreview = Pick<User, "id" | "name">;
const preview: UserPreview = { id: 1, name: "Alice" };

// Omit - exclude specific properties
type UserWithoutId = Omit<User, "id">;
const newUser: UserWithoutId = {
  name: "Bob",
  email: "bob@example.com",
  age: 30,
  isActive: true
};

// Record - create object type with specific keys and values
type UserRole = "admin" | "user" | "guest";
type Permissions = Record<UserRole, string[]>;

const permissions: Permissions = {
  admin: ["read", "write", "delete"],
  user: ["read", "write"],
  guest: ["read"]
};

// Required - makes all properties required
type RequiredUser = Required<User>;

// Readonly - makes all properties readonly
type ImmutableUser = Readonly<User>;

console.log(permissions.admin); // Output: ["read", "write", "delete"]
```

**Explanation:** Utility types transform existing types. They're built into TypeScript and help avoid repetitive type definitions.

---

## Question 12: Function Overloading

**Topic:** Function Overloading

**Question:** Create an overloaded function that can reverse either a string or an array.

**Solution:**
```typescript
// Overload signatures
function reverse(value: string): string;
function reverse<T>(value: T[]): T[];

// Implementation signature
function reverse<T>(value: string | T[]): string | T[] {
  if (typeof value === "string") {
    return value.split("").reverse().join("");
  } else {
    return value.slice().reverse();
  }
}

// Usage
const reversedString = reverse("hello");
console.log(reversedString); // Output: "olleh"

const reversedArray = reverse([1, 2, 3, 4, 5]);
console.log(reversedArray); // Output: [5, 4, 3, 2, 1]

// TypeScript knows the return type based on the argument
const str: string = reverse("TypeScript"); // Type is string
const arr: number[] = reverse([1, 2, 3]);   // Type is number[]
```

**Explanation:** Function overloading allows multiple function signatures for the same function name. The implementation must handle all overload cases.

---

## Question 13: Mapped Types

**Topic:** Mapped Types

**Question:** Create a mapped type that makes all properties of a type nullable.

**Solution:**
```typescript
// Mapped type to make all properties nullable
type Nullable<T> = {
  [P in keyof T]: T[P] | null;
};

interface Product {
  id: number;
  name: string;
  price: number;
  description: string;
}

type NullableProduct = Nullable<Product>;

// Now all properties can be null
const product: NullableProduct = {
  id: 1,
  name: "Laptop",
  price: null,         // Valid
  description: null    // Valid
};

// Advanced: Make properties optional AND nullable
type PartialNullable<T> = {
  [P in keyof T]?: T[P] | null;
};

// Create a mapped type that converts all functions to return promises
type Promisify<T> = {
  [K in keyof T]: T[K] extends (...args: any[]) => any
    ? (...args: Parameters<T[K]>) => Promise<ReturnType<T[K]>>
    : T[K];
};

interface SyncAPI {
  getUser(id: number): string;
  deleteUser(id: number): boolean;
}

type AsyncAPI = Promisify<SyncAPI>;
// Result:
// {
//   getUser(id: number): Promise<string>;
//   deleteUser(id: number): Promise<boolean>;
// }
```

**Explanation:** Mapped types transform each property in a type. They iterate over keys using `keyof` and modify property types.

---

## Question 14: Conditional Types

**Topic:** Conditional Types

**Question:** Create a conditional type that extracts the return type of a function or returns never if it's not a function.

**Solution:**
```typescript
// Conditional type
type ReturnTypeOf<T> = T extends (...args: any[]) => infer R ? R : never;

// Test functions
function getString(): string {
  return "hello";
}

function getNumber(): number {
  return 42;
}

const notAFunction = "I'm a string";

// Usage
type StringReturn = ReturnTypeOf<typeof getString>;  // Type: string
type NumberReturn = ReturnTypeOf<typeof getNumber>;  // Type: number
type NeverType = ReturnTypeOf<typeof notAFunction>;  // Type: never

// Practical example: Extract array element type
type ArrayElement<T> = T extends (infer U)[] ? U : never;

type Numbers = ArrayElement<number[]>;  // Type: number
type Strings = ArrayElement<string[]>;  // Type: string
type NotArray = ArrayElement<boolean>;  // Type: never

// Another example: NonNullable implementation
type MyNonNullable<T> = T extends null | undefined ? never : T;

type Result = MyNonNullable<string | null | undefined>; // Type: string

const value: Result = "Hello"; // OK
// const invalid: Result = null; // Error
```

**Explanation:** Conditional types select types based on conditions. The `infer` keyword extracts types from within conditional types.

---

## Question 15: Async/Await and Promises

**Topic:** Promises, Async/Await

**Question:** Create an async function that fetches user data and handles errors properly with TypeScript types.

**Solution:**
```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

// Simulated API call
function fetchUserById(id: number): Promise<ApiResponse<User>> {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (id > 0) {
        resolve({
          data: { id, name: "John Doe", email: "john@example.com" },
          status: 200,
          message: "Success"
        });
      } else {
        reject(new Error("Invalid user ID"));
      }
    }, 1000);
  });
}

// Async function with error handling
async function getUserData(id: number): Promise<User | null> {
  try {
    const response = await fetchUserById(id);
    console.log(response.message);
    return response.data;
  } catch (error) {
    if (error instanceof Error) {
      console.error("Error fetching user:", error.message);
    }
    return null;
  }
}

// Multiple async operations
async function getUsersData(ids: number[]): Promise<User[]> {
  const promises = ids.map(id => getUserData(id));
  const results = await Promise.all(promises);
  return results.filter((user): user is User => user !== null);
}

// Usage
(async () => {
  const user = await getUserData(1);
  console.log(user);
  
  const users = await getUsersData([1, 2, 3]);
  console.log(users);
})();
```

**Explanation:** Async/await provides syntactic sugar for Promises. Type annotations ensure type safety with asynchronous operations.

---

## Question 16: Decorators

**Topic:** Decorators (Experimental)

**Question:** Create a method decorator that logs execution time.

**Solution:**
```typescript
// Note: Enable "experimentalDecorators": true in tsconfig.json

// Method decorator
function LogExecutionTime(
  target: any,
  propertyKey: string,
  descriptor: PropertyDescriptor
) {
  const originalMethod = descriptor.value;

  descriptor.value = async function (...args: any[]) {
    const start = performance.now();
    const result = await originalMethod.apply(this, args);
    const end = performance.now();
    console.log(`${propertyKey} executed in ${(end - start).toFixed(2)}ms`);
    return result;
  };

  return descriptor;
}

// Class decorator
function Sealed(constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
}

@Sealed
class DataService {
  @LogExecutionTime
  async fetchData(): Promise<string> {
    // Simulate async operation
    await new Promise(resolve => setTimeout(resolve, 100));
    return "Data loaded";
  }

  @LogExecutionTime
  processData(data: string): string {
    return data.toUpperCase();
  }
}

// Usage
(async () => {
  const service = new DataService();
  await service.fetchData();
  // Output: "fetchData executed in 100.xx ms"
  
  service.processData("hello");
  // Output: "processData executed in 0.xx ms"
})();
```

**Explanation:** Decorators are functions that modify classes, methods, properties, or parameters. They're experimental but widely used in frameworks like Angular.

---

## Question 17: Namespaces and Modules

**Topic:** Namespaces, Modules

**Question:** Create a namespace for validation utilities and demonstrate module exports.

**Solution:**
```typescript
// Namespace approach
namespace Validation {
  export interface StringValidator {
    isValid(s: string): boolean;
  }

  export class EmailValidator implements StringValidator {
    isValid(s: string): boolean {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      return emailRegex.test(s);
    }
  }

  export class URLValidator implements StringValidator {
    isValid(s: string): boolean {
      try {
        new URL(s);
        return true;
      } catch {
        return false;
      }
    }
  }

  export function validate(s: string, validator: StringValidator): boolean {
    return validator.isValid(s);
  }
}

// Usage of namespace
const emailValidator = new Validation.EmailValidator();
console.log(Validation.validate("test@example.com", emailValidator)); // true
console.log(Validation.validate("invalid-email", emailValidator));    // false

// Modern module approach (recommended)
// In a file: validators.ts
export interface Validator<T> {
  validate(value: T): boolean;
}

export class NumberRangeValidator implements Validator<number> {
  constructor(private min: number, private max: number) {}

  validate(value: number): boolean {
    return value >= this.min && value <= this.max;
  }
}

export class LengthValidator implements Validator<string> {
  constructor(private minLength: number, private maxLength: number) {}

  validate(value: string): boolean {
    return value.length >= this.minLength && value.length <= this.maxLength;
  }
}

// Usage of modules
const rangeValidator = new NumberRangeValidator(1, 100);
console.log(rangeValidator.validate(50));  // true
console.log(rangeValidator.validate(150)); // false

const lengthValidator = new LengthValidator(5, 20);
console.log(lengthValidator.validate("hello"));     // true
console.log(lengthValidator.validate("hi"));        // false
```

**Explanation:** Namespaces organize code into logical groups (older approach). ES6 modules with import/export are the modern standard.

---

## Question 18: Index Signatures and keyof

**Topic:** Index Signatures, keyof operator

**Question:** Create a flexible configuration object with index signatures and type-safe property access.

**Solution:**
```typescript
// Index signature - allows dynamic properties
interface Config {
  [key: string]: string | number | boolean;
  appName: string;  // Known properties can be explicitly defined
  version: string;
}

const config: Config = {
  appName: "MyApp",
  version: "1.0.0",
  debugMode: true,
  maxRetries: 3,
  apiUrl: "https://api.example.com"
};

// keyof operator - gets union of keys
interface Person {
  name: string;
  age: number;
  email: string;
}

type PersonKeys = keyof Person; // "name" | "age" | "email"

// Type-safe property getter
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const person: Person = {
  name: "Alice",
  age: 30,
  email: "alice@example.com"
};

const name = getProperty(person, "name");   // Type: string
const age = getProperty(person, "age");     // Type: number
// const invalid = getProperty(person, "address"); // Error: Argument not assignable

// Record with keyof
type RecordExample = Record<keyof Person, string>;
const personStrings: RecordExample = {
  name: "Alice",
  age: "30",
  email: "alice@example.com"
};

// Mapped type with keyof
type ReadonlyPerson = {
  readonly [K in keyof Person]: Person[K];
};

const immutablePerson: ReadonlyPerson = {
  name: "Bob",
  age: 25,
  email: "bob@example.com"
};

// immutablePerson.name = "Charlie"; // Error: Cannot assign to 'name' because it is a read-only property
```

**Explanation:** Index signatures allow objects to have properties with dynamic keys. The `keyof` operator creates a union type of object keys, enabling type-safe property access.

---

## Question 19: Type Assertion and Type Casting

**Topic:** Type Assertion, as const

**Question:** Demonstrate type assertions and const assertions with practical examples.

**Solution:**
```typescript
// Type assertion using 'as'
const input = document.getElementById("myInput") as HTMLInputElement;
// input.value is now accessible (TypeScript knows it's an input element)

// Angle-bracket syntax (alternative, not usable in .tsx files)
const input2 = <HTMLInputElement>document.getElementById("myInput2");

// Const assertion - makes properties readonly and literal types
const config = {
  endpoint: "https://api.example.com",
  timeout: 5000,
  retries: 3
} as const;

// config.endpoint = "other"; // Error: Cannot assign to 'endpoint' because it is a read-only property
type Config = typeof config;
// Type: { readonly endpoint: "https://api.example.com"; readonly timeout: 5000; readonly retries: 3 }

// Const assertion with arrays
const colors = ["red", "green", "blue"] as const;
type Color = typeof colors[number]; // "red" | "green" | "blue"

function setColor(color: Color): void {
  console.log(`Setting color to ${color}`);
}

setColor("red");   // OK
// setColor("yellow"); // Error: Argument of type '"yellow"' is not assignable

// Non-null assertion operator (!)
interface User {
  name: string;
  email?: string;
}

function sendEmail(user: User): void {
  // We're certain email exists
  const email = user.email!; // Non-null assertion
  console.log(`Sending email to ${email.toLowerCase()}`);
}

// Unknown to specific type
function processValue(value: unknown): string {
  // Type guard first
  if (typeof value === "string") {
    return value.toUpperCase();
  }
  
  // Type assertion when you're certain
  return (value as { toString(): string }).toString();
}

// Double assertion (use sparingly, can be dangerous)
const value = "hello" as unknown as number; // Forces type, bypasses checks

console.log(processValue("hello"));
console.log(processValue({ toString: () => "object" }));
```

**Explanation:** Type assertions tell TypeScript to treat a value as a specific type. Use them when you have more information than TypeScript can infer, but be careful as they bypass type checking.

---

## Question 20: Advanced Types - Template Literal Types

**Topic:** Template Literal Types, String Manipulation Types

**Question:** Create type-safe event handling using template literal types.

**Solution:**
```typescript
// Template literal types
type EventName = "click" | "focus" | "blur";
type ElementId = "button" | "input" | "form";

type ElementEvent = `${ElementId}:${EventName}`;
// Result: "button:click" | "button:focus" | "button:blur" | 
//         "input:click" | "input:focus" | ... (9 combinations)

// Event handler with template literal types
type EventHandler<T extends string> = (event: Event) => void;

class EventEmitter {
  private handlers: Map<ElementEvent, EventHandler<ElementEvent>[]> = new Map();

  on(event: ElementEvent, handler: EventHandler<ElementEvent>): void {
    if (!this.handlers.has(event)) {
      this.handlers.set(event, []);
    }
    this.handlers.get(event)!.push(handler);
  }

  emit(event: ElementEvent, eventData: Event): void {
    const handlers = this.handlers.get(event);
    if (handlers) {
      handlers.forEach(handler => handler(eventData));
    }
  }
}

// Usage
const emitter = new EventEmitter();

emitter.on("button:click", (e) => {
  console.log("Button clicked!");
});

emitter.on("input:focus", (e) => {
  console.log("Input focused!");
});

// emitter.on("invalid:event", () => {}); // Error: not assignable

// String manipulation types
type Greeting = "hello world";
type UpperGreeting = Uppercase<Greeting>;      // "HELLO WORLD"
type LowerGreeting = Lowercase<Greeting>;      // "hello world"
type CapitalGreeting = Capitalize<Greeting>;   // "Hello world"
type UncapitalGreeting = Uncapitalize<Greeting>; // "hello world"

// Practical example: API endpoint types
type HTTPMethod = "GET" | "POST" | "PUT" | "DELETE";
type Endpoint = "/users" | "/posts" | "/comments";
type APIRoute = `${HTTPMethod} ${Endpoint}`;

function makeRequest(route: APIRoute): void {
  console.log(`Making request: ${route}`);
}

makeRequest("GET /users");    // OK
makeRequest("POST /posts");   // OK
// makeRequest("PATCH /users"); // Error: not assignable

// Extract route parts
type ExtractMethod<T extends string> = T extends `${infer M} ${string}` ? M : never;
type Method = ExtractMethod<"GET /users">; // "GET"

type ExtractPath<T extends string> = T extends `${string} ${infer P}` ? P : never;
type Path = ExtractPath<"GET /users">; // "/users"

console.log("Advanced TypeScript patterns demonstrated!");
```

**Explanation:** Template literal types create new string literal types by combining other string literal types. They're powerful for creating type-safe DSLs and APIs. String manipulation types (Uppercase, Lowercase, etc.) transform string literal types.

---

## Summary

These 20 questions cover:

1. **Basic Types** - Primitives and type annotations
2. **Interfaces** - Object shape definitions
3. **Type Aliases & Unions** - Creating reusable type definitions
4. **Arrays & Tuples** - Collection types
5. **Enums** - Named constants
6. **Generics (Basic)** - Reusable type-safe code
7. **Generics (Advanced)** - Constraints and complex generics
8. **Classes** - OOP with access modifiers
9. **Inheritance** - Abstract classes and extending
10. **Type Guards** - Runtime type checking
11. **Utility Types** - Built-in type transformations
12. **Function Overloading** - Multiple function signatures
13. **Mapped Types** - Transforming object types
14. **Conditional Types** - Type-level conditionals
15. **Async/Await** - Asynchronous programming
16. **Decorators** - Metaprogramming
17. **Namespaces & Modules** - Code organization
18. **Index Signatures** - Dynamic object properties
19. **Type Assertions** - Overriding inferred types
20. **Template Literals** - String type manipulation

Practice these questions to master TypeScript's type system and become proficient in type-safe development!