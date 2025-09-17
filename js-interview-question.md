# JavaScript Interview Questions - Complete Guide

## Table of Contents
1. [Basic JavaScript Concepts](#basic-javascript-concepts)
2. [Variables and Data Types](#variables-and-data-types)
3. [Functions](#functions)
4. [Objects and Arrays](#objects-and-arrays)
5. [Scope and Closures](#scope-and-closures)
6. [Hoisting](#hoisting)
7. [This Keyword](#this-keyword)
8. [Prototypes and Inheritance](#prototypes-and-inheritance)
9. [Asynchronous JavaScript](#asynchronous-javascript)
10. [Event Handling](#event-handling)
11. [ES6+ Features](#es6-features)
12. [Error Handling](#error-handling)
13. [Performance and Optimization](#performance-and-optimization)
14. [Design Patterns](#design-patterns)
15. [Practical Coding Questions](#practical-coding-questions)

---

## Basic JavaScript Concepts

### 1. What is JavaScript? ⭐⭐⭐
**Answer:** JavaScript is a high-level, interpreted programming language that is dynamic, weakly typed, and prototype-based. It's primarily used for web development to create interactive web pages, but also for server-side development (Node.js), mobile apps, and desktop applications.

### 2. What are the differences between `==` and `===`? ⭐⭐⭐
**Answer:**
- `==` (loose equality): Performs type coercion before comparison
- `===` (strict equality): Compares both value and type without coercion

```javascript
console.log(5 == '5');  // true (type coercion)
console.log(5 === '5'); // false (different types)
```

### 3. What is the difference between `null` and `undefined`? ⭐⭐⭐
**Answer:**
- `undefined`: Variable declared but not assigned a value
- `null`: Intentional absence of value, must be explicitly assigned

```javascript
let a;
console.log(a); // undefined

let b = null;
console.log(b); // null
```

---

## Variables and Data Types

### 4. What are the primitive data types in JavaScript? ⭐⭐⭐
**Answer:** 
- Number
- String
- Boolean
- Undefined
- Null
- Symbol (ES6)
- BigInt (ES2020)

### 5. Explain `var`, `let`, and `const`. ⭐⭐⭐
**Answer:**
- `var`: Function-scoped, hoisted, can be redeclared
- `let`: Block-scoped, hoisted but not initialized, cannot be redeclared
- `const`: Block-scoped, must be initialized, cannot be reassigned

```javascript
var a = 1;
let b = 2;
const c = 3;

// var can be redeclared
var a = 4; // OK

// let cannot be redeclared
// let b = 5; // SyntaxError

// const cannot be reassigned
// c = 6; // TypeError
```

### 6. What is temporal dead zone? ⭐⭐
**Answer:** The temporal dead zone is the time between entering scope and being declared where variables cannot be accessed.

```javascript
console.log(x); // ReferenceError
let x = 5;
```

---

## Functions

### 7. What are the different ways to create functions in JavaScript? ⭐⭐⭐
**Answer:**
```javascript
// Function Declaration
function myFunc() {}

// Function Expression
const myFunc = function() {};

// Arrow Function
const myFunc = () => {};

// Constructor Function
const myFunc = new Function('return 1');
```

### 8. What is the difference between function declaration and function expression? ⭐⭐⭐
**Answer:**
- Function declarations are hoisted completely
- Function expressions are not hoisted

```javascript
// Works due to hoisting
myFunc(); 

function myFunc() {
    console.log("Hello");
}

// Doesn't work - TypeError
myFunc2(); 

var myFunc2 = function() {
    console.log("Hello");
};
```

### 9. What are arrow functions and their differences from regular functions? ⭐⭐⭐
**Answer:** Arrow functions are a shorter syntax for function expressions with lexical `this` binding.

**Differences:**
- No `this` binding (inherits from enclosing scope)
- Cannot be used as constructors
- No `arguments` object
- Cannot be hoisted

```javascript
const regular = function() {
    console.log(this); // depends on how it's called
};

const arrow = () => {
    console.log(this); // inherits from surrounding scope
};
```

### 10. What are higher-order functions? ⭐⭐
**Answer:** Functions that take other functions as arguments or return functions.

```javascript
// Takes function as argument
function higherOrder(fn) {
    return fn();
}

// Returns function
function createMultiplier(x) {
    return function(y) {
        return x * y;
    };
}
```

---

## Objects and Arrays

### 11. How do you check if a property exists in an object? ⭐⭐
**Answer:**
```javascript
const obj = { name: 'John', age: 30 };

// Method 1: in operator
console.log('name' in obj); // true

// Method 2: hasOwnProperty
console.log(obj.hasOwnProperty('name')); // true

// Method 3: undefined check
console.log(obj.name !== undefined); // true
```

### 12. What are the different ways to iterate over an array? ⭐⭐⭐
**Answer:**
```javascript
const arr = [1, 2, 3, 4, 5];

// for loop
for (let i = 0; i < arr.length; i++) {}

// forEach
arr.forEach(item => {});

// for...of
for (const item of arr) {}

// map (returns new array)
arr.map(item => item * 2);

// for...in (not recommended for arrays)
for (const index in arr) {}
```

### 13. What is array destructuring? ⭐⭐
**Answer:** A way to extract values from arrays into distinct variables.

```javascript
const arr = [1, 2, 3];
const [a, b, c] = arr;

// With rest operator
const [first, ...rest] = arr;

// With default values
const [x, y, z = 0] = [1, 2];
```

---

## Scope and Closures

### 14. What is closure? ⭐⭐⭐
**Answer:** A closure is created when an inner function has access to variables from its outer (enclosing) scope even after the outer function returns.

```javascript
function outerFunction(x) {
    return function innerFunction(y) {
        return x + y; // Access to x from outer scope
    };
}

const addFive = outerFunction(5);
console.log(addFive(3)); // 8
```

### 15. What is lexical scoping? ⭐⭐
**Answer:** Lexical scoping means that the accessibility of variables is determined by where they are declared in the code.

```javascript
function outer() {
    let x = 10;
    
    function inner() {
        console.log(x); // Can access x from outer scope
    }
    
    inner();
}
```

### 16. What is the scope chain? ⭐⭐
**Answer:** The scope chain is the hierarchy of scopes that JavaScript uses to resolve variable names.

```javascript
let global = 'global';

function outer() {
    let outerVar = 'outer';
    
    function inner() {
        let innerVar = 'inner';
        console.log(innerVar);  // inner scope
        console.log(outerVar);  // outer scope
        console.log(global);    // global scope
    }
    
    inner();
}
```

---

## Hoisting

### 17. What is hoisting? ⭐⭐⭐
**Answer:** Hoisting is JavaScript's behavior of moving variable and function declarations to the top of their containing scope during compilation.

```javascript
console.log(x); // undefined (not ReferenceError)
var x = 5;

// Equivalent to:
var x;
console.log(x);
x = 5;
```

### 18. How does hoisting work with `let`, `const`, and `var`? ⭐⭐⭐
**Answer:**
```javascript
// var - hoisted and initialized with undefined
console.log(a); // undefined
var a = 1;

// let - hoisted but not initialized (TDZ)
console.log(b); // ReferenceError
let b = 2;

// const - hoisted but not initialized (TDZ)
console.log(c); // ReferenceError
const c = 3;
```

---

## This Keyword

### 19. What is the `this` keyword? ⭐⭐⭐
**Answer:** `this` refers to the object that the function is called on. Its value depends on how the function is invoked.

```javascript
// Global context
console.log(this); // window (browser) or global (Node.js)

// Object method
const obj = {
    name: 'John',
    getName() {
        return this.name; // refers to obj
    }
};

// Function call
function myFunc() {
    return this; // window in non-strict mode, undefined in strict mode
}
```

### 20. How do you change the context of `this`? ⭐⭐⭐
**Answer:** Using `call()`, `apply()`, and `bind()` methods.

```javascript
const person = { name: 'John' };

function greet(greeting) {
    return `${greeting}, ${this.name}`;
}

// call - passes arguments individually
greet.call(person, 'Hello'); // "Hello, John"

// apply - passes arguments as array
greet.apply(person, ['Hi']); // "Hi, John"

// bind - returns new function with bound context
const boundGreet = greet.bind(person);
boundGreet('Hey'); // "Hey, John"
```

---

## Prototypes and Inheritance

### 21. What is prototype in JavaScript? ⭐⭐⭐
**Answer:** Every JavaScript object has a prototype property that allows you to add properties and methods to object constructors.

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype.sayHello = function() {
    return `Hello, I'm ${this.name}`;
};

const john = new Person('John');
console.log(john.sayHello()); // "Hello, I'm John"
```

### 22. What is prototypal inheritance? ⭐⭐⭐
**Answer:** JavaScript uses prototypal inheritance where objects can inherit directly from other objects.

```javascript
const animal = {
    speak() {
        console.log('Animal speaks');
    }
};

const dog = Object.create(animal);
dog.bark = function() {
    console.log('Woof!');
};

dog.speak(); // "Animal speaks" (inherited)
dog.bark();  // "Woof!" (own method)
```

### 23. What is the difference between `__proto__` and `prototype`? ⭐⭐
**Answer:**
- `prototype`: Property of constructor functions
- `__proto__`: Property of instances, points to constructor's prototype

```javascript
function Person() {}

const john = new Person();

console.log(Person.prototype); // Constructor's prototype
console.log(john.__proto__);   // Instance's proto (same as Person.prototype)
```

---

## Asynchronous JavaScript

### 24. What is the event loop? ⭐⭐⭐
**Answer:** The event loop is responsible for executing code, collecting and processing events, and executing queued sub-tasks.

**Components:**
- Call Stack
- Web APIs
- Callback Queue
- Microtask Queue

### 25. What are Promises? ⭐⭐⭐
**Answer:** Promises represent the eventual completion or failure of an asynchronous operation.

```javascript
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('Success!');
    }, 1000);
});

promise
    .then(result => console.log(result))
    .catch(error => console.log(error));
```

### 26. What is async/await? ⭐⭐⭐
**Answer:** Async/await is syntactic sugar for promises that makes asynchronous code look synchronous.

```javascript
async function fetchData() {
    try {
        const response = await fetch('/api/data');
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error:', error);
    }
}
```

### 27. What is the difference between `setTimeout`, `setImmediate`, and `process.nextTick`? ⭐⭐
**Answer:**
- `setTimeout`: Executes after minimum delay
- `setImmediate`: Executes after I/O events (Node.js)
- `process.nextTick`: Executes before other asynchronous operations (Node.js)

```javascript
console.log('Start');

setTimeout(() => console.log('Timeout'), 0);
setImmediate(() => console.log('Immediate'));
process.nextTick(() => console.log('Next Tick'));

console.log('End');

// Output: Start, End, Next Tick, Immediate, Timeout
```

---

## Event Handling

### 28. What is event delegation? ⭐⭐
**Answer:** Event delegation uses event bubbling to handle events at a higher level in the DOM tree.

```javascript
// Instead of adding listeners to each button
document.getElementById('container').addEventListener('click', function(e) {
    if (e.target.matches('button')) {
        console.log('Button clicked:', e.target.textContent);
    }
});
```

### 29. What is the difference between `stopPropagation()` and `preventDefault()`? ⭐⭐
**Answer:**
- `stopPropagation()`: Stops the event from bubbling up
- `preventDefault()`: Prevents the default browser behavior

```javascript
element.addEventListener('click', function(e) {
    e.stopPropagation(); // Stops bubbling
    e.preventDefault();  // Prevents default action
});
```

---

## ES6+ Features

### 30. What are template literals? ⭐⭐
**Answer:** Template literals allow embedded expressions and multi-line strings.

```javascript
const name = 'John';
const age = 30;

const message = `Hello, my name is ${name} and I'm ${age} years old.`;

const multiline = `
    Line 1
    Line 2
    Line 3
`;
```

### 31. What are default parameters? ⭐⭐
**Answer:** Default parameters allow you to set default values for function parameters.

```javascript
function greet(name = 'Anonymous', greeting = 'Hello') {
    return `${greeting}, ${name}!`;
}

console.log(greet()); // "Hello, Anonymous!"
console.log(greet('John')); // "Hello, John!"
```

### 32. What is the spread operator? ⭐⭐⭐
**Answer:** The spread operator (...) allows an iterable to be expanded.

```javascript
// Arrays
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]

// Objects
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // { a: 1, b: 2, c: 3 }

// Function arguments
function sum(a, b, c) {
    return a + b + c;
}
console.log(sum(...arr1)); // 6
```

### 33. What is destructuring? ⭐⭐⭐
**Answer:** Destructuring allows unpacking values from arrays or properties from objects.

```javascript
// Array destructuring
const [a, b, c] = [1, 2, 3];

// Object destructuring
const { name, age } = { name: 'John', age: 30 };

// Function parameter destructuring
function greet({ name, age }) {
    return `Hello ${name}, you are ${age} years old`;
}
```

### 34. What are classes in ES6? ⭐⭐
**Answer:** ES6 classes are syntactic sugar over JavaScript's prototype-based inheritance.

```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    greet() {
        return `Hello, I'm ${this.name}`;
    }
    
    static species() {
        return 'Homo sapiens';
    }
}

class Student extends Person {
    constructor(name, age, grade) {
        super(name, age);
        this.grade = grade;
    }
    
    study() {
        return `${this.name} is studying`;
    }
}
```

---

## Error Handling

### 35. How do you handle errors in JavaScript? ⭐⭐
**Answer:** Using try-catch blocks and proper error handling patterns.

```javascript
// Synchronous error handling
try {
    // Code that might throw an error
    JSON.parse('invalid json');
} catch (error) {
    console.error('Error:', error.message);
} finally {
    console.log('This always runs');
}

// Asynchronous error handling
async function fetchData() {
    try {
        const response = await fetch('/api/data');
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return await response.json();
    } catch (error) {
        console.error('Fetch error:', error);
    }
}
```

### 36. What are custom errors? ⭐⭐
**Answer:** Custom errors extend the Error class to create specific error types.

```javascript
class CustomError extends Error {
    constructor(message, code) {
        super(message);
        this.name = 'CustomError';
        this.code = code;
    }
}

throw new CustomError('Something went wrong', 500);
```

---

## Performance and Optimization

### 37. What is memoization? ⭐⭐
**Answer:** Memoization is an optimization technique that stores results of expensive function calls.

```javascript
function memoize(fn) {
    const cache = {};
    return function(...args) {
        const key = JSON.stringify(args);
        if (key in cache) {
            return cache[key];
        }
        cache[key] = fn.apply(this, args);
        return cache[key];
    };
}

const fibonacci = memoize(function(n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
});
```

### 38. What is debouncing and throttling? ⭐⭐⭐
**Answer:**
- **Debouncing**: Delays execution until after a pause in calls
- **Throttling**: Limits execution to once per time interval

```javascript
// Debouncing
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
}

// Throttling
function throttle(func, limit) {
    let inThrottle;
    return function(...args) {
        if (!inThrottle) {
            func.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}
```

---

## Design Patterns

### 39. What is the Module pattern? ⭐⭐
**Answer:** The Module pattern encapsulates code to create private and public methods.

```javascript
const Module = (function() {
    let privateVariable = 'I am private';
    
    function privateFunction() {
        console.log('Private function');
    }
    
    return {
        publicMethod: function() {
            return privateVariable;
        },
        anotherPublicMethod: function() {
            privateFunction();
        }
    };
})();
```

### 40. What is the Observer pattern? ⭐⭐
**Answer:** The Observer pattern defines a one-to-many dependency between objects.

```javascript
class Observable {
    constructor() {
        this.observers = [];
    }
    
    subscribe(observer) {
        this.observers.push(observer);
    }
    
    unsubscribe(observer) {
        this.observers = this.observers.filter(obs => obs !== observer);
    }
    
    notify(data) {
        this.observers.forEach(observer => observer.update(data));
    }
}

class Observer {
    update(data) {
        console.log('Received:', data);
    }
}
```

---

## Practical Coding Questions

### 41. How do you deep clone an object? ⭐⭐⭐
**Answer:**
```javascript
// Method 1: JSON (limited - no functions, dates, etc.)
const deepClone1 = (obj) => JSON.parse(JSON.stringify(obj));

// Method 2: Recursive approach
function deepClone(obj) {
    if (obj === null || typeof obj !== 'object') return obj;
    if (obj instanceof Date) return new Date(obj.getTime());
    if (obj instanceof Array) return obj.map(item => deepClone(item));
    if (obj instanceof Object) {
        const copy = {};
        Object.keys(obj).forEach(key => {
            copy[key] = deepClone(obj[key]);
        });
        return copy;
    }
}
```

### 42. How do you flatten an array? ⭐⭐
**Answer:**
```javascript
// Method 1: ES2019 flat()
const arr = [1, [2, 3], [4, [5, 6]]];
console.log(arr.flat()); // [1, 2, 3, 4, [5, 6]]
console.log(arr.flat(2)); // [1, 2, 3, 4, 5, 6]

// Method 2: Recursive
function flatten(arr) {
    return arr.reduce((acc, val) => 
        Array.isArray(val) ? acc.concat(flatten(val)) : acc.concat(val), []);
}

// Method 3: Stack-based
function flattenStack(arr) {
    const stack = [...arr];
    const result = [];
    
    while (stack.length > 0) {
        const next = stack.pop();
        if (Array.isArray(next)) {
            stack.push(...next);
        } else {
            result.push(next);
        }
    }
    
    return result.reverse();
}
```

### 43. How do you remove duplicates from an array? ⭐⭐⭐
**Answer:**
```javascript
const arr = [1, 2, 2, 3, 4, 4, 5];

// Method 1: Set
const unique1 = [...new Set(arr)];

// Method 2: Filter with indexOf
const unique2 = arr.filter((item, index) => arr.indexOf(item) === index);

// Method 3: Reduce
const unique3 = arr.reduce((acc, current) => {
    if (!acc.includes(current)) {
        acc.push(current);
    }
    return acc;
}, []);
```

### 44. How do you reverse a string? ⭐⭐
**Answer:**
```javascript
const str = "hello";

// Method 1: Built-in methods
const reversed1 = str.split('').reverse().join('');

// Method 2: For loop
function reverseString(str) {
    let reversed = '';
    for (let i = str.length - 1; i >= 0; i--) {
        reversed += str[i];
    }
    return reversed;
}

// Method 3: Recursion
function reverseRecursive(str) {
    if (str === '') return '';
    return reverseRecursive(str.substr(1)) + str.charAt(0);
}
```

### 45. How do you check if a string is a palindrome? ⭐⭐
**Answer:**
```javascript
function isPalindrome(str) {
    // Remove non-alphanumeric characters and convert to lowercase
    const cleaned = str.replace(/[^a-zA-Z0-9]/g, '').toLowerCase();
    
    // Compare with reversed version
    return cleaned === cleaned.split('').reverse().join('');
}

// Alternative approach
function isPalindromeOptimized(str) {
    const cleaned = str.replace(/[^a-zA-Z0-9]/g, '').toLowerCase();
    let left = 0;
    let right = cleaned.length - 1;
    
    while (left < right) {
        if (cleaned[left] !== cleaned[right]) {
            return false;
        }
        left++;
        right--;
    }
    
    return true;
}
```

---

## ⭐ Most Important Questions Summary

1. **What is closure?** - Understanding closures is crucial for JavaScript mastery
2. **Explain `var`, `let`, and `const`** - Fundamental for variable handling
3. **What are Promises?** - Essential for asynchronous programming
4. **What is the `this` keyword?** - Critical for understanding object methods
5. **What is hoisting?** - Important for understanding JavaScript execution
6. **What is prototypal inheritance?** - Core JavaScript concept
7. **What is the event loop?** - Essential for asynchronous understanding
8. **What are the differences between `==` and `===`?** - Basic but important
9. **What is async/await?** - Modern asynchronous programming
10. **How do you handle errors in JavaScript?** - Critical for robust applications

---

## Interview Tips

1. **Practice coding questions** - Be ready to code solutions on a whiteboard or computer
2. **Understand the concepts** - Don't just memorize answers, understand the underlying principles
3. **Ask clarifying questions** - Show that you think about edge cases and requirements
4. **Explain your thought process** - Talk through your approach and reasoning
5. **Know the trade-offs** - Understand when to use different approaches and why
6. **Stay updated** - Keep learning about new JavaScript features and best practices

Good luck with your JavaScript interviews!