# JavaScript Objects - Complex Logic Questions & Answers

## Question 1: Employee Salary Analysis
**Problem:** Given an array of employee objects, find the department with the highest average salary and return an object containing the department name and average salary.

```javascript
const employees = [
  { name: "Alice", department: "Engineering", salary: 95000 },
  { name: "Bob", department: "Engineering", salary: 105000 },
  { name: "Charlie", department: "Marketing", salary: 75000 },
  { name: "David", department: "Marketing", salary: 85000 },
  { name: "Eve", department: "Sales", salary: 70000 }
];
```

**Explanation:**
This question tests your ability to:
- Use `reduce()` to aggregate data and build a complex object structure
- Group data by a common property (department)
- Calculate statistics (averages) from grouped data
- Use `Object.entries()` to iterate over object keys and values
- Track maximum values while iterating

The solution first creates a map where each department stores total salary and employee count, then calculates averages and finds the maximum.

**Answer:**
```javascript
function highestAvgSalaryDept(employees) {
  const deptMap = employees.reduce((acc, emp) => {
    if (!acc[emp.department]) {
      acc[emp.department] = { total: 0, count: 0 };
    }
    acc[emp.department].total += emp.salary;
    acc[emp.department].count += 1;
    return acc;
  }, {});
  
  let maxAvg = 0;
  let topDept = "";
  
  for (const [dept, data] of Object.entries(deptMap)) {
    const avg = data.total / data.count;
    if (avg > maxAvg) {
      maxAvg = avg;
      topDept = dept;
    }
  }
  
  return { department: topDept, averageSalary: maxAvg };
}

console.log(highestAvgSalaryDept(employees));
// Output: { department: 'Engineering', averageSalary: 100000 }
```

---

## Question 2: Deep Object Merge
**Problem:** Write a function that performs a deep merge of two objects. If both objects have the same key with object values, merge them recursively. Arrays should be concatenated.

**Explanation:**
This question focuses on:
- **Recursion**: Calling the function within itself for nested objects
- **Type checking**: Distinguishing between objects, arrays, and primitives using `typeof`, `Array.isArray()`, and null checks
- **Spread operator**: Using `...` to create shallow copies and concatenate arrays
- **Property existence**: Using `hasOwnProperty()` to safely check object properties

The challenge is handling different data types correctly. Regular objects merge recursively, arrays concatenate, and primitive values get overwritten. This is commonly used in configuration merging scenarios.

**Answer:**
```javascript
function deepMerge(obj1, obj2) {
  const result = { ...obj1 };
  
  for (const key in obj2) {
    if (obj2.hasOwnProperty(key)) {
      if (Array.isArray(obj2[key]) && Array.isArray(result[key])) {
        result[key] = [...result[key], ...obj2[key]];
      } else if (typeof obj2[key] === 'object' && obj2[key] !== null && 
                 typeof result[key] === 'object' && result[key] !== null &&
                 !Array.isArray(obj2[key])) {
        result[key] = deepMerge(result[key], obj2[key]);
      } else {
        result[key] = obj2[key];
      }
    }
  }
  
  return result;
}

const obj1 = { a: 1, b: { c: 2, d: 3 }, e: [1, 2] };
const obj2 = { b: { c: 4, f: 5 }, e: [3, 4], g: 6 };

console.log(deepMerge(obj1, obj2));
// Output: { a: 1, b: { c: 4, d: 3, f: 5 }, e: [1, 2, 3, 4], g: 6 }
```

---

## Question 3: Product Inventory Manager
**Problem:** Create a function that takes an array of product transactions (add/remove) and returns the final inventory state with product details.

```javascript
const transactions = [
  { product: "Laptop", action: "add", quantity: 5, price: 1200 },
  { product: "Mouse", action: "add", quantity: 10, price: 25 },
  { product: "Laptop", action: "remove", quantity: 2 },
  { product: "Mouse", action: "add", quantity: 5, price: 25 },
  { product: "Keyboard", action: "add", quantity: 8, price: 75 }
];
```

**Explanation:**
This simulates a real-world inventory system and teaches:
- **State management**: Building up state over time from a series of events
- **Conditional logic**: Handling different actions (add vs remove)
- **Object initialization**: Creating entries dynamically as new products appear
- **Property updates**: Maintaining both quantity and price information

The `reduce()` method processes transactions sequentially, maintaining a running inventory. This pattern is useful for processing logs, transactions, or any sequential data that builds up state.

