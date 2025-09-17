# JavaScript Polyfills - Complete Guide (Easy to Hard)

## Table of Contents
1. [What are Polyfills?](#what-are-polyfills)
2. [Easy Level Polyfills](#easy-level-polyfills)
3. [Medium Level Polyfills](#medium-level-polyfills)
4. [Hard Level Polyfills](#hard-level-polyfills)
5. [Advanced Polyfills](#advanced-polyfills)
6. [Browser API Polyfills](#browser-api-polyfills)
7. [ES6+ Feature Polyfills](#es6-feature-polyfills)
8. [Testing Your Polyfills](#testing-your-polyfills)

---

## What are Polyfills?

A **polyfill** is a piece of code that provides functionality that is not natively supported by older browsers. It "fills in" the gaps by implementing missing features using existing JavaScript capabilities.

**Why are polyfills important?**
- Ensure compatibility across different browsers
- Allow use of modern JavaScript features in older environments
- Maintain consistent behavior across platforms

---

## Easy Level Polyfills

### 1. Array.prototype.forEach() 🟢
**Difficulty:** Easy  
**Browser Support:** IE9+

```javascript
if (!Array.prototype.forEach) {
    Array.prototype.forEach = function(callback, thisArg) {
        if (this == null) {
            throw new TypeError('Array.prototype.forEach called on null or undefined');
        }
        
        if (typeof callback !== 'function') {
            throw new TypeError(callback + ' is not a function');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        
        for (let i = 0; i < len; i++) {
            if (i in O) {
                callback.call(thisArg, O[i], i, O);
            }
        }
    };
}

// Test
const arr = [1, 2, 3, 4];
arr.forEach(function(item, index) {
    console.log(`Item at ${index}: ${item}`);
});
```

### 2. Array.prototype.map() 🟢
**Difficulty:** Easy  
**Browser Support:** IE9+

```javascript
if (!Array.prototype.map) {
    Array.prototype.map = function(callback, thisArg) {
        if (this == null) {
            throw new TypeError('Array.prototype.map called on null or undefined');
        }
        
        if (typeof callback !== 'function') {
            throw new TypeError(callback + ' is not a function');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        const result = new Array(len);
        
        for (let i = 0; i < len; i++) {
            if (i in O) {
                result[i] = callback.call(thisArg, O[i], i, O);
            }
        }
        
        return result;
    };
}

// Test
const numbers = [1, 2, 3, 4];
const doubled = numbers.map(x => x * 2);
console.log(doubled); // [2, 4, 6, 8]
```

### 3. Array.prototype.filter() 🟢
**Difficulty:** Easy  
**Browser Support:** IE9+

```javascript
if (!Array.prototype.filter) {
    Array.prototype.filter = function(callback, thisArg) {
        if (this == null) {
            throw new TypeError('Array.prototype.filter called on null or undefined');
        }
        
        if (typeof callback !== 'function') {
            throw new TypeError(callback + ' is not a function');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        const result = [];
        
        for (let i = 0; i < len; i++) {
            if (i in O) {
                const value = O[i];
                if (callback.call(thisArg, value, i, O)) {
                    result.push(value);
                }
            }
        }
        
        return result;
    };
}

// Test
const numbers = [1, 2, 3, 4, 5, 6];
const evens = numbers.filter(x => x % 2 === 0);
console.log(evens); // [2, 4, 6]
```

### 4. Array.prototype.indexOf() 🟢
**Difficulty:** Easy  
**Browser Support:** IE9+

```javascript
if (!Array.prototype.indexOf) {
    Array.prototype.indexOf = function(searchElement, fromIndex) {
        if (this == null) {
            throw new TypeError('Array.prototype.indexOf called on null or undefined');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        
        if (len === 0) return -1;
        
        let n = parseInt(fromIndex) || 0;
        if (n >= len) return -1;
        
        if (n < 0) {
            n = Math.max(len + n, 0);
        }
        
        for (let i = n; i < len; i++) {
            if (i in O && O[i] === searchElement) {
                return i;
            }
        }
        
        return -1;
    };
}

// Test
const fruits = ['apple', 'banana', 'orange'];
console.log(fruits.indexOf('banana')); // 1
console.log(fruits.indexOf('grape'));  // -1
```

### 5. String.prototype.trim() 🟢
**Difficulty:** Easy  
**Browser Support:** IE9+

```javascript
if (!String.prototype.trim) {
    String.prototype.trim = function() {
        return this.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, '');
    };
}

// Test
const str = '   Hello World   ';
console.log(str.trim()); // "Hello World"
```

---

## Medium Level Polyfills

### 6. Array.prototype.reduce() 🟡
**Difficulty:** Medium  
**Browser Support:** IE9+

```javascript
if (!Array.prototype.reduce) {
    Array.prototype.reduce = function(callback, initialValue) {
        if (this == null) {
            throw new TypeError('Array.prototype.reduce called on null or undefined');
        }
        
        if (typeof callback !== 'function') {
            throw new TypeError(callback + ' is not a function');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        let k = 0;
        let accumulator;
        
        if (arguments.length >= 2) {
            accumulator = initialValue;
        } else {
            let kPresent = false;
            while (k < len && !kPresent) {
                if (k in O) {
                    accumulator = O[k];
                    kPresent = true;
                }
                k++;
            }
            if (!kPresent) {
                throw new TypeError('Reduce of empty array with no initial value');
            }
        }
        
        while (k < len) {
            if (k in O) {
                accumulator = callback(accumulator, O[k], k, O);
            }
            k++;
        }
        
        return accumulator;
    };
}

// Test
const numbers = [1, 2, 3, 4, 5];
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 15
```

### 7. Array.prototype.find() 🟡
**Difficulty:** Medium  
**Browser Support:** ES6+

```javascript
if (!Array.prototype.find) {
    Array.prototype.find = function(callback, thisArg) {
        if (this == null) {
            throw new TypeError('Array.prototype.find called on null or undefined');
        }
        
        if (typeof callback !== 'function') {
            throw new TypeError(callback + ' is not a function');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        
        for (let i = 0; i < len; i++) {
            if (i in O) {
                const value = O[i];
                if (callback.call(thisArg, value, i, O)) {
                    return value;
                }
            }
        }
        
        return undefined;
    };
}

// Test
const users = [
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' },
    { id: 3, name: 'Bob' }
];
const user = users.find(u => u.id === 2);
console.log(user); // { id: 2, name: 'Jane' }
```

### 8. Array.prototype.includes() 🟡
**Difficulty:** Medium  
**Browser Support:** ES7+

```javascript
if (!Array.prototype.includes) {
    Array.prototype.includes = function(searchElement, fromIndex) {
        if (this == null) {
            throw new TypeError('Array.prototype.includes called on null or undefined');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        
        if (len === 0) return false;
        
        let n = parseInt(fromIndex) || 0;
        if (n >= len) return false;
        
        if (n < 0) {
            n = Math.max(len + n, 0);
        }
        
        // Handle NaN case
        const sameValueZero = (x, y) => {
            return x === y || (typeof x === 'number' && typeof y === 'number' && isNaN(x) && isNaN(y));
        };
        
        for (let i = n; i < len; i++) {
            if (sameValueZero(O[i], searchElement)) {
                return true;
            }
        }
        
        return false;
    };
}

// Test
const arr = [1, 2, 3, NaN];
console.log(arr.includes(2));   // true
console.log(arr.includes(NaN)); // true
console.log(arr.includes(4));   // false
```

### 9. Object.assign() 🟡
**Difficulty:** Medium  
**Browser Support:** ES6+

```javascript
if (!Object.assign) {
    Object.assign = function(target, ...sources) {
        if (target == null) {
            throw new TypeError('Cannot convert undefined or null to object');
        }
        
        const to = Object(target);
        
        for (let i = 0; i < sources.length; i++) {
            const nextSource = sources[i];
            if (nextSource != null) {
                for (const nextKey in nextSource) {
                    if (Object.prototype.hasOwnProperty.call(nextSource, nextKey)) {
                        to[nextKey] = nextSource[nextKey];
                    }
                }
            }
        }
        
        return to;
    };
}

// Test
const target = { a: 1, b: 2 };
const source = { b: 3, c: 4 };
const result = Object.assign(target, source);
console.log(result); // { a: 1, b: 3, c: 4 }
```

### 10. Function.prototype.bind() 🟡
**Difficulty:** Medium  
**Browser Support:** IE9+

```javascript
if (!Function.prototype.bind) {
    Function.prototype.bind = function(thisArg, ...args) {
        if (typeof this !== 'function') {
            throw new TypeError('Function.prototype.bind - what is trying to be bound is not callable');
        }
        
        const fToBind = this;
        const fNOP = function() {};
        const fBound = function(...newArgs) {
            return fToBind.apply(
                this instanceof fNOP ? this : thisArg,
                args.concat(newArgs)
            );
        };
        
        if (this.prototype) {
            fNOP.prototype = this.prototype;
        }
        fBound.prototype = new fNOP();
        
        return fBound;
    };
}

// Test
function greet(greeting, punctuation) {
    return `${greeting} ${this.name}${punctuation}`;
}

const person = { name: 'John' };
const boundGreet = greet.bind(person, 'Hello');
console.log(boundGreet('!')); // "Hello John!"
```

---

## Hard Level Polyfills

### 11. Promise 🔴
**Difficulty:** Hard  
**Browser Support:** ES6+

```javascript
if (typeof Promise === 'undefined') {
    function MyPromise(executor) {
        const self = this;
        self.state = 'pending';
        self.value = undefined;
        self.onFulfilledCallbacks = [];
        self.onRejectedCallbacks = [];
        
        function resolve(value) {
            if (self.state === 'pending') {
                self.state = 'fulfilled';
                self.value = value;
                self.onFulfilledCallbacks.forEach(callback => callback(value));
            }
        }
        
        function reject(reason) {
            if (self.state === 'pending') {
                self.state = 'rejected';
                self.value = reason;
                self.onRejectedCallbacks.forEach(callback => callback(reason));
            }
        }
        
        try {
            executor(resolve, reject);
        } catch (error) {
            reject(error);
        }
    }
    
    MyPromise.prototype.then = function(onFulfilled, onRejected) {
        const self = this;
        
        return new MyPromise((resolve, reject) => {
            function handleFulfilled(value) {
                try {
                    if (typeof onFulfilled === 'function') {
                        const result = onFulfilled(value);
                        resolve(result);
                    } else {
                        resolve(value);
                    }
                } catch (error) {
                    reject(error);
                }
            }
            
            function handleRejected(reason) {
                try {
                    if (typeof onRejected === 'function') {
                        const result = onRejected(reason);
                        resolve(result);
                    } else {
                        reject(reason);
                    }
                } catch (error) {
                    reject(error);
                }
            }
            
            if (self.state === 'fulfilled') {
                setTimeout(() => handleFulfilled(self.value), 0);
            } else if (self.state === 'rejected') {
                setTimeout(() => handleRejected(self.value), 0);
            } else {
                self.onFulfilledCallbacks.push(handleFulfilled);
                self.onRejectedCallbacks.push(handleRejected);
            }
        });
    };
    
    MyPromise.prototype.catch = function(onRejected) {
        return this.then(null, onRejected);
    };
    
    MyPromise.resolve = function(value) {
        return new MyPromise((resolve) => resolve(value));
    };
    
    MyPromise.reject = function(reason) {
        return new MyPromise((resolve, reject) => reject(reason));
    };
    
    window.Promise = MyPromise;
}

// Test
const promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve('Success!'), 1000);
});

promise.then(result => console.log(result)); // "Success!" after 1 second
```

### 12. Array.from() 🔴
**Difficulty:** Hard  
**Browser Support:** ES6+

```javascript
if (!Array.from) {
    Array.from = function(arrayLike, mapFn, thisArg) {
        if (arrayLike == null) {
            throw new TypeError('Array.from requires an array-like object - not null or undefined');
        }
        
        if (mapFn != null && typeof mapFn !== 'function') {
            throw new TypeError('Array.from: when provided, the second argument must be a function');
        }
        
        const C = this;
        const items = Object(arrayLike);
        
        // Handle iterables (objects with Symbol.iterator)
        if (typeof Symbol !== 'undefined' && Symbol.iterator && items[Symbol.iterator]) {
            const iterator = items[Symbol.iterator]();
            const result = typeof C === 'function' ? new C() : [];
            let k = 0;
            let next;
            
            while (!(next = iterator.next()).done) {
                const value = next.value;
                const mappedValue = mapFn ? mapFn.call(thisArg, value, k) : value;
                
                if (typeof C === 'function') {
                    result[k] = mappedValue;
                } else {
                    result.push(mappedValue);
                }
                k++;
            }
            
            if (typeof C === 'function') {
                result.length = k;
            }
            
            return result;
        }
        
        // Handle array-like objects
        const len = parseInt(items.length) || 0;
        const result = typeof C === 'function' ? new C(len) : new Array(len);
        
        for (let k = 0; k < len; k++) {
            const value = items[k];
            const mappedValue = mapFn ? mapFn.call(thisArg, value, k) : value;
            result[k] = mappedValue;
        }
        
        return result;
    };
}

// Test
const str = 'hello';
const arr1 = Array.from(str); // ['h', 'e', 'l', 'l', 'o']

const nodeList = document.querySelectorAll('div');
const arr2 = Array.from(nodeList, node => node.textContent);

console.log(arr1);
```

### 13. Object.create() 🔴
**Difficulty:** Hard  
**Browser Support:** IE9+

```javascript
if (!Object.create) {
    Object.create = function(proto, propertiesObject) {
        if (typeof proto !== 'object' && typeof proto !== 'function') {
            throw new TypeError('Object prototype may only be an Object or null: ' + proto);
        }
        
        function F() {}
        F.prototype = proto;
        const obj = new F();
        
        if (propertiesObject != null) {
            if (typeof propertiesObject !== 'object') {
                throw new TypeError('Properties object must be an object: ' + propertiesObject);
            }
            
            // Simple implementation of property descriptors
            for (const key in propertiesObject) {
                if (propertiesObject.hasOwnProperty(key)) {
                    const descriptor = propertiesObject[key];
                    if (descriptor && typeof descriptor === 'object') {
                        if ('value' in descriptor) {
                            obj[key] = descriptor.value;
                        }
                        if ('get' in descriptor && typeof descriptor.get === 'function') {
                            obj.__defineGetter__(key, descriptor.get);
                        }
                        if ('set' in descriptor && typeof descriptor.set === 'function') {
                            obj.__defineSetter__(key, descriptor.set);
                        }
                    }
                }
            }
        }
        
        return obj;
    };
}

// Test
const parent = { name: 'Parent' };
const child = Object.create(parent, {
    age: { value: 25, writable: true }
});

console.log(child.name); // "Parent" (inherited)
console.log(child.age);  // 25 (own property)
```

### 14. Array.prototype.flat() 🔴
**Difficulty:** Hard  
**Browser Support:** ES2019+

```javascript
if (!Array.prototype.flat) {
    Array.prototype.flat = function(depth = 1) {
        if (this == null) {
            throw new TypeError('Array.prototype.flat called on null or undefined');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        const depthNum = depth === Infinity ? Number.MAX_SAFE_INTEGER : Number(depth);
        
        function flattenArray(arr, currentDepth) {
            const result = [];
            
            for (let i = 0; i < arr.length; i++) {
                if (i in arr) {
                    const element = arr[i];
                    if (Array.isArray(element) && currentDepth > 0) {
                        result.push(...flattenArray(element, currentDepth - 1));
                    } else {
                        result.push(element);
                    }
                }
            }
            
            return result;
        }
        
        return flattenArray(O, Math.max(0, depthNum));
    };
}

// Test
const nested = [1, [2, [3, [4, 5]]]];
console.log(nested.flat(1)); // [1, 2, [3, [4, 5]]]
console.log(nested.flat(2)); // [1, 2, 3, [4, 5]]
console.log(nested.flat(Infinity)); // [1, 2, 3, 4, 5]
```

### 15. String.prototype.startsWith() 🔴
**Difficulty:** Hard  
**Browser Support:** ES6+

```javascript
if (!String.prototype.startsWith) {
    String.prototype.startsWith = function(searchString, position) {
        if (this == null) {
            throw new TypeError('String.prototype.startsWith called on null or undefined');
        }
        
        if (searchString instanceof RegExp) {
            throw new TypeError('First argument to String.prototype.startsWith must not be a regular expression');
        }
        
        const str = String(this);
        const searchStr = String(searchString);
        const pos = Math.max(0, Math.floor(position) || 0);
        
        return str.slice(pos, pos + searchStr.length) === searchStr;
    };
}

// Test
const str = 'Hello World';
console.log(str.startsWith('Hello')); // true
console.log(str.startsWith('World', 6)); // true
console.log(str.startsWith('Hi')); // false
```

---

## Advanced Polyfills

### 16. Fetch API 🔴
**Difficulty:** Advanced  
**Browser Support:** Modern browsers

```javascript
if (!window.fetch) {
    window.fetch = function(url, options = {}) {
        return new Promise((resolve, reject) => {
            const xhr = new XMLHttpRequest();
            const method = options.method || 'GET';
            const headers = options.headers || {};
            const body = options.body;
            
            xhr.open(method, url);
            
            // Set headers
            Object.keys(headers).forEach(key => {
                xhr.setRequestHeader(key, headers[key]);
            });
            
            xhr.onload = function() {
                const response = {
                    ok: xhr.status >= 200 && xhr.status < 300,
                    status: xhr.status,
                    statusText: xhr.statusText,
                    url: xhr.responseURL,
                    text: () => Promise.resolve(xhr.responseText),
                    json: () => Promise.resolve(JSON.parse(xhr.responseText)),
                    blob: () => Promise.resolve(new Blob([xhr.response])),
                    headers: {
                        get: function(name) {
                            return xhr.getResponseHeader(name);
                        }
                    }
                };
                
                resolve(response);
            };
            
            xhr.onerror = function() {
                reject(new TypeError('Network request failed'));
            };
            
            xhr.ontimeout = function() {
                reject(new TypeError('Network request timed out'));
            };
            
            xhr.send(body);
        });
    };
}

// Test
fetch('/api/data')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error('Error:', error));
```

### 17. Array.prototype.flatMap() 🔴
**Difficulty:** Advanced  
**Browser Support:** ES2019+

```javascript
if (!Array.prototype.flatMap) {
    Array.prototype.flatMap = function(callback, thisArg) {
        if (this == null) {
            throw new TypeError('Array.prototype.flatMap called on null or undefined');
        }
        
        if (typeof callback !== 'function') {
            throw new TypeError(callback + ' is not a function');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        const result = [];
        
        for (let i = 0; i < len; i++) {
            if (i in O) {
                const mappedValue = callback.call(thisArg, O[i], i, O);
                
                if (Array.isArray(mappedValue)) {
                    result.push(...mappedValue);
                } else {
                    result.push(mappedValue);
                }
            }
        }
        
        return result;
    };
}

// Test
const arr = [1, 2, 3, 4];
const result = arr.flatMap(x => [x, x * 2]);
console.log(result); // [1, 2, 2, 4, 3, 6, 4, 8]
```

### 18. Object.entries() 🔴
**Difficulty:** Advanced  
**Browser Support:** ES2017+

```javascript
if (!Object.entries) {
    Object.entries = function(obj) {
        if (obj == null) {
            throw new TypeError('Object.entries called on null or undefined');
        }
        
        const O = Object(obj);
        const result = [];
        
        for (const key in O) {
            if (Object.prototype.hasOwnProperty.call(O, key)) {
                result.push([key, O[key]]);
            }
        }
        
        return result;
    };
}

// Test
const obj = { a: 1, b: 2, c: 3 };
const entries = Object.entries(obj);
console.log(entries); // [['a', 1], ['b', 2], ['c', 3]]
```

### 19. Object.values() 🔴
**Difficulty:** Advanced  
**Browser Support:** ES2017+

```javascript
if (!Object.values) {
    Object.values = function(obj) {
        if (obj == null) {
            throw new TypeError('Object.values called on null or undefined');
        }
        
        const O = Object(obj);
        const result = [];
        
        for (const key in O) {
            if (Object.prototype.hasOwnProperty.call(O, key)) {
                result.push(O[key]);
            }
        }
        
        return result;
    };
}

// Test
const obj = { a: 1, b: 2, c: 3 };
const values = Object.values(obj);
console.log(values); // [1, 2, 3]
```

---

## Browser API Polyfills

### 20. Element.matches() 🟡
**Difficulty:** Medium  
**Browser Support:** IE9+

```javascript
if (!Element.prototype.matches) {
    Element.prototype.matches = 
        Element.prototype.matchesSelector ||
        Element.prototype.mozMatchesSelector ||
        Element.prototype.msMatchesSelector ||
        Element.prototype.oMatchesSelector ||
        Element.prototype.webkitMatchesSelector ||
        function(s) {
            const matches = (this.document || this.ownerDocument).querySelectorAll(s);
            let i = matches.length;
            while (--i >= 0 && matches.item(i) !== this) {}
            return i > -1;
        };
}

// Test
const element = document.querySelector('.my-class');
console.log(element.matches('.my-class')); // true
```

### 21. Element.closest() 🟡
**Difficulty:** Medium  
**Browser Support:** Modern browsers

```javascript
if (!Element.prototype.closest) {
    Element.prototype.closest = function(s) {
        let el = this;
        
        if (!document.documentElement.contains(el)) return null;
        
        do {
            if (el.matches(s)) return el;
            el = el.parentElement || el.parentNode;
        } while (el !== null && el.nodeType === 1);
        
        return null;
    };
}

// Test
const button = document.querySelector('button');
const form = button.closest('form');
console.log(form); // Returns the closest form element
```

### 22. Array.prototype.some() 🟡
**Difficulty:** Medium  
**Browser Support:** IE9+

```javascript
if (!Array.prototype.some) {
    Array.prototype.some = function(callback, thisArg) {
        if (this == null) {
            throw new TypeError('Array.prototype.some called on null or undefined');
        }
        
        if (typeof callback !== 'function') {
            throw new TypeError(callback + ' is not a function');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        
        for (let i = 0; i < len; i++) {
            if (i in O && callback.call(thisArg, O[i], i, O)) {
                return true;
            }
        }
        
        return false;
    };
}

// Test
const numbers = [1, 2, 3, 4, 5];
const hasEven = numbers.some(x => x % 2 === 0);
console.log(hasEven); // true
```

### 23. Array.prototype.every() 🟡
**Difficulty:** Medium  
**Browser Support:** IE9+

```javascript
if (!Array.prototype.every) {
    Array.prototype.every = function(callback, thisArg) {
        if (this == null) {
            throw new TypeError('Array.prototype.every called on null or undefined');
        }
        
        if (typeof callback !== 'function') {
            throw new TypeError(callback + ' is not a function');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        
        for (let i = 0; i < len; i++) {
            if (i in O && !callback.call(thisArg, O[i], i, O)) {
                return false;
            }
        }
        
        return true;
    };
}

// Test
const numbers = [2, 4, 6, 8];
const allEven = numbers.every(x => x % 2 === 0);
console.log(allEven); // true
```

---

## ES6+ Feature Polyfills

### 24. String.prototype.padStart() 🟡
**Difficulty:** Medium  
**Browser Support:** ES2017+

```javascript
if (!String.prototype.padStart) {
    String.prototype.padStart = function(targetLength, padString) {
        if (this == null) {
            throw new TypeError('String.prototype.padStart called on null or undefined');
        }
        
        const str = String(this);
        const len = parseInt(targetLength) || 0;
        
        if (len <= str.length) {
            return str;
        }
        
        const pad = padString == null ? ' ' : String(padString);
        if (pad.length === 0) {
            return str;
        }
        
        const padLength = len - str.length;
        let padRepeated = '';
        
        for (let i = 0; i < Math.ceil(padLength / pad.length); i++) {
            padRepeated += pad;
        }
        
        return padRepeated.slice(0, padLength) + str;
    };
}

// Test
const str = '5';
console.log(str.padStart(3, '0')); // "005"
```

### 25. String.prototype.padEnd() 🟡
**Difficulty:** Medium  
**Browser Support:** ES2017+

```javascript
if (!String.prototype.padEnd) {
    String.prototype.padEnd = function(targetLength, padString) {
        if (this == null) {
            throw new TypeError('String.prototype.padEnd called on null or undefined');
        }
        
        const str = String(this);
        const len = parseInt(targetLength) || 0;
        
        if (len <= str.length) {
            return str;
        }
        
        const pad = padString == null ? ' ' : String(padString);
        if (pad.length === 0) {
            return str;
        }
        
        const padLength = len - str.length;
        let padRepeated = '';
        
        for (let i = 0; i < Math.ceil(padLength / pad.length); i++) {
            padRepeated += pad;
        }
        
        return str + padRepeated.slice(0, padLength);
    };
}

// Test
const str = 'hello';
console.log(str.padEnd(10, '.')); // "hello....."
```

### 26. Array.prototype.findIndex() 🟡
**Difficulty:** Medium  
**Browser Support:** ES6+

```javascript
if (!Array.prototype.findIndex) {
    Array.prototype.findIndex = function(callback, thisArg) {
        if (this == null) {
            throw new TypeError('Array.prototype.findIndex called on null or undefined');
        }
        
        if (typeof callback !== 'function') {
            throw new TypeError(callback + ' is not a function');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        
        for (let i = 0; i < len; i++) {
            if (i in O && callback.call(thisArg, O[i], i, O)) {
                return i;
            }
        }
        
        return -1;
    };
}

// Test
const users = [
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' },
    { id: 3, name: 'Bob' }
];
const index = users.findIndex(u => u.name === 'Jane');
console.log(index); // 1
```

### 27. String.prototype.repeat() 🟡
**Difficulty:** Medium  
**Browser Support:** ES6+

```javascript
if (!String.prototype.repeat) {
    String.prototype.repeat = function(count) {
        if (this == null) {
            throw new TypeError('String.prototype.repeat called on null or undefined');
        }
        
        const str = String(this);
        const n = parseInt(count) || 0;
        
        if (n < 0 || n === Infinity) {
            throw new RangeError('Invalid count value');
        }
        
        if (n === 0) {
            return '';
        }
        
        let result = '';
        for (let i = 0; i < n; i++) {
            result += str;
        }
        
        return result;
    };
}

// Test
const str = 'abc';
console.log(str.repeat(3)); // "abcabcabc"
```

### 28. Object.keys() 🟡
**Difficulty:** Medium  
**Browser Support:** ES5+

```javascript
if (!Object.keys) {
    Object.keys = function(obj) {
        if (obj !== Object(obj)) {
            throw new TypeError('Object.keys called on a non-object');
        }
        
        const result = [];
        for (const key in obj) {
            if (Object.prototype.hasOwnProperty.call(obj, key)) {
                result.push(key);
            }
        }
        
        return result;
    };
}

// Test
const obj = { a: 1, b: 2, c: 3 };
const keys = Object.keys(obj);
console.log(keys); // ['a', 'b', 'c']
```

### 29. Number.isNaN() 🟡
**Difficulty:** Medium  
**Browser Support:** ES6+

```javascript
if (!Number.isNaN) {
    Number.isNaN = function(value) {
        // NaN is the only value that is not equal to itself
        return value !== value;
    };
}

// Test
console.log(Number.isNaN(NaN));       // true
console.log(Number.isNaN('hello'));   // false (unlike global isNaN)
console.log(Number.isNaN(123));       // false
```

### 30. Number.isInteger() 🟡
**Difficulty:** Medium  
**Browser Support:** ES6+

```javascript
if (!Number.isInteger) {
    Number.isInteger = function(value) {
        return typeof value === 'number' && 
               isFinite(value) && 
               Math.floor(value) === value;
    };
}

// Test
console.log(Number.isInteger(42));     // true
console.log(Number.isInteger(42.0));   // true
console.log(Number.isInteger(42.1));   // false
console.log(Number.isInteger('42'));   // false
```

---

## Advanced Implementation Patterns

### 31. Custom Event Polyfill 🔴
**Difficulty:** Advanced  
**Browser Support:** IE9+

```javascript
if (typeof window.CustomEvent !== 'function') {
    function CustomEvent(event, params) {
        params = params || { bubbles: false, cancelable: false, detail: undefined };
        const evt = document.createEvent('CustomEvent');
        evt.initCustomEvent(event, params.bubbles, params.cancelable, params.detail);
        return evt;
    }

    CustomEvent.prototype = window.Event.prototype;
    window.CustomEvent = CustomEvent;
}

// Test
const event = new CustomEvent('myEvent', {
    detail: { message: 'Hello World' },
    bubbles: true,
    cancelable: true
});

document.addEventListener('myEvent', function(e) {
    console.log('Custom event fired:', e.detail.message);
});

document.dispatchEvent(event);
```

### 32. requestAnimationFrame Polyfill 🔴
**Difficulty:** Advanced  
**Browser Support:** Modern browsers

```javascript
(function() {
    let lastTime = 0;
    const vendors = ['ms', 'moz', 'webkit', 'o'];
    
    for (let x = 0; x < vendors.length && !window.requestAnimationFrame; ++x) {
        window.requestAnimationFrame = window[vendors[x] + 'RequestAnimationFrame'];
        window.cancelAnimationFrame = window[vendors[x] + 'CancelAnimationFrame'] || 
                                    window[vendors[x] + 'CancelRequestAnimationFrame'];
    }
    
    if (!window.requestAnimationFrame) {
        window.requestAnimationFrame = function(callback) {
            const currTime = new Date().getTime();
            const timeToCall = Math.max(0, 16 - (currTime - lastTime));
            const id = window.setTimeout(function() {
                callback(currTime + timeToCall);
            }, timeToCall);
            lastTime = currTime + timeToCall;
            return id;
        };
    }
    
    if (!window.cancelAnimationFrame) {
        window.cancelAnimationFrame = function(id) {
            clearTimeout(id);
        };
    }
})();

// Test
function animate() {
    // Animation logic here
    console.log('Frame rendered');
    requestAnimationFrame(animate);
}

requestAnimationFrame(animate);
```

### 33. WeakMap Polyfill 🔴
**Difficulty:** Advanced  
**Browser Support:** ES6+

```javascript
if (typeof WeakMap === 'undefined') {
    (function() {
        const defineProperty = Object.defineProperty;
        const counter = Date.now() % 1e9;
        
        const WeakMap = function() {
            this.name = '__st' + (Math.random() * 1e9 >>> 0) + (counter++ + '__');
        };
        
        WeakMap.prototype = {
            set: function(key, value) {
                const entry = key[this.name];
                if (entry && entry[0] === key) {
                    entry[1] = value;
                } else {
                    defineProperty(key, this.name, {
                        value: [key, value],
                        writable: true
                    });
                }
                return this;
            },
            
            get: function(key) {
                const entry = key[this.name];
                return entry && entry[0] === key ? entry[1] : undefined;
            },
            
            delete: function(key) {
                const entry = key[this.name];
                if (!entry || entry[0] !== key) return false;
                entry[0] = entry[1] = undefined;
                return true;
            },
            
            has: function(key) {
                const entry = key[this.name];
                return entry ? entry[0] === key : false;
            }
        };
        
        window.WeakMap = WeakMap;
    })();
}

// Test
const wm = new WeakMap();
const obj = {};
wm.set(obj, 'value');
console.log(wm.get(obj)); // "value"
```

### 34. Map Polyfill 🔴
**Difficulty:** Advanced  
**Browser Support:** ES6+

```javascript
if (typeof Map === 'undefined') {
    function Map(iterable) {
        this._entries = [];
        this.size = 0;
        
        if (iterable) {
            for (const [key, value] of iterable) {
                this.set(key, value);
            }
        }
    }
    
    Map.prototype.set = function(key, value) {
        const entry = this._find(key);
        if (entry) {
            entry[1] = value;
        } else {
            this._entries.push([key, value]);
            this.size++;
        }
        return this;
    };
    
    Map.prototype.get = function(key) {
        const entry = this._find(key);
        return entry ? entry[1] : undefined;
    };
    
    Map.prototype.has = function(key) {
        return !!this._find(key);
    };
    
    Map.prototype.delete = function(key) {
        const index = this._findIndex(key);
        if (index >= 0) {
            this._entries.splice(index, 1);
            this.size--;
            return true;
        }
        return false;
    };
    
    Map.prototype.clear = function() {
        this._entries = [];
        this.size = 0;
    };
    
    Map.prototype._find = function(key) {
        for (const entry of this._entries) {
            if (this._sameValueZero(entry[0], key)) {
                return entry;
            }
        }
        return null;
    };
    
    Map.prototype._findIndex = function(key) {
        for (let i = 0; i < this._entries.length; i++) {
            if (this._sameValueZero(this._entries[i][0], key)) {
                return i;
            }
        }
        return -1;
    };
    
    Map.prototype._sameValueZero = function(x, y) {
        return x === y || (x !== x && y !== y);
    };
    
    Map.prototype.forEach = function(callback, thisArg) {
        for (const [key, value] of this._entries) {
            callback.call(thisArg, value, key, this);
        }
    };
    
    Map.prototype.keys = function*() {
        for (const [key] of this._entries) {
            yield key;
        }
    };
    
    Map.prototype.values = function*() {
        for (const [, value] of this._entries) {
            yield value;
        }
    };
    
    Map.prototype.entries = function*() {
        for (const entry of this._entries) {
            yield [entry[0], entry[1]];
        }
    };
    
    Map.prototype[Symbol.iterator] = Map.prototype.entries;
    
    window.Map = Map;
}

// Test
const map = new Map();
map.set('key1', 'value1');
map.set('key2', 'value2');
console.log(map.get('key1')); // "value1"
console.log(map.size); // 2
```

---

## Testing Your Polyfills

### Test Runner Template

```javascript
// Simple test framework for polyfills
function runTests() {
    const tests = [];
    
    function test(name, fn) {
        tests.push({ name, fn });
    }
    
    function assertEqual(actual, expected, message) {
        if (actual !== expected) {
            throw new Error(`${message}: expected ${expected}, got ${actual}`);
        }
    }
    
    function assertDeepEqual(actual, expected, message) {
        if (JSON.stringify(actual) !== JSON.stringify(expected)) {
            throw new Error(`${message}: expected ${JSON.stringify(expected)}, got ${JSON.stringify(actual)}`);
        }
    }
    
    // Array.prototype.map tests
    test('Array.prototype.map should transform elements', () => {
        const arr = [1, 2, 3];
        const doubled = arr.map(x => x * 2);
        assertDeepEqual(doubled, [2, 4, 6], 'map should double values');
    });
    
    // Array.prototype.filter tests
    test('Array.prototype.filter should filter elements', () => {
        const arr = [1, 2, 3, 4, 5];
        const evens = arr.filter(x => x % 2 === 0);
        assertDeepEqual(evens, [2, 4], 'filter should return even numbers');
    });
    
    // Array.prototype.reduce tests
    test('Array.prototype.reduce should accumulate values', () => {
        const arr = [1, 2, 3, 4];
        const sum = arr.reduce((acc, curr) => acc + curr, 0);
        assertEqual(sum, 10, 'reduce should sum all values');
    });
    
    // Promise tests
    test('Promise should resolve correctly', (done) => {
        const promise = new Promise(resolve => {
            setTimeout(() => resolve('success'), 10);
        });
        
        promise.then(result => {
            assertEqual(result, 'success', 'Promise should resolve with success');
            done();
        });
    });
    
    // Run all tests
    let passed = 0;
    let failed = 0;
    
    tests.forEach(({ name, fn }) => {
        try {
            if (fn.length > 0) {
                // Async test
                fn(() => {
                    console.log(`✅ ${name}`);
                    passed++;
                });
            } else {
                // Sync test
                fn();
                console.log(`✅ ${name}`);
                passed++;
            }
        } catch (error) {
            console.error(`❌ ${name}: ${error.message}`);
            failed++;
        }
    });
    
    console.log(`\nTest Results: ${passed} passed, ${failed} failed`);
}

// Run tests
runTests();
```

---

## Performance Considerations

### Optimized Polyfills

```javascript
// Optimized Array.prototype.forEach with early type checking
if (!Array.prototype.forEach) {
    Array.prototype.forEach = function(callback, thisArg) {
        'use strict';
        
        // Fast path for arrays
        if (this.constructor === Array && typeof callback === 'function') {
            for (let i = 0, len = this.length; i < len; i++) {
                if (i in this) {
                    callback.call(thisArg, this[i], i, this);
                }
            }
            return;
        }
        
        // Fallback for array-like objects
        if (this == null) {
            throw new TypeError('Array.prototype.forEach called on null or undefined');
        }
        
        if (typeof callback !== 'function') {
            throw new TypeError(callback + ' is not a function');
        }
        
        const O = Object(this);
        const len = parseInt(O.length) || 0;
        
        for (let i = 0; i < len; i++) {
            if (i in O) {
                callback.call(thisArg, O[i], i, O);
            }
        }
    };
}
```

### Feature Detection Patterns

```javascript
// Better feature detection
function hasNativeSupport(feature) {
    try {
        return typeof feature === 'function' && 
               /\[native code\]/.test(feature.toString());
    } catch (e) {
        return false;
    }
}

// Only polyfill if native support is missing
if (!hasNativeSupport(Array.prototype.includes)) {
    // Polyfill implementation here
}
```

---

## Best Practices for Writing Polyfills

### 1. **Follow the Specification**
Always implement polyfills according to official ECMAScript specifications.

### 2. **Handle Edge Cases**
```javascript
// Good: Handle all edge cases
if (!Array.prototype.myMethod) {
    Array.prototype.myMethod = function(callback, thisArg) {
        // Check for null/undefined
        if (this == null) {
            throw new TypeError('called on null or undefined');
        }
        
        // Check callback type
        if (typeof callback !== 'function') {
            throw new TypeError(callback + ' is not a function');
        }
        
        // Handle sparse arrays
        // Handle non-numeric lengths
        // Handle negative indices
    };
}
```

### 3. **Performance Optimization**
```javascript
// Use efficient algorithms
// Cache frequently accessed properties
// Minimize function calls in loops
// Use native methods when available
```

### 4. **Cross-Browser Compatibility**
```javascript
// Test in multiple browsers
// Handle browser-specific quirks
// Use feature detection, not browser detection
```

### 5. **Error Handling**
```javascript
// Throw appropriate error types
// Use descriptive error messages
// Handle unexpected input gracefully
```

---

## Summary

This comprehensive guide covers polyfills from basic to advanced levels:

**Easy Level (🟢):**
- Array methods: forEach, map, filter, indexOf
- String methods: trim
- Basic functionality with simple implementations

**Medium Level (🟡):**
- Array methods: reduce, find, includes, some, every
- Object methods: assign, keys
- Function methods: bind
- More complex logic and edge case handling

**Hard Level (🔴):**
- Promise implementation
- Array.from with iterator support
- Object.create with property descriptors
- Advanced array methods: flat, flatMap
- Complex feature detection and error handling

**Advanced Level:**
- Browser API polyfills: fetch, CustomEvent
- ES6+ features: Map, WeakMap, Set
- Performance optimizations
- Complete specification compliance

Each polyfill includes:
- Proper error handling
- Edge case coverage
- Browser compatibility notes
- Test examples
- Performance considerations

Use these polyfills to ensure your JavaScript code works across all target browsers and environments.