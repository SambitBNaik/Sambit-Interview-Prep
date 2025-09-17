# 50 JavaScript Object Problems with Answers

---

## 🔹 Basics of Objects (Creation & Access)

**1. Create an object `person` with keys `name`, `age`, and `city`. Access each property.**

```js
const person = { name: "John", age: 25, city: "New York" };
console.log(person.name); // John
console.log(person.age);  // 25
console.log(person.city); // New York
```

**2. Add a new property `gender` to the `person` object dynamically.**

```js
person.gender = "Male";
```

**3. Delete the `city` property from the `person` object.**

```js
delete person.city;
```

**4. Check if the key `age` exists in `person`.**

```js
console.log("age" in person); // true
```

**5. Iterate through all keys of an object using `for...in`.**

```js
for (let key in person) {
  console.log(key, person[key]);
}
```

**6. Clone an object using spread operator (`...`).**

```js
const clone = { ...person };
clone.name = "Mike";
console.log(person.name); // John
console.log(clone.name);  // Mike
```

**7. Merge two objects `{a:1}` and `{b:2}`.**

```js
const obj1 = { a: 1 }, obj2 = { b: 2 };
const merged = { ...obj1, ...obj2 };
```

**8. Convert an object into an array of key-value pairs.**

```js
Object.entries(person);
```

**9. Convert an array of key-value pairs back into an object.**

```js
Object.fromEntries([["name", "John"], ["age", 25]]);
```

**10. Freeze an object.**

```js
Object.freeze(person);
person.age = 30; // Won’t change
```

---

## 🔹 Object Methods & `this`

**11. Object method using `this`.**

```js
const user = {
  name: "Alice",
  greet() { console.log(`Hello, my name is ${this.name}`); }
};
user.greet();
```

**12. Regular vs arrow function `this`.**

```js
const obj = {
  name: "Bob",
  reg: function() { console.log(this.name); },
  arrow: () => console.log(this.name)
};
obj.reg();   // Bob
obj.arrow(); // undefined
```

**13. Use `Object.keys()`.**

```js
Object.keys(user); // ["name", "greet"]
```

**14. Use `Object.values()`.**

```js
Object.values(user);
```

**15. Use `Object.entries()`.**

```js
Object.entries(user);
```

**16. Function counting properties.**

```js
function countProps(obj) {
  return Object.keys(obj).length;
}
```

**17. `this` in strict vs non-strict.**

```js
function test() { console.log(this); }
test(); // window (non-strict), undefined (strict)
```

**18. Object assignment reference.**

```js
const a = { x: 1 };
const b = a;
b.x = 2;
console.log(a.x); // 2
```

**19. Use `Object.assign()`.**

```js
const target = {};
Object.assign(target, { a: 1 }, { b: 2 });
```

**20. Use `Object.seal()`.**

```js
const car = { brand: "BMW" };
Object.seal(car);
car.color = "red"; // won’t add
```

---

## 🔹 Prototypes & Inheritance

**21. Constructor function `Car`.**

```js
function Car(brand, model) {
  this.brand = brand;
  this.model = model;
}
```

**22. Add method to `Car.prototype`.**

```js
Car.prototype.drive = function() { console.log(`${this.brand} driving`); };
```

**23. Create object and call method.**

```js
const c = new Car("Toyota", "Corolla");
c.drive();
```

**24. Prototype chaining.**

```js
const parent = { greet: () => console.log("hi") };
const child = Object.create(parent);
child.greet();
```

**25. Difference `__proto__` vs `prototype`.**

* `prototype`: property of constructor function (used for inheritance).
* `__proto__`: reference inside instance pointing to its prototype.

**26. ES5 inheritance.**

```js
function Animal(name) { this.name = name; }
Animal.prototype.speak = function() { console.log("sound"); };
function Dog(name) { Animal.call(this, name); }
Dog.prototype = Object.create(Animal.prototype);
```