**Answer:**
```javascript
function calculateInventory(transactions) {
  return transactions.reduce((inventory, transaction) => {
    const { product, action, quantity, price } = transaction;
    
    if (!inventory[product]) {
      inventory[product] = { quantity: 0, price: price || 0 };
    }
    
    if (action === "add") {
      inventory[product].quantity += quantity;
      if (price) inventory[product].price = price;
    } else if (action === "remove") {
      inventory[product].quantity -= quantity;
    }
    
    return inventory;
  }, {});
}

const inventory = calculateInventory(transactions);
console.log(inventory);
/* Output:
{
  Laptop: { quantity: 3, price: 1200 },
  Mouse: { quantity: 15, price: 25 },
  Keyboard: { quantity: 8, price: 75 }
}
*/
```

---

## Question 4: Nested Object Path Value Retrieval
**Problem:** Write a function that retrieves a value from a nested object using a string path (e.g., "user.address.city"). Return undefined if the path doesn't exist.

**Explanation:**
This question demonstrates:
- **String manipulation**: Using `split()` to break path into segments
- **Array reduction**: Using `reduce()` for sequential access
- **Optional chaining**: Using `?.` operator to safely access properties without errors
- **Null handling**: Gracefully returning undefined when path doesn't exist

This pattern is extremely useful when working with API responses or deeply nested configuration objects. The optional chaining (`?.`) prevents errors when intermediate properties don't exist, making the code safe and concise.

**Answer:**
```javascript
function getNestedValue(obj, path) {
  return path.split('.').reduce((current, key) => {
    return current?.[key];
  }, obj);
}

const data = {
  user: {
    name: "John",
    address: {
      city: "New York",
      zipCode: "10001"
    }
  }
};

console.log(getNestedValue(data, "user.address.city")); // "New York"
console.log(getNestedValue(data, "user.phone")); // undefined
console.log(getNestedValue(data, "user.address.country")); // undefined
```

---

## Question 5: Group and Transform Array of Objects
**Problem:** Given an array of student objects, group them by grade, calculate the average score for each grade, and return only grades with an average above 75.

```javascript
const students = [
  { name: "Alice", grade: "A", score: 92 },
  { name: "Bob", grade: "B", score: 78 },
  { name: "Charlie", grade: "A", score: 88 },
  { name: "David", grade: "C", score: 65 },
  { name: "Eve", grade: "B", score: 82 },
  { name: "Frank", grade: "A", score: 95 }
];
```

**Explanation:**
This is a multi-step data transformation that practices:
- **Grouping**: Using `reduce()` to organize data by a category
- **Aggregation**: Calculating totals and counts simultaneously
- **Transformation**: Converting from one object structure to another using `map()`
- **Filtering**: Applying conditions to select specific results
- **Sorting**: Ordering results by calculated values

This pipeline approach (group → transform → filter → sort) is common in data analysis and resembles SQL operations. It shows how JavaScript can handle complex data processing efficiently.

**Answer:**
```javascript
function analyzeGrades(students) {
  const gradeStats = students.reduce((acc, student) => {
    if (!acc[student.grade]) {
      acc[student.grade] = { total: 0, count: 0, students: [] };
    }
    acc[student.grade].total += student.score;
    acc[student.grade].count += 1;
    acc[student.grade].students.push(student.name);
    return acc;
  }, {});
  
  return Object.entries(gradeStats)
    .map(([grade, data]) => ({
      grade,
      averageScore: data.total / data.count,
      studentCount: data.count,
      students: data.students
    }))
    .filter(item => item.averageScore > 75)
    .sort((a, b) => b.averageScore - a.averageScore);
}

console.log(analyzeGrades(students));
/* Output:
[
  { grade: 'A', averageScore: 91.67, studentCount: 3, students: ['Alice', 'Charlie', 'Frank'] },
  { grade: 'B', averageScore: 80, studentCount: 2, students: ['Bob', 'Eve'] }
]
*/
```

---

## Question 6: Object Difference Finder
**Problem:** Create a function that compares two objects and returns an object containing only the keys where values differ, along with both values.

**Explanation:**
This question covers:
- **Set operations**: Using `Set` to combine keys from both objects (union)
- **Object comparison**: Checking value equality with strict equality (`!==`)
- **Dynamic object building**: Creating result objects on the fly
- **Conditional returns**: Returning null when no differences exist

This is practical for change detection systems, audit logs, or form validation. The use of `Set` ensures we check all keys from both objects, not missing any that exist in one but not the other.

