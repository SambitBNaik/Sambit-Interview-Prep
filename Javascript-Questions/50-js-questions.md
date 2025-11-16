# JavaScript Concepts & Interview Guide

## Core Concepts

### 1. Scope in JavaScript

**Global Scope**: Variables declared outside any function or block are in the global scope and accessible everywhere.

```javascript
var globalVar = "I'm global";
function test() {
  console.log(globalVar); // Accessible
}
```

**Function Scope**: Variables declared inside a function are only accessible within that function.

```javascript
function myFunc() {
  var funcVar = "I'm local";
  console.log(funcVar); // Works
}
console.log(funcVar); // Error: funcVar is not defined
```

**Block Scope**: Variables declared with `let` and `const` inside `{}` are block-scoped.

```javascript
if (true) {
  let blockVar = "I'm block scoped";
  console.log(blockVar); // Works
}
console.log(blockVar); // Error
```

### 2. Scope Chaining

When a variable is used, JavaScript looks for it in the current scope, then parent scope, and continues up the chain until it reaches the global scope.

```javascript
const global = "global";
function outer() {
  const outerVar = "outer";
  function inner() {
    const innerVar = "inner";
    console.log(innerVar);  // Found in local scope
    console.log(outerVar);  // Found in parent scope
    console.log(global);    // Found in global scope
  }
  inner();
}
```

### 3. Primitive vs Non-Primitive

**Primitive Types** (immutable, stored by value):
- String, Number, Boolean, Undefined, Null, Symbol, BigInt

**Non-Primitive Types** (mutable, stored by reference):
- Object, Array, Function

```javascript
// Primitive
let a = 10;
let b = a;
b = 20;
console.log(a); // 10 (unchanged)

// Non-Primitive
let obj1 = { value: 10 };
let obj2 = obj1;
obj2.value = 20;
console.log(obj1.value); // 20 (changed)
```

### 4. Var, Let, and Const

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Hoisting | Yes (initialized as undefined) | Yes (TDZ) | Yes (TDZ) |
| Re-declaration | Yes | No | No |
| Re-assignment | Yes | Yes | No |

```javascript
var x = 1;
var x = 2; // OK

let y = 1;
// let y = 2; // Error

const z = 1;
// z = 2; // Error
```

### 5. Temporal Dead Zone (TDZ)

The time between entering scope and variable declaration where accessing the variable throws an error.

```javascript
console.log(x); // ReferenceError: Cannot access 'x' before initialization
let x = 5;

// TDZ exists for let and const, not var
console.log(y); // undefined
var y = 10;
```

### 6. Hoisting

JavaScript moves declarations to the top of their scope during compilation.

```javascript
// Function hoisting
greet(); // Works!
function greet() {
  console.log("Hello");
}

// Variable hoisting with var
console.log(x); // undefined
var x = 5;

// let/const are hoisted but not initialized (TDZ)
console.log(y); // ReferenceError
let y = 10;
```

### 7. Prototypes in JavaScript

Every JavaScript object has an internal link to another object called its prototype. The prototype is used to inherit properties and methods.

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  console.log(`Hello, I'm ${this.name}`);
};

const john = new Person("John");
john.greet(); // "Hello, I'm John"
```

### 8. Prototype Object

The prototype object is a template object from which other objects inherit properties and methods.

```javascript
const obj = {};
console.log(obj.__proto__ === Object.prototype); // true

const arr = [];
console.log(arr.__proto__ === Array.prototype); // true
console.log(arr.__proto__.__proto__ === Object.prototype); // true
```

### 9. Prototype Chaining

When accessing a property, JavaScript first looks at the object itself, then its prototype, then the prototype's prototype, until it reaches `null`.

```javascript
function Animal(name) {
  this.name = name;
}
Animal.prototype.eat = function() {
  console.log(`${this.name} is eating`);
};

function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.bark = function() {
  console.log("Woof!");
};