**27. ES6 class inheritance.**

```js
class Animal { speak() { console.log("sound"); } }
class Dog extends Animal { }
```

**28. Override method.**

```js
class Dog extends Animal {
  speak() { console.log("bark"); }
}
```

**29. Static method.**

```js
class MathUtil {
  static add(a,b) { return a+b; }
}
MathUtil.add(2,3);
```

**30. Overriding vs overloading.**

* JS supports **overriding** but not method overloading.

---

## 🔹 Advanced Object Topics

**31. Use `Object.create()`.**

```js
const proto = { greet: () => console.log("hi") };
const obj3 = Object.create(proto);
obj3.greet();
```

**32. `Object.defineProperty()`.**

```js
const obj4 = {};
Object.defineProperty(obj4, "name", { value: "Tom", writable: false });
```

**33. Getter and Setter.**

```js
const user2 = {
  firstName: "Sam",
  get name() { return this.firstName; },
  set name(val) { this.firstName = val; }
};
```

**34. Computed property.**

```js
let key = "age";
const obj5 = { [key]: 30 };
```

**35. Symbols as keys.**

```js
const sym = Symbol("id");
const obj6 = { [sym]: 123 };
```

**36. Deep compare.**

```js
function isEqual(a, b) {
  return JSON.stringify(a) === JSON.stringify(b);
}
```

**37. Check empty object.**

```js
Object.keys({}).length === 0;
```

**38. Shallow vs deep copy.**

* Shallow: `{...obj}`
* Deep: `JSON.parse(JSON.stringify(obj))`

**39. Deep clone.**

```js
function deepClone(obj) {
  return JSON.parse(JSON.stringify(obj));
}
```

**40. Safe nested access.**

```js
const nested = { a: { b: { c: 5 } } };
console.log(nested?.a?.b?.c);
```

---

## 🔹 Practical Use Cases

**41. Group objects by property.**

```js
const students = [
  { name: "A", grade: "X" },
  { name: "B", grade: "Y" },
  { name: "C", grade: "X" }
];
const grouped = students.reduce((acc, s) => {
  (acc[s.grade] = acc[s.grade] || []).push(s);
  return acc;
}, {});
```

**42. Sort objects by key.**

```js
students.sort((a,b) => a.name.localeCompare(b.name));
```

**43. Remove duplicates by key.**

```js
const unique = [...new Map(students.map(s => [s.name, s])).values()];
```

**44. Convert array to object with key.**

```js
const obj7 = Object.fromEntries(students.map(s => [s.name, s]));
```

**45. Flatten nested object.**

```js
function flatten(obj, parent = "", res = {}) {
  for (let key in obj) {
    const prop = parent ? parent + "." + key : key;
    if (typeof obj[key] === "object") flatten(obj[key], prop, res);
    else res[prop] = obj[key];
  }
  return res;
}
```

**46. Unflatten object.**

```js
function unflatten(data) {
  const result = {};
  for (let i in data) {
    const keys = i.split(".");
    keys.reduce((acc, k, idx) =>
      acc[k] = idx === keys.length-1 ? data[i] : acc[k] || {}, result);
  }
  return result;
}
```

**47. Compare two objects for differences.**

```js
function diff(obj1, obj2) {
  const res = {};
  for (let key in obj1) {
    if (obj1[key] !== obj2[key]) res[key] = [obj1[key], obj2[key]];
  }
  return res;
}
```

**48. Deep freeze object.**

```js
function deepFreeze(obj) {
  Object.freeze(obj);
  for (let key in obj) {
    if (typeof obj[key] === "object") deepFreeze(obj[key]);
  }
}
```

**49. Serialize and parse JSON.**

```js
const str = JSON.stringify(person);
const back = JSON.parse(str);
```

**50. Why `JSON.stringify({a:undefined})` gives `{}`?**

* Because `undefined` values are **ignored** during JSON serialization.

---
