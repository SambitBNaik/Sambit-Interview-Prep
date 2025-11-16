# JavaScript Polyfills & Utilities Guide

## 1. Array Methods: Map, Reduce, Filter

### Map Polyfill
```javascript
Array.prototype.myMap = function(callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    result.push(callback(this[i], i, this));
  }
  return result;
};

// Usage
const arr = [1, 2, 3];
const doubled = arr.myMap(x => x * 2); // [2, 4, 6]
```

### Filter Polyfill
```javascript
Array.prototype.myFilter = function(callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i], i, this)) {
      result.push(this[i]);
    }
  }
  return result;
};

// Usage
const arr = [1, 2, 3, 4];
const evens = arr.myFilter(x => x % 2 === 0); // [2, 4]
```

### Reduce Polyfill
```javascript
Array.prototype.myReduce = function(callback, initialValue) {
  let accumulator = initialValue;
  let startIndex = 0;
  
  if (initialValue === undefined) {
    accumulator = this[0];
    startIndex = 1;
  }
  
  for (let i = startIndex; i < this.length; i++) {
    accumulator = callback(accumulator, this[i], i, this);
  }
  
  return accumulator;
};

// Usage
const arr = [1, 2, 3, 4];
const sum = arr.myReduce((acc, val) => acc + val, 0); // 10
```

## 2. Function Methods: Call, Apply, Bind

### Call Polyfill
```javascript
Function.prototype.myCall = function(context, ...args) {
  context = context || globalThis;
  const fnSymbol = Symbol();
  context[fnSymbol] = this;
  const result = context[fnSymbol](...args);
  delete context[fnSymbol];
  return result;
};

// Usage
function greet(greeting) {
  return `${greeting}, ${this.name}`;
}
const person = { name: 'Alice' };
greet.myCall(person, 'Hello'); // "Hello, Alice"
```

### Apply Polyfill
```javascript
Function.prototype.myApply = function(context, argsArray) {
  context = context || globalThis;
  const fnSymbol = Symbol();
  context[fnSymbol] = this;
  const result = argsArray ? context[fnSymbol](...argsArray) : context[fnSymbol]();
  delete context[fnSymbol];
  return result;
};

// Usage
function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}
const person = { name: 'Bob' };
greet.myApply(person, ['Hi', '!']); // "Hi, Bob!"
```

### Bind Polyfill
```javascript
Function.prototype.myBind = function(context, ...boundArgs) {
  const fn = this;
  return function(...args) {
    return fn.apply(context, [...boundArgs, ...args]);
  };
};

// Usage
function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}
const person = { name: 'Charlie' };
const boundGreet = greet.myBind(person, 'Hey');
boundGreet('!!!'); // "Hey, Charlie!!!"
```

## 3. Promise Methods

### Promise Polyfill (Basic)
```javascript
class MyPromise {
  constructor(executor) {
    this.state = 'pending';
    this.value = undefined;
    this.handlers = [];
    
    const resolve = (value) => {
      if (this.state !== 'pending') return;
      this.state = 'fulfilled';
      this.value = value;
      this.handlers.forEach(handler => handler.onFulfilled(value));
    };
    
    const reject = (reason) => {
      if (this.state !== 'pending') return;
      this.state = 'rejected';
      this.value = reason;
      this.handlers.forEach(handler => handler.onRejected(reason));
    };
    
    try {
      executor(resolve, reject);
    } catch (error) {
      reject(error);
    }
  }
  
  then(onFulfilled, onRejected) {
    return new MyPromise((resolve, reject) => {
      const handle = () => {
        if (this.state === 'fulfilled') {
          try {
            const result = onFulfilled ? onFulfilled(this.value) : this.value;
            resolve(result);
          } catch (error) {
            reject(error);
          }
        } else if (this.state === 'rejected') {
          try {
            const result = onRejected ? onRejected(this.value) : this.value;
            resolve(result);
          } catch (error) {
            reject(error);
          }
        }
      };
      
      if (this.state === 'pending') {
        this.handlers.push({
          onFulfilled: (value) => {
            try {
              const result = onFulfilled ? onFulfilled(value) : value;
              resolve(result);
            } catch (error) {
              reject(error);
            }
          },
          onRejected: (reason) => {
            try {
              const result = onRejected ? onRejected(reason) : reason;
              reject(result);
            } catch (error) {
              reject(error);
            }
          }
        });
      } else {
        setTimeout(handle, 0);
      }
    });
  }
  
  catch(onRejected) {
    return this.then(null, onRejected);
  }
}
```