const myDog = new Dog("Buddy", "Golden Retriever");
myDog.eat();  // Inherited from Animal
myDog.bark(); // Own method
```

### 10. Closures

A closure is a function that has access to variables in its outer (enclosing) scope, even after the outer function has returned.

```javascript
function outer() {
  let count = 0;
  return function inner() {
    count++;
    console.log(count);
  };
}

const counter = outer();
counter(); // 1
counter(); // 2
counter(); // 3
```

**Use Cases**: Data privacy, factory functions, callbacks

### 11. Pass by Reference vs Pass by Value

**Primitives**: Passed by value (copy)
**Objects**: Passed by reference (memory address)

```javascript
// Pass by value
let num = 10;
function changeNum(x) {
  x = 20;
}
changeNum(num);
console.log(num); // 10

// Pass by reference
let obj = { value: 10 };
function changeObj(o) {
  o.value = 20;
}
changeObj(obj);
console.log(obj.value); // 20
```

### 12. Currying in JavaScript

Transforming a function with multiple arguments into a sequence of functions each taking a single argument.

```javascript
// Regular function
function add(a, b, c) {
  return a + b + c;
}

// Curried function
function curriedAdd(a) {
  return function(b) {
    return function(c) {
      return a + b + c;
    };
  };
}

console.log(curriedAdd(1)(2)(3)); // 6

// Modern syntax
const curry = a => b => c => a + b + c;
```

### 13. Infinite Currying Problem

Create a function that can be called infinite times and sum all arguments.

```javascript
function infiniteSum(a) {
  return function(b) {
    if (b) {
      return infiniteSum(a + b);
    }
    return a;
  };
}

console.log(infiniteSum(1)(2)(3)(4)()); // 10

// Alternative with valueOf
function sum(a) {
  const func = function(b) {
    return sum(a + b);
  };
  func.valueOf = function() {
    return a;
  };
  return func;
}
console.log(+sum(1)(2)(3)); // 6
```

### 14. Memoization in JavaScript

Caching function results to avoid redundant calculations.

```javascript
function memoize(fn) {
  const cache = {};
  return function(...args) {
    const key = JSON.stringify(args);
    if (key in cache) {
      console.log('From cache');
      return cache[key];
    }
    console.log('Computing...');
    const result = fn(...args);
    cache[key] = result;
    return result;
  };
}

const factorial = memoize(function(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
});

console.log(factorial(5)); // Computing... 120
console.log(factorial(5)); // From cache 120
```

### 15. Rest Parameter

Collects all remaining arguments into an array.

```javascript
function sum(...numbers) {
  return numbers.reduce((acc, num) => acc + num, 0);
}

console.log(sum(1, 2, 3, 4)); // 10

function myFunc(first, second, ...rest) {
  console.log(first);  // 1
  console.log(second); // 2
  console.log(rest);   // [3, 4, 5]
}
myFunc(1, 2, 3, 4, 5);
```

### 16. Spread Operator

Expands an iterable into individual elements.

```javascript
// Array spreading
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]

// Object spreading
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // { a: 1, b: 2, c: 3 }

// Function arguments
const numbers = [1, 2, 3];
Math.max(...numbers); // 3
```

### 17. Ways to Create Objects in JavaScript

```javascript
// 1. Object Literal
const obj1 = { name: "John" };

// 2. Constructor Function
function Person(name) {
  this.name = name;
}
const obj2 = new Person("Jane");

// 3. Object.create()
const proto = { greet() { console.log("Hi"); } };
const obj3 = Object.create(proto);

// 4. ES6 Class
class User {
  constructor(name) {
    this.name = name;
  }
}
const obj4 = new User("Alice");

// 5. Factory Function
function createPerson(name) {
  return { name };
}
const obj5 = createPerson("Bob");

// 6. new Object()
const obj6 = new Object();
obj6.name = "Charlie";
```

### 18. Generator Functions

Functions that can pause execution and resume later using `yield`.

```javascript
function* numberGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = numberGenerator();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }

// Infinite sequence
function* infiniteNumbers() {
  let i = 0;
  while (true) {
    yield i++;
  }
}
```

### 19. JavaScript Single Threaded Language

JavaScript has a single call stack and executes one task at a time. Asynchronous operations are handled through the event loop, callback queue, and Web APIs.

```
┌───────────────────────────┐
│    JavaScript Engine      │
│  ┌─────────────────────┐  │
│  │    Call Stack       │  │
│  └─────────────────────┘  │
└───────────────────────────┘
         │
         ▼
┌───────────────────────────┐
│      Event Loop           │
└───────────────────────────┘
         │
    ┌────┴────┐
    ▼         ▼
Callback  Microtask
 Queue     Queue
```

### 20. Callbacks and Why We Need Them

Callbacks are functions passed as arguments to other functions, executed after an asynchronous operation completes.

```javascript
// Without callback
function getData() {
  // Takes time to fetch
}
const data = getData();
console.log(data); // undefined (hasn't finished)

// With callback
function getData(callback) {
  setTimeout(() => {
    const data = { name: "John" };
    callback(data);
  }, 1000);
}

getData((data) => {
  console.log(data); // { name: "John" } after 1 second
});
```

### 21. Event Loop

The mechanism that allows JavaScript to perform non-blocking operations by offloading operations to the browser/Node.js APIs.

**Process**:
1. Execute synchronous code
2. Check microtask queue (Promise callbacks, queueMicrotask)
3. Check callback queue (setTimeout, setInterval, I/O)
4. Render (in browsers)
5. Repeat

```javascript
console.log('1');

setTimeout(() => console.log('2'), 0);

Promise.resolve().then(() => console.log('3'));

console.log('4');

// Output: 1, 4, 3, 2
```

### 22. Callback Queue (Macrotask Queue)

Holds callbacks from Web APIs like setTimeout, setInterval, I/O operations.

```javascript
setTimeout(() => console.log('Callback Queue'), 0);

console.log('Call Stack');

// Output:
// Call Stack
// Callback Queue
```

### 23. Microtask Queue

Has higher priority than callback queue. Includes Promise callbacks, MutationObserver, queueMicrotask.

```javascript
setTimeout(() => console.log('Macrotask'), 0);

Promise.resolve().then(() => console.log('Microtask 1'))
  .then(() => console.log('Microtask 2'));

console.log('Synchronous');

// Output:
// Synchronous
// Microtask 1
// Microtask 2
// Macrotask
```

### 24. Promises

Objects representing the eventual completion or failure of an asynchronous operation.

```javascript
const promise = new Promise((resolve, reject) => {
  const success = true;
  if (success) {
    resolve("Success!");
  } else {
    reject("Error!");
  }
});

promise
  .then(result => console.log(result))
  .catch(error => console.error(error))
  .finally(() => console.log("Done"));

// Promise states: pending, fulfilled, rejected
```

### 25. Event Propagation

The process of event flowing through the DOM tree.

**Three Phases**:
1. Capturing phase (from window to target)
2. Target phase (at the element)
3. Bubbling phase (from target to window)

```javascript
element.addEventListener('click', handler, true); // Capturing
element.addEventListener('click', handler, false); // Bubbling (default)
```

### 26. Event Bubbling

Events bubble up from the target element to its ancestors.

```html
<div id="parent">
  <button id="child">Click me</button>
</div>
```

```javascript
document.getElementById('parent').addEventListener('click', () => {
  console.log('Parent clicked');
});

document.getElementById('child').addEventListener('click', () => {
  console.log('Child clicked');
});

// Clicking button outputs:
// Child clicked
// Parent clicked
```

### 27. Event Capturing

Events capture down from the window to the target element.

```javascript
document.getElementById('parent').addEventListener('click', () => {
  console.log('Parent clicked');
}, true); // Capturing phase

document.getElementById('child').addEventListener('click', () => {
  console.log('Child clicked');
}, true);

// Clicking button outputs:
// Parent clicked
// Child clicked
```

### 28. Event Stop Propagation

Prevents the event from bubbling up or capturing down.

```javascript
document.getElementById('child').addEventListener('click', (e) => {
  e.stopPropagation(); // Stops bubbling
  console.log('Child clicked');
});

