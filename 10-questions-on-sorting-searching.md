# JavaScript Sorting & Searching in Objects - Interview Questions

## Part 1: Sorting Questions (10 Questions)

### Q1: Sort an array of product objects by price in ascending order

**Real-time Example:**
```javascript
const products = [
  { id: 1, name: "Laptop", price: 1200 },
  { id: 2, name: "Mouse", price: 25 },
  { id: 3, name: "Keyboard", price: 75 },
  { id: 4, name: "Monitor", price: 300 }
];
```

**Answer:**
```javascript
const sortedByPrice = products.sort((a, b) => a.price - b.price);
console.log(sortedByPrice);
// Output: Mouse(25), Keyboard(75), Monitor(300), Laptop(1200)
```

---

### Q2: Sort an array of employee objects by name (alphabetically) and then by salary (descending) if names are same

**Real-time Example:**
```javascript
const employees = [
  { name: "John", salary: 50000, department: "IT" },
  { name: "Alice", salary: 60000, department: "HR" },
  { name: "John", salary: 55000, department: "Sales" },
  { name: "Bob", salary: 45000, department: "IT" }
];
```

**Answer:**
```javascript
const sortedEmployees = employees.sort((a, b) => {
  if (a.name === b.name) {
    return b.salary - a.salary; // Descending by salary
  }
  return a.name.localeCompare(b.name); // Ascending by name
});
console.log(sortedEmployees);
```

---

### Q3: Sort an array of date objects (orders) by date in descending order

**Real-time Example:**
```javascript
const orders = [
  { orderId: 101, date: "2024-03-15", amount: 250 },
  { orderId: 102, date: "2024-01-10", amount: 180 },
  { orderId: 103, date: "2024-06-22", amount: 320 },
  { orderId: 104, date: "2024-02-05", amount: 150 }
];
```

**Answer:**
```javascript
const sortedOrders = orders.sort((a, b) => {
  return new Date(b.date) - new Date(a.date);
});
console.log(sortedOrders);
// Output: Orders from newest to oldest
```

---

### Q4: Sort an array of student objects by grade (descending), and maintain original order for same grades (stable sort)

**Real-time Example:**
```javascript
const students = [
  { name: "Emma", grade: 85, rollNo: 1 },
  { name: "Liam", grade: 92, rollNo: 2 },
  { name: "Olivia", grade: 85, rollNo: 3 },
  { name: "Noah", grade: 78, rollNo: 4 }
];
```

**Answer:**
```javascript
const sortedStudents = students
  .map((student, index) => ({ ...student, originalIndex: index }))
  .sort((a, b) => {
    if (b.grade === a.grade) {
      return a.originalIndex - b.originalIndex;
    }
    return b.grade - a.grade;
  });
console.log(sortedStudents);
```

---

### Q5: Sort an array of objects by nested property (city within address)

**Real-time Example:**
```javascript
const users = [
  { name: "John", address: { city: "New York", zip: "10001" } },
  { name: "Alice", address: { city: "Chicago", zip: "60601" } },
  { name: "Bob", address: { city: "Austin", zip: "73301" } },
  { name: "Carol", address: { city: "Boston", zip: "02101" } }
];
```

**Answer:**
```javascript
const sortedByCity = users.sort((a, b) => {
  return a.address.city.localeCompare(b.address.city);
});
console.log(sortedByCity);
// Output: Austin, Boston, Chicago, New York
```

---

### Q6: Sort an array of objects by multiple criteria: status (priority order), then by date

**Real-time Example:**
```javascript
const tickets = [
  { id: 1, status: "open", date: "2024-03-10", title: "Bug fix" },
  { id: 2, status: "closed", date: "2024-03-15", title: "Feature" },
  { id: 3, status: "in-progress", date: "2024-03-08", title: "Update" },
  { id: 4, status: "open", date: "2024-03-05", title: "Enhancement" }
];
```

**Answer:**
```javascript
const statusPriority = { "open": 1, "in-progress": 2, "closed": 3 };

const sortedTickets = tickets.sort((a, b) => {
  if (statusPriority[a.status] !== statusPriority[b.status]) {
    return statusPriority[a.status] - statusPriority[b.status];
  }
  return new Date(a.date) - new Date(b.date);
});
console.log(sortedTickets);
```

---

### Q7: Sort an array of objects by string length of a property

**Real-time Example:**
```javascript
const posts = [
  { id: 1, title: "JavaScript Tips", content: "Short post" },
  { id: 2, title: "Advanced React", content: "This is a longer post with more content" },
  { id: 3, title: "CSS Grid", content: "Medium length post here" },
  { id: 4, title: "Node.js", content: "Brief" }
];
```