**Answer:**
```javascript
function findDifferences(obj1, obj2) {
  const differences = {};
  const allKeys = new Set([...Object.keys(obj1), ...Object.keys(obj2)]);
  
  for (const key of allKeys) {
    if (obj1[key] !== obj2[key]) {
      differences[key] = {
        original: obj1[key],
        updated: obj2[key]
      };
    }
  }
  
  return Object.keys(differences).length > 0 ? differences : null;
}

const original = { name: "John", age: 30, city: "NYC", role: "Developer" };
const updated = { name: "John", age: 31, city: "LA", role: "Developer" };

console.log(findDifferences(original, updated));
/* Output:
{
  age: { original: 30, updated: 31 },
  city: { original: 'NYC', updated: 'LA' }
}
*/
```

---

## Question 7: Shopping Cart Total Calculator
**Problem:** Calculate the total cost of items in a shopping cart, applying discounts based on quantity and category. If quantity > 5, apply 10% discount. Electronics get additional 5% off.

```javascript
const cart = [
  { name: "Laptop", category: "Electronics", price: 1000, quantity: 2 },
  { name: "Mouse", category: "Electronics", price: 50, quantity: 6 },
  { name: "Book", category: "Media", price: 20, quantity: 8 },
  { name: "Pen", category: "Stationery", price: 2, quantity: 10 }
];
```

**Explanation:**
This simulates e-commerce logic and teaches:
- **Business rules**: Implementing multiple discount conditions
- **Cumulative calculations**: Stacking discounts (quantity + category)
- **Detailed tracking**: Maintaining item-level and total-level summaries
- **Financial calculations**: Working with percentages and decimals

The challenge is applying multiple discount rules correctly and maintaining both summary data (total, total discount) and detailed breakdowns (per-item calculations). This pattern is common in billing systems, pricing engines, and checkout flows.

**Answer:**
```javascript
function calculateCartTotal(cart) {
  return cart.reduce((summary, item) => {
    let itemTotal = item.price * item.quantity;
    let discount = 0;
    
    // Quantity discount
    if (item.quantity > 5) {
      discount += itemTotal * 0.10;
    }
    
    // Category discount
    if (item.category === "Electronics") {
      discount += itemTotal * 0.05;
    }
    
    const finalPrice = itemTotal - discount;
    
    summary.items.push({
      name: item.name,
      subtotal: itemTotal,
      discount: discount,
      finalPrice: finalPrice
    });
    
    summary.total += finalPrice;
    summary.totalDiscount += discount;
    
    return summary;
  }, { items: [], total: 0, totalDiscount: 0 });
}

console.log(calculateCartTotal(cart));
/* Output:
{
  items: [
    { name: 'Laptop', subtotal: 2000, discount: 100, finalPrice: 1900 },
    { name: 'Mouse', subtotal: 300, discount: 45, finalPrice: 255 },
    { name: 'Book', subtotal: 160, discount: 16, finalPrice: 144 },
    { name: 'Pen', subtotal: 20, discount: 2, finalPrice: 18 }
  ],
  total: 2317,
  totalDiscount: 163
}
*/
```

---

## Question 8: Flatten Nested Object
**Problem:** Write a function that flattens a nested object into a single-level object with dot-notation keys.

**Explanation:**
This demonstrates:
- **Recursion**: Function calling itself for nested structures
- **String concatenation**: Building dot-notation paths progressively
- **Type discrimination**: Identifying objects vs arrays vs primitives
- **Reference passing**: Using a shared result object across recursive calls

Flattening is useful for database storage, CSV exports, or form field mapping. The recursive approach elegantly handles arbitrary nesting depth. The key insight is building the path string as you recurse deeper (`user` → `user.contact` → `user.contact.email`).

**Answer:**
```javascript
function flattenObject(obj, prefix = '', result = {}) {
  for (const key in obj) {
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

const nested = {
  user: {
    name: "John",
    contact: {
      email: "john@example.com",
      phone: "123-456-7890"
    }
  },
  active: true
};

console.log(flattenObject(nested));
/* Output:
{
  'user.name': 'John',
  'user.contact.email': 'john@example.com',
  'user.contact.phone': '123-456-7890',
  'active': true
}
*/
```

---

## Question 9: Meeting Room Scheduler
**Problem:** Given an array of meeting objects with start and end times, find all time slots where rooms are available. Assume working hours are 9 AM to 5 PM.

```javascript
const meetings = [
  { title: "Standup", start: 9, end: 10 },
  { title: "Review", start: 11, end: 12 },
  { title: "Planning", start: 14, end: 16 }
];
```