document.getElementById('parent').addEventListener('click', () => {
  console.log('Parent clicked'); // Won't execute
});
```

### 29. Event Delegation

Attaching a single event listener to a parent element to handle events for multiple children.

```javascript
document.getElementById('parent').addEventListener('click', (e) => {
  if (e.target.matches('button')) {
    console.log(`Button ${e.target.textContent} clicked`);
  }
});

// Works for dynamically added buttons too
```

### 30. Type Coercion vs Conversion

**Coercion** (implicit): JavaScript automatically converts types.

```javascript
console.log('5' + 3);   // '53' (number to string)
console.log('5' - 3);   // 2 (string to number)
console.log(true + 1);  // 2 (boolean to number)
```

**Conversion** (explicit): Developer explicitly converts types.

```javascript
Number('5');    // 5
String(123);    // '123'
Boolean(0);     // false
parseInt('10'); // 10
```

### 31. Throttle

Limits function execution to once per specified time period.

```javascript
function throttle(func, delay) {
  let lastCall = 0;
  return function(...args) {
    const now = Date.now();
    if (now - lastCall >= delay) {
      lastCall = now;
      func(...args);
    }
  };
}

const handleScroll = throttle(() => {
  console.log('Scrolling...');
}, 1000);

window.addEventListener('scroll', handleScroll);
// Executes at most once per second
```

### 32. Debounce

Delays function execution until after a specified time has passed since the last call.

```javascript
function debounce(func, delay) {
  let timeoutId;
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => {
      func(...args);
    }, delay);
  };
}

const handleInput = debounce((value) => {
  console.log('Searching for:', value);
}, 500);

input.addEventListener('input', (e) => handleInput(e.target.value));
// Executes only after 500ms of no typing
```

### 33. How JavaScript Parses and Compiles Code

**Step 1: Lexical Analysis (Tokenization)**
- Source code is broken into tokens (keywords, identifiers, operators)

**Step 2: Parsing**
- Tokens are converted into an Abstract Syntax Tree (AST)

**Step 3: Compilation**
- AST is converted to bytecode by the JavaScript engine
- Just-In-Time (JIT) compilation

**Step 4: Execution**
- Creation Phase: Memory allocation, hoisting
- Execution Phase: Code runs line by line

```javascript
// Memory Creation Phase
// - function declarations are hoisted
// - var declarations are hoisted (undefined)
// - let/const in TDZ

// Execution Phase
// - Code executes line by line
```

### 34. Handling Infinite Microtask Queue

If a microtask continuously adds new microtasks, it can block the event loop.

```javascript
// Problem: Infinite microtask loop
function infiniteMicrotask() {
  Promise.resolve().then(infiniteMicrotask);
}
infiniteMicrotask(); // Blocks event loop

// Solution 1: Use setTimeout (macrotask)
function controlled() {
  setTimeout(controlled, 0); // Allows other tasks to run
}

// Solution 2: Add conditions
function conditionalMicrotask(count) {
  if (count > 100) return; // Stop condition
  Promise.resolve().then(() => conditionalMicrotask(count + 1));
}

// Solution 3: Task scheduling
let taskCount = 0;
function batchedWork() {
  Promise.resolve().then(() => {
    if (++taskCount % 100 === 0) {
      setTimeout(batchedWork, 0); // Yield control
    } else {
      batchedWork();
    }
  });
}
```

### 35. First Class Functions

Functions are treated as values: assigned to variables, passed as arguments, returned from functions.

```javascript
// Assign to variable
const greet = function() { console.log("Hello"); };

// Pass as argument
function execute(fn) {
  fn();
}
execute(greet);

// Return from function
function createMultiplier(x) {
  return function(y) {
    return x * y;
  };
}
const double = createMultiplier(2);
console.log(double(5)); // 10
```

### 36. Immediately Invoked Function Expressions (IIFE)

Functions that execute immediately after creation.

```javascript
// Basic IIFE
(function() {
  console.log("I run immediately!");
})();

// With parameters
(function(name) {
  console.log(`Hello, ${name}`);
})("John");

// Arrow function IIFE
(() => {
  console.log("Arrow IIFE");
})();