### Promise.all() Polyfill
```javascript
Promise.myAll = function(promises) {
  return new Promise((resolve, reject) => {
    if (!Array.isArray(promises)) {
      return reject(new TypeError('Argument must be an array'));
    }
    
    const results = [];
    let completedCount = 0;
    
    if (promises.length === 0) {
      return resolve(results);
    }
    
    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then(value => {
          results[index] = value;
          completedCount++;
          
          if (completedCount === promises.length) {
            resolve(results);
          }
        })
        .catch(reject);
    });
  });
};

// Usage
Promise.myAll([Promise.resolve(1), Promise.resolve(2)])
  .then(results => console.log(results)); // [1, 2]
```

### Promise.any() Polyfill
```javascript
Promise.myAny = function(promises) {
  return new Promise((resolve, reject) => {
    if (!Array.isArray(promises)) {
      return reject(new TypeError('Argument must be an array'));
    }
    
    const errors = [];
    let rejectedCount = 0;
    
    if (promises.length === 0) {
      return reject(new AggregateError(errors, 'All promises were rejected'));
    }
    
    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then(resolve)
        .catch(error => {
          errors[index] = error;
          rejectedCount++;
          
          if (rejectedCount === promises.length) {
            reject(new AggregateError(errors, 'All promises were rejected'));
          }
        });
    });
  });
};

// Usage
Promise.myAny([Promise.reject('Error 1'), Promise.resolve('Success')])
  .then(result => console.log(result)); // "Success"
```

### Promise.allSettled() Polyfill
```javascript
Promise.myAllSettled = function(promises) {
  return new Promise((resolve) => {
    if (!Array.isArray(promises)) {
      return resolve([]);
    }
    
    const results = [];
    let completedCount = 0;
    
    if (promises.length === 0) {
      return resolve(results);
    }
    
    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then(value => {
          results[index] = { status: 'fulfilled', value };
          completedCount++;
          
          if (completedCount === promises.length) {
            resolve(results);
          }
        })
        .catch(reason => {
          results[index] = { status: 'rejected', reason };
          completedCount++;
          
          if (completedCount === promises.length) {
            resolve(results);
          }
        });
    });
  });
};

// Usage
Promise.myAllSettled([Promise.resolve(1), Promise.reject('Error')])
  .then(results => console.log(results));
// [{ status: 'fulfilled', value: 1 }, { status: 'rejected', reason: 'Error' }]
```

### Promise.race() Polyfill
```javascript
Promise.myRace = function(promises) {
  return new Promise((resolve, reject) => {
    if (!Array.isArray(promises)) {
      return reject(new TypeError('Argument must be an array'));
    }
    
    promises.forEach(promise => {
      Promise.resolve(promise)
        .then(resolve)
        .catch(reject);
    });
  });
};

// Usage
Promise.myRace([
  new Promise(resolve => setTimeout(() => resolve('Fast'), 100)),
  new Promise(resolve => setTimeout(() => resolve('Slow'), 500))
]).then(result => console.log(result)); // "Fast"
```

## 4. Map Limit

```javascript
async function mapLimit(array, limit, asyncFn) {
  const results = [];
  const executing = [];
  
  for (let i = 0; i < array.length; i++) {
    const promise = Promise.resolve().then(() => asyncFn(array[i], i, array));
    results.push(promise);
    
    if (limit <= array.length) {
      const executing_promise = promise.then(() => 
        executing.splice(executing.indexOf(executing_promise), 1)
      );
      executing.push(executing_promise);
      
      if (executing.length >= limit) {
        await Promise.race(executing);
      }
    }
  }
  
  return Promise.all(results);
}

// Usage
const urls = ['url1', 'url2', 'url3', 'url4', 'url5'];
mapLimit(urls, 2, async (url) => {
  const response = await fetch(url);
  return response.json();
}).then(results => console.log(results));
```