**Explanation:**
This is an interval/scheduling problem that covers:
- **Sorting**: Ordering meetings chronologically before processing
- **Gap detection**: Finding spaces between consecutive intervals
- **Boundary handling**: Checking slots before first and after last meeting
- **Time arithmetic**: Calculating durations from start/end times

This algorithm is fundamental in calendar applications, resource scheduling, and interval merging problems. The key is to sort first, then check three types of gaps: before all meetings, between consecutive meetings, and after all meetings.

**Answer:**
```javascript
function findAvailableSlots(meetings, workStart = 9, workEnd = 17) {
  // Sort meetings by start time
  const sorted = meetings.sort((a, b) => a.start - b.start);
  const available = [];
  
  // Check slot before first meeting
  if (sorted[0].start > workStart) {
    available.push({ start: workStart, end: sorted[0].start });
  }
  
  // Check slots between meetings
  for (let i = 0; i < sorted.length - 1; i++) {
    if (sorted[i].end < sorted[i + 1].start) {
      available.push({ 
        start: sorted[i].end, 
        end: sorted[i + 1].start 
      });
    }
  }
  
  // Check slot after last meeting
  const lastMeeting = sorted[sorted.length - 1];
  if (lastMeeting.end < workEnd) {
    available.push({ start: lastMeeting.end, end: workEnd });
  }
  
  return available.map(slot => ({
    ...slot,
    duration: slot.end - slot.start
  }));
}

console.log(findAvailableSlots(meetings));
/* Output:
[
  { start: 10, end: 11, duration: 1 },
  { start: 12, end: 14, duration: 2 },
  { start: 16, end: 17, duration: 1 }
]
*/
```

---

## Question 10: Social Network Friend Suggestions
**Problem:** Given a user object and an array of all users with their friends, suggest potential friends (friends of friends who aren't already friends).

```javascript
const users = [
  { id: 1, name: "Alice", friends: [2, 3] },
  { id: 2, name: "Bob", friends: [1, 4, 5] },
  { id: 3, name: "Charlie", friends: [1, 5] },
  { id: 4, name: "David", friends: [2] },
  { id: 5, name: "Eve", friends: [2, 3] }
];
```

**Explanation:**
This is a graph traversal problem teaching:
- **Graph representation**: Using adjacency lists (friend arrays)
- **Two-hop traversal**: Going from friends → friends of friends
- **Deduplication**: Tracking candidates and counting mutual connections
- **Filtering logic**: Excluding self and existing friends
- **Ranking**: Sorting suggestions by number of mutual friends

This algorithm powers "People You May Know" features on social platforms. The challenge is iterating through friends' friends while avoiding duplicates and invalid suggestions (self, existing friends). Counting mutual connections provides a quality metric for ranking suggestions.

**Answer:**
```javascript
function suggestFriends(userId, users) {
  const user = users.find(u => u.id === userId);
  if (!user) return [];
  
  const suggestions = {};
  
  // Find friends of friends
  user.friends.forEach(friendId => {
    const friend = users.find(u => u.id === friendId);
    if (friend) {
      friend.friends.forEach(fofId => {
        // Not self, not already a friend, and not already suggested
        if (fofId !== userId && !user.friends.includes(fofId)) {
          if (!suggestions[fofId]) {
            suggestions[fofId] = { 
              user: users.find(u => u.id === fofId),
              mutualFriends: 0,
              mutualFriendIds: []
            };
          }
          suggestions[fofId].mutualFriends += 1;
          suggestions[fofId].mutualFriendIds.push(friendId);
        }
      });
    }
  });
  
  return Object.values(suggestions)
    .map(s => ({
      id: s.user.id,
      name: s.user.name,
      mutualFriends: s.mutualFriends,
      mutualWith: s.mutualFriendIds.map(id => users.find(u => u.id === id).name)
    }))
    .sort((a, b) => b.mutualFriends - a.mutualFriends);
}

console.log(suggestFriends(1, users));
/* Output:
[
  { id: 5, name: 'Eve', mutualFriends: 2, mutualWith: ['Bob', 'Charlie'] },
  { id: 4, name: 'David', mutualFriends: 1, mutualWith: ['Bob'] }
]
*/
```

---

## Summary
These questions cover various complex object manipulation scenarios including:
- Array reduction and aggregation
- Deep object operations (merge, flatten, path access)
- Business logic implementation (inventory, shopping cart, scheduling)
- Graph-like data structures (social networks)
- Object comparison and transformation

Practice these to master JavaScript object manipulation and logical computation!