// Use case: Create private scope
const counter = (function() {
  let count = 0;
  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count
  };
})();
```

### 37. Call, Apply, and Bind

Methods to set the `this` context of a function.

```javascript
const person = {
  name: "John",
  greet: function(greeting, punctuation) {
    console.log(`${greeting}, I'm ${this.name}${punctuation}`);
  }
};

const anotherPerson = { name: "Jane" };

// call: invoke immediately with arguments
person.greet.call(anotherPerson, "Hello", "!"); 
// "Hello, I'm Jane!"

// apply: invoke immediately with array of arguments
person.greet.apply(anotherPerson, ["Hi", "."]);
// "Hi, I'm Jane."

// bind: return new function with bound context
const boundGreet = person.greet.bind(anotherPerson, "Hey");
boundGreet("!!!"); // "Hey, I'm Jane!!!"
```

**Use Cases**:
- Borrowing methods
- Function currying
- Event handlers
- Preserving context

### 38. MapLimit

Execute async operations with concurrency limit.

```javascript
async function mapLimit(array, limit, asyncFunc) {
  const results = [];
  const executing = [];
  
  for (const [index, item] of array.entries()) {
    const promise = asyncFunc(item, index).then(result => {
      results[index] = result;
      executing.splice(executing.indexOf(promise), 1);
    });
    
    executing.push(promise);
    
    if (executing.length >= limit) {
      await Promise.race(executing);
    }
  }
  
  await Promise.all(executing);
  return results;
}

// Usage
const urls = ['url1', 'url2', 'url3', 'url4'];
await mapLimit(urls, 2, async (url) => {
  const response = await fetch(url);
  return response.json();
});
```

### 39. Async and Await

Syntactic sugar over Promises for cleaner asynchronous code.

```javascript
// With Promises
function fetchData() {
  return fetch('/api/data')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));
}

// With Async/Await
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}

// Parallel execution
async function fetchMultiple() {
  const [users, posts] = await Promise.all([
    fetch('/api/users').then(r => r.json()),
    fetch('/api/posts').then(r => r.json())
  ]);
}
```

### 40. Then and Catch

Promise methods for handling resolved and rejected states.

```javascript
fetch('/api/data')
  .then(response => response.json())
  .then(data => {
    console.log('Success:', data);
    return data;
  })
  .catch(error => {
    console.error('Error:', error);
  })
  .finally(() => {
    console.log('Cleanup');
  });

// Chaining
Promise.resolve(5)
  .then(x => x * 2)
  .then(x => x + 3)
  .then(x => console.log(x)); // 13

// Error propagation
Promise.reject('Error')
  .then(() => console.log('Won\'t run'))
  .catch(error => console.log('Caught:', error));
```

### 41. Variable Shadowing

When a variable in an inner scope has the same name as a variable in an outer scope.

```javascript
let x = 10;

function example() {
  let x = 20; // Shadows outer x
  console.log(x); // 20
}

example();
console.log(x); // 10

// Illegal shadowing (let cannot shadow var in same scope)
function illegal() {
  var a = 10;
  {
    let a = 20; // OK
  }
  // {
  //   let a = 30; // Error if var a is in same function
  // }
}
```

### 42. Static in JavaScript Class

Static methods/properties belong to the class itself, not instances.

```javascript
class MathHelper {
  static PI = 3.14159;
  
  static add(a, b) {
    return a + b;
  }
  
  static multiply(a, b) {
    return a * b;
  }
}

console.log(MathHelper.PI); // 3.14159
console.log(MathHelper.add(2, 3)); // 5

const helper = new MathHelper();
// console.log(helper.add(2, 3)); // Error: not a function

// Use case: Factory methods
class User {
  constructor(name) {
    this.name = name;
  }
  
  static create(name) {
    return new User(name);
  }
}