**Answer:**
```javascript
const sortedByContentLength = posts.sort((a, b) => {
  return a.content.length - b.content.length;
});
console.log(sortedByContentLength);
// Output: Sorted from shortest to longest content
```

---

### Q8: Sort an array of objects with null/undefined values, placing them at the end

**Real-time Example:**
```javascript
const items = [
  { name: "Item A", rating: 4.5 },
  { name: "Item B", rating: null },
  { name: "Item C", rating: 3.8 },
  { name: "Item D", rating: undefined },
  { name: "Item E", rating: 4.9 }
];
```

**Answer:**
```javascript
const sortedItems = items.sort((a, b) => {
  if (a.rating == null) return 1;
  if (b.rating == null) return -1;
  return b.rating - a.rating;
});
console.log(sortedItems);
// Output: Rated items first (high to low), then null/undefined
```

---

### Q9: Sort an array of objects by boolean property (true first, then false)

**Real-time Example:**
```javascript
const tasks = [
  { id: 1, task: "Review code", completed: false },
  { id: 2, task: "Write tests", completed: true },
  { id: 3, task: "Deploy app", completed: false },
  { id: 4, task: "Update docs", completed: true }
];
```

**Answer:**
```javascript
const sortedTasks = tasks.sort((a, b) => {
  return b.completed - a.completed;
  // Or: return (b.completed === a.completed) ? 0 : b.completed ? 1 : -1;
});
console.log(sortedTasks);
// Output: Completed tasks first, then incomplete
```

---

### Q10: Sort an array of objects by case-insensitive string property

**Real-time Example:**
```javascript
const companies = [
  { name: "apple Inc", employees: 150000 },
  { name: "Google LLC", employees: 140000 },
  { name: "Amazon.com", employees: 1500000 },
  { name: "microsoft Corp", employees: 180000 }
];
```

**Answer:**
```javascript
const sortedCompanies = companies.sort((a, b) => {
  return a.name.toLowerCase().localeCompare(b.name.toLowerCase());
});
console.log(sortedCompanies);
// Output: amazon.com, apple Inc, Google LLC, microsoft Corp
```

---

## Part 2: Searching Questions (10 Questions)

### Q11: Find a single object by ID using find()

**Real-time Example:**
```javascript
const products = [
  { id: 101, name: "Laptop", price: 1200 },
  { id: 102, name: "Mouse", price: 25 },
  { id: 103, name: "Keyboard", price: 75 }
];
```

**Answer:**
```javascript
const product = products.find(p => p.id === 102);
console.log(product);
// Output: { id: 102, name: "Mouse", price: 25 }
```

---

### Q12: Find all objects matching a condition using filter()

**Real-time Example:**
```javascript
const employees = [
  { name: "John", salary: 50000, department: "IT" },
  { name: "Alice", salary: 60000, department: "HR" },
  { name: "Bob", salary: 55000, department: "IT" },
  { name: "Carol", salary: 45000, department: "Sales" }
];
```

**Answer:**
```javascript
const itEmployees = employees.filter(emp => emp.department === "IT");
console.log(itEmployees);
// Output: John and Bob from IT department
```

---

### Q13: Search for objects with property value within a range

**Real-time Example:**
```javascript
const products = [
  { id: 1, name: "Laptop", price: 1200 },
  { id: 2, name: "Mouse", price: 25 },
  { id: 3, name: "Keyboard", price: 75 },
  { id: 4, name: "Monitor", price: 300 }
];
```

**Answer:**
```javascript
const affordableProducts = products.filter(p => p.price >= 50 && p.price <= 500);
console.log(affordableProducts);
// Output: Keyboard (75) and Monitor (300)
```

---

### Q14: Search for objects containing a substring (case-insensitive)

**Real-time Example:**
```javascript
const books = [
  { title: "JavaScript: The Good Parts", author: "Douglas Crockford" },
  { title: "Learning React", author: "Alex Banks" },
  { title: "Eloquent JavaScript", author: "Marijn Haverbeke" },
  { title: "You Don't Know JS", author: "Kyle Simpson" }
];
```

**Answer:**
```javascript
const searchTerm = "javascript";
const results = books.filter(book => 
  book.title.toLowerCase().includes(searchTerm.toLowerCase())
);
console.log(results);
// Output: Two books with "JavaScript" in title
```

---

### Q15: Find index of an object using findIndex()