## 5. Debounce

```javascript
function debounce(func, delay) {
  let timeoutId;
  
  return function(...args) {
    const context = this;
    
    clearTimeout(timeoutId);
    
    timeoutId = setTimeout(() => {
      func.apply(context, args);
    }, delay);
  };
}

// Usage
const handleSearch = debounce((query) => {
  console.log('Searching for:', query);
}, 300);

// Will only execute once after 300ms of no calls
handleSearch('a');
handleSearch('ab');
handleSearch('abc'); // Only this will execute after 300ms
```

## 6. Throttle

```javascript
function throttle(func, limit) {
  let inThrottle;
  
  return function(...args) {
    const context = this;
    
    if (!inThrottle) {
      func.apply(context, args);
      inThrottle = true;
      
      setTimeout(() => {
        inThrottle = false;
      }, limit);
    }
  };
}

// Alternative: Throttle with trailing call
function throttleTrailing(func, limit) {
  let inThrottle;
  let lastArgs;
  let lastContext;
  
  return function(...args) {
    lastArgs = args;
    lastContext = this;
    
    if (!inThrottle) {
      func.apply(lastContext, lastArgs);
      inThrottle = true;
      
      setTimeout(() => {
        inThrottle = false;
        if (lastArgs) {
          func.apply(lastContext, lastArgs);
          lastArgs = null;
        }
      }, limit);
    }
  };
}

// Usage
const handleScroll = throttle(() => {
  console.log('Scroll event handled');
}, 1000);

window.addEventListener('scroll', handleScroll);
// Will execute at most once per second
```

## 7. Event Emitter

```javascript
class EventEmitter {
  constructor() {
    this.events = {};
  }
  
  on(event, listener) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(listener);
    return this;
  }
  
  once(event, listener) {
    const onceWrapper = (...args) => {
      listener.apply(this, args);
      this.off(event, onceWrapper);
    };
    return this.on(event, onceWrapper);
  }
  
  emit(event, ...args) {
    if (!this.events[event]) {
      return false;
    }
    
    this.events[event].forEach(listener => {
      listener.apply(this, args);
    });
    
    return true;
  }
  
  off(event, listenerToRemove) {
    if (!this.events[event]) {
      return this;
    }
    
    this.events[event] = this.events[event].filter(
      listener => listener !== listenerToRemove
    );
    
    return this;
  }
  
  removeAllListeners(event) {
    if (event) {
      delete this.events[event];
    } else {
      this.events = {};
    }
    return this;
  }
  
  listenerCount(event) {
    return this.events[event] ? this.events[event].length : 0;
  }
}

// Usage
const emitter = new EventEmitter();

const greet = (name) => console.log(`Hello, ${name}`);
emitter.on('greet', greet);
emitter.emit('greet', 'Alice'); // "Hello, Alice"

emitter.once('goodbye', (name) => console.log(`Goodbye, ${name}`));
emitter.emit('goodbye', 'Bob'); // "Goodbye, Bob"
emitter.emit('goodbye', 'Charlie'); // Nothing happens
```

## 8. setInterval Polyfill

```javascript
function mySetInterval(callback, delay) {
  let timerId;
  let count = 0;
  
  function repeat() {
    timerId = setTimeout(() => {
      callback();
      count++;
      repeat();
    }, delay);
  }
  
  repeat();
  
  return {
    clear: () => clearTimeout(timerId),
    count: () => count
  };
}

// Usage
const interval = mySetInterval(() => {
  console.log('Tick');
}, 1000);

// To clear
setTimeout(() => {
  interval.clear();
}, 5000);
```

## 9. Parallel Limit Function