const user = User.create("John");
```

### 43. Undefined vs Not-Defined vs Null

**Undefined**: Variable declared but not assigned a value.

```javascript
let x;
console.log(x); // undefined
console.log(typeof x); // "undefined"
```

**Not-Defined**: Variable not declared at all.

```javascript
console.log(y); // ReferenceError: y is not defined
```

**Null**: Intentional absence of value.

```javascript
let z = null;
console.log(z); // null
console.log(typeof z); // "object" (historical bug)
```

### 44. Higher Order Functions

Functions that take functions as arguments or return functions.

```javascript
// Takes function as argument
function repeat(n, action) {
  for (let i = 0; i < n; i++) {
    action(i);
  }
}
repeat(3, console.log); // 0, 1, 2

// Returns function
function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
}
const double = multiplier(2);
console.log(double(5)); // 10

// Built-in HOFs
[1, 2, 3].map(x => x * 2);        // [2, 4, 6]
[1, 2, 3, 4].filter(x => x > 2);  // [3, 4]
[1, 2, 3].reduce((a, b) => a + b); // 6
```

### 45. Callback Hell

Nested callbacks making code hard to read and maintain.

```javascript
// Callback Hell (Pyramid of Doom)
getData(function(a) {
  getMoreData(a, function(b) {
    getEvenMoreData(b, function(c) {
      getFinalData(c, function(d) {
        console.log(d);
      });
    });
  });
});

// Solution 1: Named functions
function handleA(a) {
  getMoreData(a, handleB);
}
function handleB(b) {
  getEvenMoreData(b, handleC);
}
getData(handleA);

// Solution 2: Promises
getData()
  .then(a => getMoreData(a))
  .then(b => getEvenMoreData(b))
  .then(c => getFinalData(c))
  .then(d => console.log(d));

// Solution 3: Async/Await
async function fetchAllData() {
  const a = await getData();
  const b = await getMoreData(a);
  const c = await getEvenMoreData(b);
  const d = await getFinalData(c);
  console.log(d);
}
```

### 46. This Concept in JavaScript

`this` refers to the object that is executing the current function.

```javascript
// Global context
console.log(this); // window (browser) or global (Node.js)

// Object method
const obj = {
  name: "John",
  greet: function() {
    console.log(this.name); // "John"
  }
};

// Constructor function
function Person(name) {
  this.name = name; // this = new object
}
const person = new Person("Alice");

// Arrow functions (lexical this)
const obj2 = {
  name: "Jane",
  greet: () => {
    console.log(this.name); // undefined (inherits from outer scope)
  }
};

// Event handler
button.addEventListener('click', function() {
  console.log(this); // button element
});

// Explicit binding
function greet() {
  console.log(this.name);
}
greet.call({ name: "Bob" }); // "Bob"
```

### 47. Function Types

**Function Declaration**:
```javascript
function greet() {
  return "Hello";
}
// Hoisted, can be called before declaration
```

**Function Expression**:
```javascript
const greet = function() {
  return "Hello";
};
// Not hoisted, must be defined before use
```

**Anonymous Function**:
```javascript
setTimeout(function() {
  console.log("Anonymous");
}, 1000);
```

**Arrow Function**:
```javascript
const greet = () => "Hello";
const add = (a, b) => a + b;
const complex = (x) => {
  const y = x * 2;
  return y + 1;
};
// No own 'this', cannot be used as constructor
```

**Differences**:
- Arrow functions: No `this`, `arguments`, `super`
- Declarations are hoisted, expressions are not
- Arrow functions are concise for callbacks

### 48. Async vs Defer

Script loading attributes for HTML.

```html
<!-- Normal: Blocks HTML parsing -->
<script src="script.js"></script>

<!-- Async: Loads parallel, executes when ready (may block parsing) -->
<script async src="script.js"></script>

<!-- Defer: Loads parallel, executes after HTML parsed -->
<script defer src="script.js"></script>
```

**Comparison**:
| Feature | Normal | Async | Defer |
|---------|--------|-------|-------|
| Blocks parsing | Yes | Maybe | No |
| Execution order | Sequential | Random | Sequential |
| DOMContentLoaded | Waits | Doesn't wait | Waits |

**Use Cases**:
- `async`: Independent scripts (analytics)
- `defer`: Scripts that need DOM or depend on order

---