**Real-time Example:**
```javascript
const cart = [
  { productId: 1, name: "Laptop", quantity: 1 },
  { productId: 2, name: "Mouse", quantity: 2 },
  { productId: 3, name: "Keyboard", quantity: 1 }
];
```

**Answer:**
```javascript
const index = cart.findIndex(item => item.productId === 2);
console.log(index); // Output: 1

// Use case: Update quantity
if (index !== -1) {
  cart[index].quantity += 1;
}
```

---

### Q16: Search in nested objects

**Real-time Example:**
```javascript
const users = [
  { name: "John", address: { city: "New York", country: "USA" } },
  { name: "Alice", address: { city: "London", country: "UK" } },
  { name: "Bob", address: { city: "Toronto", country: "Canada" } }
];
```

**Answer:**
```javascript
const usersInUSA = users.filter(user => user.address.country === "USA");
console.log(usersInUSA);
// Output: John from New York, USA
```

---

### Q17: Check if any object meets a condition using some()

**Real-time Example:**
```javascript
const orders = [
  { orderId: 1, status: "delivered", amount: 250 },
  { orderId: 2, status: "pending", amount: 180 },
  { orderId: 3, status: "shipped", amount: 320 }
];
```

**Answer:**
```javascript
const hasPendingOrders = orders.some(order => order.status === "pending");
console.log(hasPendingOrders); // Output: true

const hasExpensiveOrder = orders.some(order => order.amount > 1000);
console.log(hasExpensiveOrder); // Output: false
```

---

### Q18: Check if all objects meet a condition using every()

**Real-time Example:**
```javascript
const students = [
  { name: "Emma", grade: 85, passed: true },
  { name: "Liam", grade: 92, passed: true },
  { name: "Olivia", grade: 78, passed: true }
];
```

**Answer:**
```javascript
const allPassed = students.every(student => student.passed);
console.log(allPassed); // Output: true

const allAbove80 = students.every(student => student.grade >= 80);
console.log(allAbove80); // Output: false
```

---

### Q19: Search with multiple conditions (AND/OR logic)

**Real-time Example:**
```javascript
const products = [
  { id: 1, name: "Laptop", price: 1200, inStock: true, category: "Electronics" },
  { id: 2, name: "Mouse", price: 25, inStock: false, category: "Electronics" },
  { id: 3, name: "Desk", price: 300, inStock: true, category: "Furniture" },
  { id: 4, name: "Chair", price: 150, inStock: true, category: "Furniture" }
];
```

**Answer:**
```javascript
// AND logic: Electronics AND in stock AND under $500
const results = products.filter(p => 
  p.category === "Electronics" && p.inStock && p.price < 500
);

// OR logic: Either Electronics OR under $200
const results2 = products.filter(p => 
  p.category === "Electronics" || p.price < 200
);

console.log(results); // Empty array
console.log(results2); // Mouse, Desk, Chair
```

---

### Q20: Binary search on sorted array of objects

**Real-time Example:**
```javascript
const sortedProducts = [
  { id: 1, name: "Item A", price: 10 },
  { id: 2, name: "Item B", price: 25 },
  { id: 3, name: "Item C", price: 40 },
  { id: 4, name: "Item D", price: 55 },
  { id: 5, name: "Item E", price: 70 }
];
```

**Answer:**
```javascript
function binarySearchByPrice(arr, targetPrice) {
  let left = 0;
  let right = arr.length - 1;
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    
    if (arr[mid].price === targetPrice) {
      return arr[mid];
    }
    
    if (arr[mid].price < targetPrice) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }
  
  return null;
}

const result = binarySearchByPrice(sortedProducts, 40);
console.log(result); // Output: { id: 3, name: "Item C", price: 40 }
```

---

## Bonus Tips for Interviews

### Performance Considerations:
- **find()** stops at first match (O(n) worst case)
- **filter()** always checks all elements (O(n))
- **sort()** is O(n log n) - avoid unnecessary sorting
- **Binary search** is O(log n) but requires sorted array

### Common Gotchas:
1. **Mutating original array**: `sort()` modifies the original - use `[...array].sort()` to avoid
2. **Comparing strings**: Use `localeCompare()` for proper alphabetical sorting
3. **Null/undefined handling**: Always check for edge cases
4. **Type coercion**: Be careful with `==` vs `===` in comparisons

### Best Practices:
- Use descriptive variable names in callbacks
- Chain methods for readability: `filter().map().sort()`
- Consider using lodash for complex operations
- Write reusable search/sort functions
- Add error handling for edge cases