```javascript
async function parallelLimit(tasks, limit) {
  const results = [];
  const executing = [];
  
  for (let i = 0; i < tasks.length; i++) {
    const task = tasks[i];
    
    const promise = Promise.resolve().then(() => task());
    results.push(promise);
    
    if (limit <= tasks.length) {
      const executing_promise = promise.then(() => 
        executing.splice(executing.indexOf(executing_promise), 1)
      );
      executing.push(executing_promise);
      
      if (executing.length >= limit) {
        await Promise.race(executing);
      }
    }
  }
  
  return Promise.all(results);
}

// Alternative implementation with better error handling
async function parallelLimitWithErrors(tasks, limit) {
  const results = [];
  let index = 0;
  
  async function runTask() {
    while (index < tasks.length) {
      const currentIndex = index++;
      try {
        results[currentIndex] = await tasks[currentIndex]();
      } catch (error) {
        results[currentIndex] = { error };
      }
    }
  }
  
  const workers = Array(Math.min(limit, tasks.length))
    .fill()
    .map(() => runTask());
  
  await Promise.all(workers);
  return results;
}

// Usage
const tasks = [
  () => new Promise(resolve => setTimeout(() => resolve('Task 1'), 1000)),
  () => new Promise(resolve => setTimeout(() => resolve('Task 2'), 500)),
  () => new Promise(resolve => setTimeout(() => resolve('Task 3'), 800)),
  () => new Promise(resolve => setTimeout(() => resolve('Task 4'), 300))
];

parallelLimit(tasks, 2).then(results => console.log(results));
// Executes max 2 tasks at a time
```

## 10. Deep vs Shallow Copy

### Shallow Copy
```javascript
// Method 1: Spread operator
const obj = { a: 1, b: { c: 2 } };
const shallowCopy1 = { ...obj };

// Method 2: Object.assign
const shallowCopy2 = Object.assign({}, obj);

// Method 3: For arrays
const arr = [1, 2, [3, 4]];
const shallowArrCopy = [...arr];

// Issue with shallow copy:
shallowCopy1.b.c = 99;
console.log(obj.b.c); // 99 (nested object is shared!)
```

### Deep Copy
```javascript
function deepCopy(obj, hash = new WeakMap()) {
  // Handle null or non-object types
  if (obj === null || typeof obj !== 'object') {
    return obj;
  }
  
  // Handle Date
  if (obj instanceof Date) {
    return new Date(obj);
  }
  
  // Handle RegExp
  if (obj instanceof RegExp) {
    return new RegExp(obj);
  }
  
  // Handle circular references
  if (hash.has(obj)) {
    return hash.get(obj);
  }
  
  // Handle Array
  if (Array.isArray(obj)) {
    const arrCopy = [];
    hash.set(obj, arrCopy);
    obj.forEach((item, index) => {
      arrCopy[index] = deepCopy(item, hash);
    });
    return arrCopy;
  }
  
  // Handle Object
  const objCopy = {};
  hash.set(obj, objCopy);
  
  Object.keys(obj).forEach(key => {
    objCopy[key] = deepCopy(obj[key], hash);
  });
  
  return objCopy;
}

// Usage
const original = {
  a: 1,
  b: { c: 2 },
  d: [3, 4, { e: 5 }]
};

const deep = deepCopy(original);
deep.b.c = 99;
console.log(original.b.c); // 2 (unchanged!)

// Simple deep copy (doesn't handle functions, Date, RegExp, circular refs)
const simpleDeepCopy = JSON.parse(JSON.stringify(original));
```

## 11. Flatten Deeply Nested Object

```javascript
function flattenObject(obj, prefix = '', result = {}) {
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      const newKey = prefix ? `${prefix}.${key}` : key;
      
      if (typeof obj[key] === 'object' && obj[key] !== null && !Array.isArray(obj[key])) {
        flattenObject(obj[key], newKey, result);
      } else {
        result[newKey] = obj[key];
      }
    }
  }
  return result;
}

// Alternative: Flatten with custom separator
function flattenWithSeparator(obj, separator = '.') {
  const result = {};
  
  function flatten(current, prefix = '') {
    for (let key in current) {
      if (current.hasOwnProperty(key)) {
        const newKey = prefix ? `${prefix}${separator}${key}` : key;
        
        if (typeof current[key] === 'object' && 
            current[key] !== null && 
            !Array.isArray(current[key])) {
          flatten(current[key], newKey);
        } else {
          result[newKey] = current[key];
        }
      }
    }
  }
  
  flatten(obj);
  return result;
}

// Usage
const nested = {
  a: 1,
  b: {
    c: 2,
    d: {
      e: 3,
      f: 4
    }
  },
  g: 5
};

const flattened = flattenObject(nested);
console.log(flattened);
// { 'a': 1, 'b.c': 2, 'b.d.e': 3, 'b.d.f': 4, 'g': 5 }

// Unflatten
function unflattenObject(obj, separator = '.') {
  const result = {};
  
  for (let key in obj) {
    const keys = key.split(separator);
    let current = result;
    
    for (let i = 0; i < keys.length - 1; i++) {
      if (!current[keys[i]]) {
        current[keys[i]] = {};
      }
      current = current[keys[i]];
    }
    
    current[keys[keys.length - 1]] = obj[key];
  }
  
  return result;
}

const unflattened = unflattenObject(flattened);
console.log(unflattened); // Original nested structure
```

