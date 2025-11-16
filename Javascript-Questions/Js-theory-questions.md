# JavaScript Interview Theory Questions

## Table of Contents
1. [Fundamentals](#fundamentals)
2. [Data Types & Variables](#data-types--variables)
3. [Functions](#functions)
4. [Closures & Scope](#closures--scope)
5. [Asynchronous JavaScript](#asynchronous-javascript)
6. [Promises & Async/Await](#promises--asyncawait)
7. [Objects & Prototypes](#objects--prototypes)
8. [ES6+ Features](#es6-features)
9. [Event Loop](#event-loop)
10. [DOM & Browser APIs](#dom--browser-apis)
11. [Advanced Concepts](#advanced-concepts)

---

## Fundamentals

### Q1. What is JavaScript?
JavaScript is a high-level, interpreted programming language that conforms to the ECMAScript specification. It's primarily used for creating interactive web pages and is one of the core technologies of the web alongside HTML and CSS.

### Q2. What are the different data types in JavaScript?
- **Primitive types:** String, Number, BigInt, Boolean, undefined, null, Symbol
- **Non-primitive type:** Object (includes Arrays, Functions, Dates, etc.)

### Q3. What is the difference between `var`, `let`, and `const`?
- **var:** Function-scoped, hoisted, can be redeclared and updated
- **let:** Block-scoped, hoisted but not initialized, can be updated but not redeclared
- **const:** Block-scoped, hoisted but not initialized, cannot be updated or redeclared (though object properties can be modified)

### Q4. What is hoisting?
Hoisting is JavaScript's behavior of moving variable and function declarations to the top of their scope during the compilation phase, before code execution.

### Q5. Explain `==` vs `===`
- **==** (abstract equality): Compares values after type coercion
- **===** (strict equality): Compares both value and type without coercion

---

## Data Types & Variables

### Q6. What is the difference between `null` and `undefined`?
- **undefined:** A variable has been declared but not assigned a value
- **null:** An intentional absence of any object value, must be assigned explicitly

### Q7. What is type coercion?
Type coercion is the automatic or implicit conversion of values from one data type to another. JavaScript is a loosely typed language and performs coercion in certain operations.

### Q8. What is NaN? How to check if a value is NaN?
NaN stands for "Not-a-Number" and is returned when a mathematical operation fails. Use `Number.isNaN()` to check (not `isNaN()` which coerces values first).

### Q9. What is the difference between primitive and reference types?
- **Primitives:** Stored by value, immutable, compared by value
- **References:** Stored by reference, mutable, compared by reference (memory address)

### Q10. Explain truthy and falsy values
**Falsy values:** `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, `NaN`
Everything else is truthy.

---

## Functions

### Q11. What are first-class functions?
In JavaScript, functions are first-class citizens, meaning they can be assigned to variables, passed as arguments, returned from other functions, and stored in data structures.

### Q12. What is the difference between function declaration and function expression?
```javascript
// Function Declaration - hoisted
function declared() {}

// Function Expression - not hoisted
const expressed = function() {};
```

### Q13. What are arrow functions? How are they different from regular functions?
Arrow functions have shorter syntax and don't have their own `this`, `arguments`, `super`, or `new.target`. They cannot be used as constructors.

### Q14. What are higher-order functions?
Functions that take other functions as arguments or return functions as their result.

### Q15. What is the `arguments` object?
An array-like object accessible inside functions containing the values of arguments passed to that function. Not available in arrow functions.

---

## Closures & Scope

### Q16. What is a closure?
A closure is a function that has access to variables in its outer (enclosing) lexical scope, even after the outer function has returned.

### Q17. What are the different types of scope in JavaScript?
- **Global scope:** Variables accessible everywhere
- **Function scope:** Variables accessible within function
- **Block scope:** Variables accessible within block (let/const)
- **Lexical scope:** Inner functions have access to outer function variables

### Q18. What is lexical scoping?
Lexical scoping means that the scope of a variable is determined by its position in the source code, and nested functions have access to variables declared in their outer scope.

### Q19. Explain the scope chain
When JavaScript looks for a variable, it first searches in the local scope, then moves up to the outer scope, continuing until it reaches global scope or finds the variable.

### Q20. What are IIFEs (Immediately Invoked Function Expressions)?
Functions that are executed immediately after they're created, often used to create a private scope:
```javascript
(function() {
  // code
})();
```

---

## Asynchronous JavaScript

### Q21. What is the difference between synchronous and asynchronous code?
- **Synchronous:** Code executes line by line, blocking subsequent operations
- **Asynchronous:** Code executes without blocking, allowing other operations to continue

### Q22. What is a callback function?
A function passed as an argument to another function, to be executed later (often after an async operation completes).

### Q23. What is callback hell?
Deeply nested callbacks that make code hard to read and maintain, often called "pyramid of doom."

### Q24. What are Web APIs?
Browser-provided APIs like setTimeout, fetch, DOM APIs that enable async operations and browser functionality.

### Q25. Explain `setTimeout` and `setInterval`
- **setTimeout:** Executes code once after a specified delay
- **setInterval:** Repeatedly executes code at specified intervals

---

## Promises & Async/Await

### Q26. What is a Promise?
An object representing the eventual completion or failure of an asynchronous operation. It has three states: pending, fulfilled, rejected.

### Q27. What are the methods available on Promises?
- `.then()` - handles success
- `.catch()` - handles errors
- `.finally()` - executes regardless of outcome
- `Promise.all()`, `Promise.race()`, `Promise.allSettled()`, `Promise.any()`

### Q28. What is async/await?
Syntactic sugar over Promises that makes asynchronous code look and behave more like synchronous code. `async` functions always return a Promise.

### Q29. What is the difference between Promise.all() and Promise.race()?
- **Promise.all():** Waits for all promises to resolve (or one to reject)
- **Promise.race():** Resolves/rejects as soon as the first promise settles

### Q30. How do you handle errors in async/await?
Use try-catch blocks around await statements to handle rejected promises.

---

## Objects & Prototypes

### Q31. How do you create objects in JavaScript?
- Object literal: `{}`
- Constructor function: `new Object()`
- `Object.create()`
- ES6 classes
- Factory functions

### Q32. What is prototypal inheritance?
JavaScript objects inherit properties and methods from a prototype object. Every object has an internal `[[Prototype]]` property.

### Q33. What is the prototype chain?
When accessing a property, JavaScript searches the object, then its prototype, then the prototype's prototype, up to `Object.prototype`.

### Q34. What is the difference between `__proto__` and `prototype`?
- **`__proto__`:** Property of object instances pointing to the prototype
- **`prototype`:** Property of constructor functions used when creating new instances

### Q35. What are getters and setters?
Special methods that allow you to define how a property is accessed or modified on an object.

---

## ES6+ Features

### Q36. What is destructuring?
A way to extract values from arrays or properties from objects into distinct variables:
```javascript
const [a, b] = [1, 2];
const {name, age} = person;
```

### Q37. What are template literals?
String literals allowing embedded expressions and multi-line strings using backticks.

### Q38. What is the spread operator?
The `...` operator that expands iterables into individual elements, or copies object/array properties.

### Q39. What is the rest parameter?
Uses `...` syntax to represent an indefinite number of arguments as an array in function parameters.

### Q40. What are default parameters?
Function parameters that have default values if no value or undefined is passed.

### Q41. What are ES6 Modules?
A way to organize and share code using `export` and `import` statements, providing better code organization and reusability.

### Q42. What is the difference between Map and Object?
- **Map:** Keys can be any type, maintains insertion order, has size property, better performance for frequent additions/removals
- **Object:** Keys are strings/symbols, may have inherited properties, no built-in size

### Q43. What are Sets?
Collections of unique values of any type, automatically removing duplicates.

### Q44. What are Symbols?
A unique and immutable primitive data type, often used as object property keys to avoid collisions.

### Q45. What are generators?
Functions that can be paused and resumed, defined with `function*` and use `yield` to produce values.

---

## Event Loop

### Q46. What is the Event Loop?
A mechanism that handles asynchronous operations by managing the call stack, callback queue, and microtask queue.

### Q47. What is the Call Stack?
A LIFO data structure that tracks function execution. Functions are pushed when called and popped when they return.

### Q48. What are microtasks and macrotasks?
- **Microtasks:** Promise callbacks, process.nextTick (higher priority)
- **Macrotasks:** setTimeout, setInterval, I/O operations

### Q49. Explain the order of execution in the event loop
1. Execute synchronous code
2. Execute all microtasks
3. Execute one macrotask
4. Execute all microtasks created during macrotask
5. Repeat from step 3

### Q50. What is the difference between call stack and task queue?
Call stack executes functions synchronously; task queue holds callbacks waiting to be executed when the stack is empty.

---

## DOM & Browser APIs

### Q51. What is the DOM?
Document Object Model - a programming interface representing HTML/XML documents as a tree structure that can be manipulated with JavaScript.

### Q52. What is event bubbling?
When an event fires on an element, it first runs handlers on that element, then on its parent, then all the way up to ancestors.

### Q53. What is event capturing?
The opposite of bubbling - the event travels down from the root to the target element.

### Q54. What is event delegation?
Attaching a single event listener to a parent element instead of multiple listeners to child elements, using event bubbling.

### Q55. What is the difference between `preventDefault()` and `stopPropagation()`?
- **preventDefault():** Stops the default browser action
- **stopPropagation():** Stops the event from bubbling/capturing further

### Q56. What is localStorage and sessionStorage?
Web Storage APIs for storing key-value pairs:
- **localStorage:** Persists until explicitly cleared
- **sessionStorage:** Cleared when page session ends

### Q57. What are cookies?
Small pieces of data stored by the browser, sent with every HTTP request to the domain.

---

## Advanced Concepts

### Q58. What is currying?
Transforming a function with multiple arguments into a sequence of functions, each taking a single argument.

### Q59. What is memoization?
An optimization technique that caches function results based on inputs to avoid redundant calculations.

### Q60. What is debouncing?
Limiting function execution so it only fires after a certain amount of time has passed since it was last called.

### Q61. What is throttling?
Limiting function execution to once per specified time period, regardless of how many times it's called.

### Q62. What is the `this` keyword?
A reference to the object that is currently executing the code. Its value depends on how and where a function is called.

### Q63. What are the ways to change the value of `this`?
- `.call()` - invokes function with specified this and arguments
- `.apply()` - like call but takes arguments as array
- `.bind()` - creates new function with bound this value

### Q64. What is method chaining?
Calling multiple methods on the same object in sequence by returning `this` from each method.

### Q65. What is the difference between deep copy and shallow copy?
- **Shallow copy:** Copies only the first level (nested objects are referenced)
- **Deep copy:** Recursively copies all levels (creates independent copies)

### Q66. What is the Temporal Dead Zone?
The period between entering scope and variable declaration where let/const variables cannot be accessed.

### Q67. What are WeakMap and WeakSet?
Collections with weak references to keys/values that don't prevent garbage collection.

### Q68. What is the difference between `for...in` and `for...of`?
- **for...in:** Iterates over enumerable property names (keys)
- **for...of:** Iterates over iterable object values

### Q69. What is strict mode?
A restricted variant of JavaScript activated with `"use strict"` that catches common coding errors and prevents unsafe actions.

### Q70. What are Proxy and Reflect?
- **Proxy:** Creates a wrapper for objects to intercept and customize operations
- **Reflect:** Built-in object providing methods for interceptable JavaScript operations

---

## Bonus Questions

### Q71. What is tree shaking?
Dead code elimination technique in bundlers that removes unused code from the final bundle.

### Q72. What is polyfill?
Code that provides modern functionality on older browsers that don't natively support it.

### Q73. What is transpiling?
Converting modern JavaScript code to older versions for browser compatibility (e.g., Babel).

### Q74. What is the difference between `Object.freeze()` and `Object.seal()`?
- **freeze():** Makes object immutable (can't add, delete, or modify)
- **seal():** Prevents adding/deleting properties but allows modification

### Q75. What is optional chaining?
The `?.` operator that safely accesses nested properties without throwing errors if a reference is null/undefined.

---

## Tips for Interview Success

1. **Understand, don't memorize** - Focus on concepts, not just definitions
2. **Practice coding** - Theory is important but be ready to code solutions
3. **Use examples** - Illustrate your answers with code snippets
4. **Ask clarifying questions** - Shows thoughtfulness and communication skills
5. **Admit when you don't know** - Better to be honest than to guess
6. **Stay updated** - JavaScript evolves; know recent features
7. **Explain your thinking** - Walk through your thought process

---

**Good luck with your interview preparation! 🚀**