## 12. Memoization / Caching

### Basic Memoization
```javascript
function memoize(fn) {
  const cache = {};
  
  return function(...args) {
    const key = JSON.stringify(args);
    
    if (key in cache) {
      console.log('Returning from cache');
      return cache[key];
    }
    
    console.log('Computing result');
    const result = fn.apply(this, args);
    cache[key] = result;
    return result;
  };
}

// Usage
const fibonacci = memoize(function(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
});

console.log(fibonacci(40)); // Fast!
```

### Advanced Memoization with LRU Cache
```javascript
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.cache = new Map();
  }
  
  get(key) {
    if (!this.cache.has(key)) {
      return undefined;
    }
    
    // Move to end (most recently used)
    const value = this.cache.get(key);
    this.cache.delete(key);
    this.cache.set(key, value);
    return value;
  }
  
  put(key, value) {
    // Delete if exists (to reinsert at end)
    if (this.cache.has(key)) {
      this.cache.delete(key);
    }
    
    // Add new entry
    this.cache.set(key, value);
    
    // Remove oldest if over capacity
    if (this.cache.size > this.capacity) {
      const oldestKey = this.cache.keys().next().value;
      this.cache.delete(oldestKey);
    }
  }
}

function memoizeWithLRU(fn, capacity = 100) {
  const cache = new LRUCache(capacity);
  
  return function(...args) {
    const key = JSON.stringify(args);
    const cached = cache.get(key);
    
    if (cached !== undefined) {
      return cached;
    }
    
    const result = fn.apply(this, args);
    cache.put(key, result);
    return result;
  };
}

// Memoization with custom key resolver
function memoizeWithResolver(fn, resolver) {
  const cache = new Map();
  
  return function(...args) {
    const key = resolver ? resolver(...args) : JSON.stringify(args);
    
    if (cache.has(key)) {
      return cache.get(key);
    }
    
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

// Usage
const expensiveOperation = memoizeWithResolver(
  (obj, id) => {
    console.log('Computing...');
    return obj.data[id];
  },
  (obj, id) => id // Use only id as cache key
);
```

### Memoization with Expiry
```javascript
function memoizeWithExpiry(fn, ttl = 5000) {
  const cache = new Map();
  
  return function(...args) {
    const key = JSON.stringify(args);
    const cached = cache.get(key);
    
    if (cached && Date.now() - cached.timestamp < ttl) {
      return cached.value;
    }
    
    const result = fn.apply(this, args);
    cache.set(key, {
      value: result,
      timestamp: Date.now()
    });
    
    return result;
  };
}

// Usage
const fetchData = memoizeWithExpiry(async (id) => {
  const response = await fetch(`/api/data/${id}`);
  return response.json();
}, 10000); // Cache for 10 seconds
```

---

## Summary

This guide covers essential JavaScript polyfills and utility functions commonly asked in interviews:

- **Array methods** provide custom implementations of map, filter, and reduce
- **Function methods** demonstrate how call, apply, and bind work internally
- **Promise utilities** handle asynchronous operations with various strategies
- **Rate limiting** functions (debounce, throttle, mapLimit) control execution frequency
- **Event handling** with a custom event emitter
- **Object manipulation** techniques for copying and flattening
- **Optimization** through memoization and caching strategies

Each implementation includes usage examples and handles edge cases for production-ready code.